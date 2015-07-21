![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is ami?

ami is a collection of plugins that search, index and extract pieces of information from structured documents. A piece of information can be a mention of a certain species (e.g. Brachiosaurus) in a text, a gene sequence, or an expression of a chemical compound. The type of relevant information varies from discipline to discipline. We are continously developing new plugins to cover different types of encoded knowledge.

The input for ami is always a ctree containing documents in [scholarly HTML](../sHTML/sHTML-overview.md). ami then applies one of the available plugins, extracts the relevant content from the sHTML, and stores results in a folder in the ctree.


[1. Installation](#installation)

[2. species](#ami2-species)

[3. genes](#ami2-gene)

[4. sequences](#ami2-sequences)

[5. regex](#ami2-regex)

[6. Next steps](#next-steps)


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
`$ cat dinosaurs-xmls-species/PMC4349051/results/species/genussp/results.xml `
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
$ tree dinosaurs-xmls-gene/ | head -50
dinosaurs-xmls-gene/
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

`$ cat dinosaurs-xmls-gene/PMC4454486/results/gene/human/results.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<results title="human">
 <result pre=" individual vertebrae in Cretoxyrhina mantelli. The relationship between centrum diameter ( " exact="CD" post=" in mm) and total length ( TL) can be estimated with the following formula: " name="human"/>
 <result pre="hina mantelli. The relationship between centrum diameter ( CD in mm) and total length ( " exact="TL" post=") can be estimated with the following formula: " name="human"/>
</results>
```


#### ami2-sequence



minimum: `$ ami2-sequence -q dinosaurs-xmls/ -i scholarly.html --sq.sequence --sq.type [dna|rna|prot|prot3|carb3]`



#### ami2-regex


How to construct a regex query

https://github.com/ContentMine/ami/blob/master/regex/agriculture.xml


### What can I do with ami-results?


### Next steps


**Next steps**
* Continue to [cat](../cat/cat-tutorial.md) for the next step of the ContentMine pipeline.

