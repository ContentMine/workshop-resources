# ami
==============================

## Table of Content

1. [Description](#description) 
1. [Preparations](#preparations)
1. [Data Input](#data-input)
1. [How to use ami-plugins](#how-to-use-ami-plugins)
  1. [ami2-species](#ami2-species)
  1. [ami2-gene](#ami2-gene)
  1. [ami2-sequence](#ami2-sequence)
  1. [ami2-regex](#ami2-regex)
  1. [ami2-word](#ami2-word)
1. [Summary](#summary)
1. [Next Steps](#next-steps)
1. [Further Materials](#further-materials)

## Description

**What does ami?**

Ami is a collection of plugins that search, index and extract pieces of information from structured documents. So this is where the facts get extracted into XML files, so they can be used for further usage.

**Why do we need ami?**

Ami offers a plugin-style architecture with some powerful modules to extract facts like genes, species and sequences and to use regular expressions. We are continously developing new plugins to cover different types of encoded knowledge.

**How can I use ami?**

Ami is to be used after the normalization of papers happened. You take the output of [norma](../norma/README.md) and run individual plugins.

**What you will learn here**

This tutorial shows you:
- how to use the species plugin
- how to use the genes plugin
- how to use the sequences plugin
- how to use regular expressions
- what to do with the extracted facts

**How to use the tutorial**

We have some conventions at work, which will be used through-out the tutorial. 
- Variables as placeholders are always caps, like NAME, YOURDIRECTORY etc.


**Glossary**



## Preparations
### Pre-Requisites

### Used Software
- [Future TDM Virtual Machine](LINK)
- ami2-species
- ami2-gene
- ami2-sequence
- ami2-regex

### Installation

On the ContentMine-VM ami is already provided. If you want to install it locally, you have to build it from source. For this you need `git`, `maven` and `maven3`.

```bash
git clone https://github.com/ContentMine/ami-plugin.git
cd ami-plugin
mvn clean install
```

You can find the technical documentation for `ami` in its [repository](https://github.com/ContentMine/ami-plugin).

## Input data

The input for ami is always a [CProject](../cproject) containing documents in [scholarly HTML](../sHTML). ami then applies one of the available plugins, extracts the relevant content from the sHTML, and attaches the results to the paper.

## How to use ami-plugins

ami-plugins require `scholarly.html`-files as input. Please follow the instructions for [norma](../norma/norma-tutorial.md). Your project directory should look like this:

```bash
tree ursus
ursus/
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   └── scholarly.html
├── PMC3893247
│   ├── fulltext.xml
│   └── scholarly.html
├── PMC3898307
│   ├── fulltext.xml
│   └── scholarly.html
...
```

### a) ami2-species

You can then search for all occurences of a species name with 
```bash
ami2-species -q CPROJECTFOLDER -i FILETYPE --sp.species --sp.type SPECIESTYPE
```

You have to choose between three different types of species terms for ```SPECIESTYPE``` 
- ```genus```, which will extract terms like *Brachiosaurus* [more details](https://en.wikipedia.org/wiki/Genus)
- ```binomial```, which will extract terms like *B. altithorax* [more details](https://en.wikipedia.org/wiki/Binomial_nomenclature)
- or ```genussp```, whick will extract terms like *Bacillus sp* or *Ursus spp.*

ami will print out all matches while searching.

```bash
ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type genus
ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type binomial
ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type genussp
```

The results will all be saved as XML in the corresponding CTree inside results/species with an own folder named after the SPECIESTYPE.

```
tree dinosaurs-xmls
dinosaurs-xmls
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   ├── results
│   │   └── species
│   │       ├── **binomial**
│   │       │   └── results.xml
│   │       ├── **genus**
│   │       │   └── results.xml
│   │       └── **genussp**
│   │           └── results.xml
│   └── scholarly.html
├── PMC3893247
│   ├── fulltext.xml
│   ├── results
│   │   └── species
│   │       ├── **binomial**
│   │       │   └── results.xml
│   │       ├── **genus**
│   │       │   └── results.xml
│   │       └── **genussp**
│   │           └── results.xml
│   └── scholarly.html
...
```

```bash
cat dinosaurs-xmls/PMC4349051/results/species/genussp/results.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="genussp">
 <result pre="t very strongly arched, as is the case in adult lambeosaurines and unlike young individuals (e.g., " exact="Parasaurolophus sp" match="Parasaurolophus sp" post=". RAM 14000). These different angles are possibly a consequence of more strongly arched frontals in" name="genussp"/>
 <result pre=" and 1/240; Saveliev, Alifanov &amp;amp;amp; Bolotsky, 2012; Lauters et al., 2013) and the subadult of " exact="Corythosaurus sp" match="Corythosaurus sp" post=". (CMN 34825; Evans, Ridgely &amp;amp;amp; Witmer, 2009). The olfactory bulbs are turned downward with " name="genussp"/>
 <result pre="ampullae, the lateral ampulla is larger than the posterior ampulla and the anterior ampulla, as in " exact="Parasaurolophus sp" match="Parasaurolophus sp" post=". RAM 14000 ( Farke et al., 2013) and unlike in Hypacrosaurus altispinus ROM 702 and Lamb" name="genussp"/>
 <result pre=" sp. RAM 14000 ( Farke et al., 2013) and unlike in Hypacrosaurus altispinus ROM 702 and " exact="Lambeosaurus sp" match="Lambeosaurus sp" post=". ROM 758 ( Evans, Ridgely &amp;amp;amp; Witmer, 2009), where the anterior ampulla is the largest, foll" name="genussp"/>
</results>
```

The results.xml consists of different amounts of lines, where every line represents one extracted fact inside the `<result>`-tag and consists of:
-  `exact`: the exact match - the fact. e. g. `exact="Parasaurolophus sp" for the fact "Parasaurolophus sp"
- `pre`: 99 characters before the match
- `post`: 99 characters after the match

Down to earth, this is what the fact extraction looks like in the end: a list of terms extracted from the literature with XX characters before and afterwards as context around the fact.


### b) ami2-gene

The search for genes works in the same way, just with another command: 
```bash
ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type GENETYPE
```

At the moment there is only one GENETYPE available (`human`). Results are again stored within the CTree.

```bash
ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type human
```

This then creates a folder gene/human inside results next to the results of the species module.

```
tree dinosaurs-xmls
dinosaurs-xmls
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   ├── results
│   │   └── gene
│   │       └── human
│   │           └── results.xml
│   └── scholarly.html
├── PMC3893247
│   ├── fulltext.xml
│   ├── results
│   │   └── gene
│   │       └── human
│   │           └── results.xml
│   └── scholarly.html
...
```

The `results.xml` is, as always, the same structure: a results-tag with pre, post, and exact attributes.

```
cat dinosaurs-xmls/PMC4454486/results/gene/human/results.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="human">
 <result pre=" individual vertebrae in Cretoxyrhina mantelli. The relationship between centrum diameter ( " exact="CD" post=" in mm) and total length ( TL) can be estimated with the following formula: " name="human"/>
 <result pre="hina mantelli. The relationship between centrum diameter ( CD in mm) and total length ( " exact="TL" post=") can be estimated with the following formula: " name="human"/>
</results>
```

### c) ami2-sequence

The search for sequences follows the same structure: 
```bash
ami2-sequence -q dinosaurs-xmls/ -i scholarly.html --sq.sequence --sq.type SEQUENCETYPE
```

- SEQUENCETYPE is one of `dna rna prot prot3 carb3`. 

```bash
ami2-sequence -q dinosaurs-xmls/ -i scholarly.html --sq.sequence --sq.type rna
```

Again, this creates an own folder called sequence/rna inside results. The results are in general of the same structure as before, with an additional attribute `xpath` that shows the location of the match within the html-structure of the `scholarly.html`.

```bash
cat dinosaurs-xmls/PMC4447998/results/sequence/rna/results.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="rna">
 <result pre="the oligonucleotides, within NR3C1 (GenBank #AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTT" exact="AGAGGG" post="-3′ and NR3HumR: 5′-biotin-7-CCCCCAACTCCCCAAAAA-3′ (adapted from Oberlander et al., 2008). Amplific" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
 <result pre="(GenBank #AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTTAGAGGG-3′ and NR3HumR: 5′-biotin-7-" exact="CCCCCAAC" post="TCCCCAAAAA-3′ (adapted from Oberlander et al., 2008). Amplification resulted in a 403 bp fragment (" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
 <result pre="#AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTTAGAGGG-3′ and NR3HumR: 5′-biotin-7-CCCCCAACT" exact="CCCCAAAAA" post="-3′ (adapted from Oberlander et al., 2008). Amplification resulted in a 403 bp fragment (position-3" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
</results>
```

### d) ami2-regex

**Now, what is a regex?** 

Regex is the shortcut for ([regular expression](https://en.wikipedia.org/wiki/Regular_expression)) and is used to match search patterns inside text. This means to search for such basic strings like "Dinosaur" inside a sentence, but also allows way more complex patterns to look for, like a string where the first two characters are numbers, followed by 3 big letters and finished by a dot. `[Dd]inosaur[s]?` matches for example upper and lower case words in singular and plural form, so you look at the same time for Dinosaur, dinosaur, Dinosaurs, and dinosaurs. Do you see the potential for formalizing searches?

Digits can be added by e.g. `[0-9]`, which matches any digit, or by fixed sequences `(00111001)`. Non-alphanumeric characters have to be escaped by `\`, so if you want to search for the number 3.14 explicitly, the regex looks like `(3\.14)`. ADD LINK TO A TUTORIAL AND IN DETAIL DESCRIPTION.

We will now see a variety of regex-s and how they are used in a XML file.

**How to use regex with ami**


```bash
ami2-regex -q dinosaurs-xml -i scholarly.html --context PRE POST --r.regex REGEXFILE.xml
```. 

- ```PRE```: tells ami how many characters before a match should be captured
- ```POST```: tells ami how many characters after a match should be captured
- ```REGEXFILE```: target location of your regex XML file. It contains all regex-queries.

**Create regex XML file**


To create a regex XML file, you have to use the following guidelines:


It always needs to be wrapped by opening tags ```<compoundRegex title="TITLE">``` and closing tags ```</compoundRegex>```, which are the opening and closing tags. The ```TITLE``` sets the name of the folder, where the output gets stored and can be of any type.

```xml
<compoundRegex title="dinosaurfood">
</compoundRegex>
```

In the regex XML each regex-query is written to a new line, and consists of the opening and closing tags `<regex></regex>`. Within the opening tag there must be two attributes declared, 
- ```weight```: the relative importance given to each match (influences indexing engines). Normally this is set to the default value `1.0` 
- ```fields```: corresponds to the regex-query, and specifies the name of the query

If you take "food" from line two of the following regex XML, one variable named "food" is expected within a match. If you take "predator" from line three, you specify the name, but your query should also be returning two variables within a match.
```xml
<compoundRegex title="dinosaurfood">
<regex weight="1.0" fields="food"></regex>
<regex weight="1.0" fields="predator"></regex>
</compoundRegex>
```

What is missing now is the regex-query itself. A query itself is placed between the regex-tags `<regex>query</regex>`and is framed by round brackets `()`. 

In line two one field ("food") is defined. We want to get both upper and lower cases, and `[Ff]` matches either `F` or `f`: `([Ff]ood)`. The following characters `ood` are fixed for this query, they have to be matched. 

For the second query, we want to find all mentions of "predator regime/s". For this we need `\s`, a special character standing for ` ` - the whitespace, blank character. The questions mark `[s]?` makes the "s" optional: `([Pp]redator\sregime[s]?)`

```xml
<compoundRegex title="dinosaurfood">
<regex weight="1.0" fields="food">([Ff]ood)</regex>
<regex weight="1.0" fields="predator">([Pp]redator\sregime[s]?)</regex>
</compoundRegex>
```

We now construct a ```regex.xml``` like that (use any texteditor for that) and place this XML in our project folder as `dinosaurfood.xml`. We run ami with it, and because we want to get some context around our matches, add the ```PRE``` and ```POST``` option, which capture characters before and after the match.

```bash
ami2-regex -q dinosaurs-xmls/ -i scholarly.html --r.regex dinosaurs-xmls/dinosaurfood.xml --context 50 50
```

```bash
cat dinosaurs-xmls-regex/PMC4298445/results/regex/dinosaurfood/results.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="dinosaurfood">
 <result pre="rs of Darwin's finches entered a lard of abundant " name0="food" value0="food" post="and varied living quarters, unmarred by the presen" xpath="/html[1]/body[1]/div[1]/div[4]/p[2]"/>
 <result pre="amas mosquitofish populations occupying different " name0="predator" value0="predator regimes" post="causes premating reproductive isolation due to sex" xpath="/html[1]/body[1]/div[1]/div[10]/div[1]/div[1]/p[4]"/>
</results>
```

The output contains 50 characters `pre` and 50 characters `post` the `value0`, as well as the `xpath` of the match in the scholarly.html.


### e) ami2-word

Word frequency is one of the most valuable tools for catagorising documents.

(This uses an example where the search query was `microRNA` rather than dinosaurs)

The simplest approach is to count the words in document/s or chunks of document/s. 

```bash
ami2-word --w.words wordFrequencies -q dinosaurs-xmls/ -i scholarly.html
```
creates
```
eupmc
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC2275095
│   ├── fulltext.xml
│   ├── results
│   │   └── word
│   │       └── frequencies
│   │           ├── results.html
│   │           └── results.xml
│   └── scholarly.html
├── PMC2586803
│   ├── fulltext.xml
│   ├── results
│   │   └── word
│   │       └── frequencies
│   │           ├── results.html
│   │           └── results.xml
```

The first lists word frequencies as:

```
<?xml version="1.0" encoding="UTF-8"?>
<results title="frequencies">
 <result title="frequency" word="the" count="233"/>
 <result title="frequency" word="and" count="188"/>
 <result title="frequency" word="miR-" count="99"/>
 <result title="frequency" word="were" count="77"/>
 <result title="frequency" word="with" count="68"/>
 <result title="frequency" word="for" count="57"/>
 <result title="frequency" word="The" count="50"/>
 <result title="frequency" word="was" count="49"/>
```

and the second creates a "Word Cloud"-like HTML display with the most frequent words in order and with fonts proportional to the count.

Clearly this mainly reflects the frequency in the English language, so we can remove the commonest words by creating <em>stopwords</em>. 

```bash
ami2-word --w.words wordFrequencies --project eupmc --w.stopwords /org/xmlcml/ami2/plugins/word/stopwords.txt
```

gives `results.xml` as
```
<?xml version="1.0" encoding="UTF-8"?>
<results title="frequencies">
 <result title="frequency" word="miR-" count="99"/>
 <result title="frequency" word="LNA-antimiR" count="49"/>
 <result title="frequency" word="mice" count="39"/>
 <result title="frequency" word="liver" count="35"/>
 <result title="frequency" word="using" count="32"/>
 <result title="frequency" word="Figure" count="28"/>
 <result title="frequency" word="levels" count="28"/>
 <result title="frequency" word="control" count="28"/>
 <result title="frequency" word="expression" count="25"/>
 <result title="frequency" word="miRNA" count="24"/>
```

clearly there is a strong signal now

We  have a range of stopword files in different languages. It is also possible to create your own files and add them:

```bash
ami2-word --w.words wordFrequencies --project eupmc --w.stopwords /org/xmlcml/ami2/plugins/word/stopwords.txt mydir/myfile.txt
```

The format is a simple list of words:
```
a
about
above
across
after
afterwards
again
against
```

and the file can be referenced either through a URL format or relative/absolute filename.

## Summary

* A project folder containing ctrees is always the input.
* Plugins are own software parts with own commands.
* Rsults/Facts are stored within the ctree in a plugin-specific folder.
* Facts also store the context of 99 characters before and after the fact.

**Next steps**

* Continue to  for the next step of the ContentMine pipeline.


## Next tutorial

To find out more of how we index the facts and make them usable for others, look at the [cat tutorial](../cat/cat-tutorial.md).

## Further material

**ContentMine**
- [contentmine.org](http://contentmine.org)
- office ett contentmine dot org
- [@TheContentMine](http://twitter.com/thecontentmine)

**Slides**


**Assets**

**Videos**


**Learning Materials**

**www**


**Papers**




