![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is norma?

Scientific literature is vast, and their representations are as manifold as their content. But below the superficial differences in appearance and layout, scientific literature follows a generally accepted structure. This structure has been formalized and translated into machine readable format via [scholarly HTML](../sHTML/sHTML-overview.md) (sHTML). If you want to successfully apply search and retrieval techniques onto a collection of papers, or to do a comprehensive literature review, the diverse website, PDF or other document formats have to be converted into a uniform, consistent structure. This process is called *normalization*, and the tool in the ContentMine pipeline performing this step is called **norma**. norma furthermore possesses the ability to reverse engineer graphs, and e.g. extract data points from a timeline.

norma:
* takes inputs from three sources (getpapers, quickscrape, an existing collection of documents),
* is able to apply a selection of transformations between formats,
* is able to extract specific structural elements from a document,
* creates normalized *scholarly HTML* (shtml from now on) for each document.

norma offers a web of paths between three different input streams (an API-query from [getpapers](../getpapers/getpapers-tutorial.md), a URL-scrape from [quickscrape](../quickscrape-tutorial.md), and an existing collection of PDFs), and three different outputs (a collection of sHTML-files, ). The goal is always to have complete ctrees in order to run search and retrieve tools ([ami](../ami/ami-tutorial.md) or [cat](../cat/cat-tutorial.md)).

In this tutorial we will trace the path through norma for a variety of use cases:
* normalizing search results from getpapers towards sHTML
* normalizing search results from quickscrape towards sHTML
* normalizing a collection of PDFs towards sHTML

[1. Installation](#installation)

[2. XML to sHTML](#xml-to-shtml)

[3. HTML to sHTML](#html-to-shtml)

[4. PDF collections to TXT](#pdf-collections-to-txt)

[5. Summary and next steps](#summary-and-next-steps)


### Installation

norma can be installed from the latest `.deb`-file on [link missing](404)


### XML to sHTML


![xml2shtml](../../resources/images/software/norma/xml2shtml.png)

In this case we start with a ctree containing search results from 

```bash
$ getpapers -q dinosaurs -o dinosaurs-xmls -x
```

```bash
$ tree dinosaurs
dinosaurs/
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   └── fulltext.xml
├── PMC3893247
│   └── fulltext.xml
...
```

We transform them into sHTML with

```bash
$ norma \
     -q dinosaurs-xmls \
     -i fulltext.xml \
     -o scholarly.html \
     --transform nlm2html
```

`-q` is the parameter specifying the project-folder location, containing ctrees. `-i` is the parameter specifying the files to take as input, in this case fulltext.xml. `-o` is the parameter specifying the desired output, in this case scholarly.html. `--transform`` is the parameter specifying the normalizing process to undertake, in this case a conversion from XML to sHTML. The ctree then is added with a scholary.html.


```bash
$ tree dinosaurs
dinosaurs/
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   ├── fulltext.xml
│   └── scholarly.html
├── PMC3893247
│   ├── fulltext.xml
│   └── scholarly.html
...
```


Shouldn't -i and -o be redundant/deprecated? because `--transform nlm2html` should contain all the relevant information: look into all ctrees, take each fulltext.xml and convert it into scholarly.html???

### HTML to sHTML


![html2shtml](../../resources/images/software/norma/html2shtml.png)

The path from a fulltext.html to a scholarly.html is one step longer. We begin with the results of a quickscrape of a list of fulltext-urls (this has to be provided by you).

```bash
$ quickscrape \
    -r urllist.txt \
    -o dinosaurs-htmls \
    -d journal-scrapers/scrapers
```


```bash
$ tree dinosaurs-html
dinosaurs-htmls/
├── http_europepmc.org_articles_PMC2214819
│   ├── fulltext.html
│   └── results.json
├── http_europepmc.org_articles_PMC2635535
│   ├── fulltext.html
│   └── results.json
├── http_europepmc.org_articles_PMC2997427
│   ├── fulltext.html
│   └── results.json
...
```

```bash
$ norma \
    -q dinosaurs-html \
    -i fulltext.html \
    -o fulltext.xhtml \
    --html jsoup
```


```bash
$ tree dinosaurs-htmls/
dinosaurs-htmls/
├── http_europepmc.org_articles_PMC2214819
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   └── results.json
├── http_europepmc.org_articles_PMC2635535
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   └── results.json
├── http_europepmc.org_articles_PMC2997427
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   └── results.json
...
```


```bash
$ norma \
    -q dinosaurs-htmls/ \
    -i fulltext.xhtml \
    -o scholarly.html \
    --transform nature2html
```


```bash
$ tree dinosaurs-htmls/
dinosaurs-htmls/
├── http_europepmc.org_articles_PMC2214819
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   ├── results.json
│   └── scholarly.html
├── http_europepmc.org_articles_PMC2635535
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   ├── results.json
│   └── scholarly.html
├── http_europepmc.org_articles_PMC2997427
│   ├── fulltext.html
│   ├── fulltext.xhtml
│   ├── results.json
│   └── scholarly.html
...
```

**Problem: only one transformation nature2html available?**

### PDF collections to TXT

![pdf2txt](../../resources/images/software/norma/pdf2txt.png)

If you have an existing collection of PDFs, norma can apply some normalizations to them as well. Before that, we have to move them into a ctree-similar structure. If you `cd`` into your folder with PDFs (e.g. `cd dinosaur-pdfs`), you may find something like this:

```
$ cd dinosaur-pdfs
$ tree
.
├── Natarajan - Bone Cancer.pdf
├── Taylor - Aspects of sauropod dinosaurs.pdf
├── Taylor - Sauropods Giraffes.pdf
└── Wedel - Posteranial Pneumacity in Dinosaurs.pdf

0 directories, 4 files
```

You can then use this small shell script to create folders based on the names of PDFs, move each PDF into the corresponding folder, and rename it to fulltext.pdf. This is now our project folder. We then move one level back in the hierarchy with `cd ..`, in order to run norma from outside the folder.

```bash
$ for fname in *.pdf; do
filename=$(basename "$fname");
filename="${filename%.*}";
mkdir "$filename";
mv "$fname" "$filename"/fulltext.pdf;
done
cd ..
```

The folder structure should then look like this:

```
$ tree dinosaur-pdfs
dinosaur-pdfs
├── Natarajan - Bone Cancer
│   └── fulltext.pdf
├── Taylor - Aspects of sauropod dinosaurs
│   └── fulltext.pdf
├── Taylor - Sauropods Giraffes
│   └── fulltext.pdf
└── Wedel - Posteranial Pneumacity in Dinosaurs
    └── fulltext.pdf

4 directories, 4 files
```

The following command creates simple *.txt files from each pdf

```bash
$ norma -q dinosaurs-pdfs/ -i fulltext.pdf -o fulltext.pdf.txt --transform pdf2txt
$ tree dinosaurs-pdfs/
dinosaurs-pdfs/
├── Natarajan - Bone Cancer
│   ├── fulltext.pdf
│   └── fulltext.pdf.txt
├── Taylor - Aspects of sauropod dinosaurs
│   ├── fulltext.pdf
│   └── fulltext.pdf.txt
├── Taylor - Sauropods Giraffes
│   ├── fulltext.pdf
│   └── fulltext.pdf.txt
└── Wedel - Posteranial Pneumacity in Dinosaurs
    ├── fulltext.pdf
    └── fulltext.pdf.txt

4 directories, 8 files
```


### Summary and next steps

Following transformations are possible, with different goals in mind:
* 


**Next steps**
* Continue to [sHTML](../sHTML/sHTML-overview.md) if you want to learn more about scholarly HTML.
* Continue to [ami](../norma/ami-tutorial.md) for the next step of the ContentMine pipeline.