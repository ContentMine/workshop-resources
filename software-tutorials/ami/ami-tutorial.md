![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is ami?

ami is a collection of plugins that search, index and extract pieces of information from structured documents. A piece of information can be a mention of a certain species (e.g. Brachiosaurus) in a text, a gene sequence, or an expression of a chemical compound. The type of relevant information varies from discipline to discipline. We are continously developing new plugins to cover different types of encoded knowledge.

The input for ami is always a ctree containing documents in [scholarly HTML](../sHTML/sHTML-overview.md). ami then applies one of the available plugins, extracts the relevant content from the sHTML, and stores results in a folder in the ctree.


[1. Installation](#installation)

[2. species](#ami2-species)

[3. genes](#ami2-gene)

[4. sequences](#ami2-sequences)

[5. regex](#ami2-regex)

[6. Summary and next steps](#summary-and-next-steps)


### Installation

ami can be installed from the latest `.deb`-file, [Download here](https://jenkins.ch.cam.ac.uk/view/AMI2/job/ami-plugin/org.xml-cml$ami2/lastSuccessfulBuild/artifact/org.xml-cml/ami2/0.1-SNAPSHOT/ami2-0.1-SNAPSHOT.deb)

```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/ami-plugin/org.xml-cml$ami2/lastSuccessfulBuild/artifact/org.xml-cml/ami2/0.1-SNAPSHOT/ami2-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

### How to use ami-plugins

It all starts with a `scholarly.html`-file. For ami to be able to run smoothly, please follow the instructions for [norma](../norma/norma-tutorial.md). Your project directory should look like this:

```bash
$ tree dinosaurs-xmls/
dinosaurs-xmls/
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

#### ami2-species


You can then search for all occurences of a species name with `$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type [genus|binomial|genussp]`, where `--sp.type` is one of [genus](https://en.wikipedia.org/wiki/Genus) (*Brachiosaurus*), [binomial](https://en.wikipedia.org/wiki/Binomial_nomenclature) (*B. altithorax*) or [genussp](???) (*example*).

ami will print out all matches while searching.

```bash
$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type genus
$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type binomial
$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type genussp
```

Results get stored within the corresponding ctree for each document, so you get

```
$ tree dinosaurs-xmls
dinosaurs-xmls
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   ├── results
│   │   └── species
│   │       ├── binomial
│   │       │   └── results.xml
│   │       ├── genus
│   │       │   └── results.xml
│   │       └── genussp
│   │           └── results.xml
│   └── scholarly.html
├── PMC3893247
│   ├── fulltext.xml
│   ├── results
│   │   └── species
│   │       ├── binomial
│   │       │   └── results.xml
│   │       ├── genus
│   │       │   └── results.xml
│   │       └── genussp
│   │           └── results.xml
│   └── scholarly.html
...
```
Results are stored in the [XML-format](https://en.wikipedia.org/wiki/XML), which is similar to json in the sense that it stores named values in tags and elements. If you take a look into a `results.xml` you find a list of all matching results.
`$ cat dinosaurs-xmls/PMC4349051/results/species/genussp/results.xml `
```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="genussp">
 <result pre="t very strongly arched, as is the case in adult lambeosaurines and unlike young individuals (e.g., " exact="Parasaurolophus sp" match="Parasaurolophus sp" post=". RAM 14000). These different angles are possibly a consequence of more strongly arched frontals in" name="genussp"/>
 <result pre=" and 1/240; Saveliev, Alifanov &amp;amp;amp; Bolotsky, 2012; Lauters et al., 2013) and the subadult of " exact="Corythosaurus sp" match="Corythosaurus sp" post=". (CMN 34825; Evans, Ridgely &amp;amp;amp; Witmer, 2009). The olfactory bulbs are turned downward with " name="genussp"/>
 <result pre="ampullae, the lateral ampulla is larger than the posterior ampulla and the anterior ampulla, as in " exact="Parasaurolophus sp" match="Parasaurolophus sp" post=". RAM 14000 ( Farke et al., 2013) and unlike in Hypacrosaurus altispinus ROM 702 and Lamb" name="genussp"/>
 <result pre=" sp. RAM 14000 ( Farke et al., 2013) and unlike in Hypacrosaurus altispinus ROM 702 and " exact="Lambeosaurus sp" match="Lambeosaurus sp" post=". ROM 758 ( Evans, Ridgely &amp;amp;amp; Witmer, 2009), where the anterior ampulla is the largest, foll" name="genussp"/>
</results>
```
An individual result is stored within result-tags: `<result>`. A search result contains the exact match, e.g. `exact="Parasaurolophus sp"`, where "exact" is the name of the attribute, and "Parasaurolophus sp" its value. Additionally, a search result contains 99 characters *before* and 99 characters *after* the match in the `pre` and `post` attributes.


#### ami2-gene

The search for genes works in the same way, just with another command: `$ ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type human`. At the moment there is only one gene type available (`human`). Results are again stored within the ctree.

```
$ tree dinosaurs-xmls
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

The `results.xml` follows the same structure, a results-tag with pre, post, and exact attributes.

`$ cat dinosaurs-xmls/PMC4454486/results/gene/human/results.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="human">
 <result pre=" individual vertebrae in Cretoxyrhina mantelli. The relationship between centrum diameter ( " exact="CD" post=" in mm) and total length ( TL) can be estimated with the following formula: " name="human"/>
 <result pre="hina mantelli. The relationship between centrum diameter ( CD in mm) and total length ( " exact="TL" post=") can be estimated with the following formula: " name="human"/>
</results>
```


#### ami2-sequence

The search for sequences follows the same structure: `$ ami2-sequence -q dinosaurs-xmls/ -i scholarly.html --sq.sequence --sq.type [dna|rna|prot|prot3|carb3]`, where `--sq.type` is one of `dna rna prot prot3 carb3`. 

The results are in general of the same structure, with an additional attribute `xpath` that shows the location of the match within the html-structure of the `scholarly.html`.

`$ cat dinosaurs-xmls/PMC4447998/results/sequence/rna/results.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="rna">
 <result pre="the oligonucleotides, within NR3C1 (GenBank #AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTT" exact="AGAGGG" post="-3′ and NR3HumR: 5′-biotin-7-CCCCCAACTCCCCAAAAA-3′ (adapted from Oberlander et al., 2008). Amplific" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
 <result pre="(GenBank #AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTTAGAGGG-3′ and NR3HumR: 5′-biotin-7-" exact="CCCCCAAC" post="TCCCCAAAAA-3′ (adapted from Oberlander et al., 2008). Amplification resulted in a 403 bp fragment (" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
 <result pre="#AY436590) is the following: NR3HumF: 5′-TTTGAAGTTTTTTTAGAGGG-3′ and NR3HumR: 5′-biotin-7-CCCCCAACT" exact="CCCCAAAAA" post="-3′ (adapted from Oberlander et al., 2008). Amplification resulted in a 403 bp fragment (position-3" xpath="/html[1]/body[1]/div[1]/div[4]/div[2]/div[8]/p[3]" name="rna"/>
</results>
```

#### ami2-regex

Constructing a regex-query with ami works a bit different: `ami2-regex -q dinosaurs-xml -i scholarly.html --context 25 40 --r.regex path-to/regex.xml`. Two new arguments come in here, `--context pre post`, which tells ami how many characters before and after a match should be captured; and `--r.regex path-to/regex.xml`. The `regex.xml` is a file we create beforehand, and which contains all regex-queries we want ami to do. 

**Now, what is a regex?** A regex ([regular expressions](https://en.wikipedia.org/wiki/Regular_expression)) is a sequence of characters that can be used for matching a search pattern (e.g. dinosaurs) in a text ("This text is about dinosaurs."). It is formalized and more complex patterns can be constructed, `[Dd]inosaur[s]?` matches upper and lower case words in singular and plural form. With `[Dd]inosaur[s]?` you look at the same time for Dinosaur, dinosaur, Dinosaurs, and dinosaurs. Do you see the potential for formalizing searches?

Digits can be added by e.g. `[0-9]`, which matches any digit, or by fixed sequences `(00111001)`. Non-alphanumeric characters have to be escaped by `\`, so if you want to search for the number 3.14 explicitly, the regex looks like `(3\.14)`.

We will now see a variety of regex-s and how they are used in a `regex.xml`.

A `regex.xml` has to be always wrapped by opening tags `<compoundRegex title="title">` and closing tags `</compoundRegex>` - notice the `/` at the beginning of the closing tag. You can set `"title"` to any representative name you want - it will be the folder name where the outputs are stored.
```xml
<compoundRegex title="dinosaurfood">
</compoundRegex>
```

In the `regex.xml` each regex-query is written to a new line, and consists of opening and closing tags `<regex></regex>`. Within the opening tag there are two attributes declared, `weight` and `fields`. `weight` is the relative importance given to each match, and influences indexing engines - we can use the default value of `"1.0"`. `fields` corresponds to the regex-query, and specifies the name and additional variables the query returns. 
If you take "food" from line two, one variable named "food" is expected within a match. If you take "predator" from line three, you specify the name and another field, and your query should also be returning two variables within a match.
```xml
<compoundRegex title="dinosaurfood">
<regex weight="1.0" fields="food"></regex>
<regex weight="1.0" fields="predator"></regex>
</compoundRegex>
```

What is missing now is the regex-query itself. A query itself is placed between the regex-tags `<regex>query</regex>`and is framed by round brackets `()`. In line two one field ("food") is defined. We want to get both upper and lower cases, and `[Ff]` matches either `F` or `f`: `([Ff]ood)`

The following characters `ood` are fixed for this query, they have to be matched. For the second query, we want to find all mentions of "predator regime/s". For this we need `\s`, a special character standing for ` ` - the whitespace, blank character. The questions mark `[s]?` makes the "s" optional: `([Pp]redator\sregime[s]?)`

```xml
<compoundRegex title="dinosaurfood">
<regex weight="1.0" fields="food">([Ff]ood)</regex>
<regex weight="1.0" fields="predator">([Pp]redator\sregime[s]?)</regex>
</compoundRegex>
```

We now construct a regex.xml like that (use any texteditor for that) and place this XML in our project folder as `dinosaurfood.xml`. We run ami with it, and because we want to get some context around our matches, add the `--context pre post` option, which captures character before and after the match.

```bash
$ ami2-regex -q dinosaurs-xmls/ -i scholarly.html --r.regex dinosaurs-xmls/dinosaurfood.xml --context 50 50
```

`$ cat dinosaurs-xmls-regex/PMC4298445/results/regex/dinosaurfood/results.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="dinosaurfood">
 <result pre="rs of Darwin's finches entered a lard of abundant " name0="food" value0="food" post="and varied living quarters, unmarred by the presen" xpath="/html[1]/body[1]/div[1]/div[4]/p[2]"/>
 <result pre="amas mosquitofish populations occupying different " name0="predator" value0="predator regimes" post="causes premating reproductive isolation due to sex" xpath="/html[1]/body[1]/div[1]/div[10]/div[1]/div[1]/p[4]"/>
</results>
```

The output contains 50 characters `pre` and 50 characters `post` the `value0`, as well as the `xpath` of the match in the scholarly.html.


### What can I do with ami-results?

A project folder with a ctree after using ami-plugins looks like this:

```bash
$ tree dinosaurs-xmls
dinosaurs-xmls
├── dinosaurfood.xml
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   ├── results
│   │   ├── gene
│   │   │   └── human
│   │   │       └── results.xml
│   │   ├── regex
│   │   │   └── dinosaurfood
│   │   │       └── results.xml
│   │   ├── sequence
│   │   │   ├── carb3
│   │   │   │   └── results.xml
│   │   │   ├── dna
│   │   │   │   └── results.xml
│   │   │   ├── prot
│   │   │   │   └── results.xml
│   │   │   ├── prot3
│   │   │   │   └── results.xml
│   │   │   └── rna
│   │   │       └── results.xml
│   │   └── species
│   │       ├── binomial
│   │       │   └── results.xml
│   │       ├── genus
│   │       │   └── results.xml
│   │       └── genussp
│   │           └── results.xml
│   └── scholarly.html
├── PMC3893247
...
```

### Summary and next steps

* Plugins each have their own commands.
* A project folder containing ctrees is always the input.
* Results are stored within the ctree in a plugin-specific folder.

**Next steps**
* Continue to [cat](../cat/cat-tutorial.md) for the next step of the ContentMine pipeline.
