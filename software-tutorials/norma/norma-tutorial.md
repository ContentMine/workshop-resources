![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is norma?

Scientific literature is vast, and their representations are as manifold as their content. But below the superficial differences in appearance and layout, scientific literature follows a generally accepted structure. This structure has been formalized and translated into machine readable format via [scholarly HTML](../sHTML/sHTML-overview.md) (sHTML). If you want to successfully apply search and retrieval techniques onto a collection of papers, or to do a comprehensive literature review, the diverse website, PDF or other document formats have to be converted into a uniform, consistent structure. This process is called *normalization*, and the tool in the ContentMine pipeline performing this step is called **norma**. norma furthermore possesses the ability to reverse engineer graphs, and e.g. extract data points from a timeline.

norma offers a web of paths between three different input streams (an API-query from [getpapers](../getpapers/getpapers-tutorial.md), a URL-scrape from [quickscrape](../quickscrape-tutorial.md), and an existing collection of PDFs), and three different outputs (a collection of sHTML-files, ). The goal is to create a scholarly HTML-file for each source document, in order to run search and retrieve tools ([ami](../ami/ami-tutorial.md) or [cat](../cat/cat-tutorial.md)). We will learn how to compare and validate the output of norma in the [overview for scholarly HTML](../sHTML/sHTML-overview.md).

![normasmall0](../../resources/images/software/norma/normasmall0.png)

Now we will trace the path through norma for three use cases:
* normalizing search results from getpapers towards sHTML
* normalizing search results from quickscrape towards sHTML
* normalizing a collection of PDFs towards simple text files

[1. Installation](#installation)

[2. getpapers: XML to sHTML](#xml-to-shtml)

[3. quickscrape: HTML to sHTML](#html-to-shtml)

[4. experimental: PDF collections to TXT](#pdf-collections-to-txt)

[5. Summary and next steps](#summary-and-next-steps)


### Installation

norma can be installed from the latest `.deb`-file, [Download here](https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb)

```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

The tutorial has been developed on 
```
$ norma --version
norma(0.1.8)
```

### XML to sHTML

![normaxml2shtml](../../resources/images/software/norma/normaxml2shtml0.png)

**Overview**

![xml2shtml](../../resources/images/software/norma/xml2shtml.png)


We start with a search results with getpapers

```bash
$ getpapers -q dinosaurs -o dinosaurs-xmls -x
```

getpapers returns a **project folder** containing search metadata and ctrees. Each ctree holds all data regarding one paper, in this case a `fulltext.xml`.
```
$ tree dinosaurs-xmls
dinosaurs-xmls/
├── eupmc_results.json
├── fulltext_html_urls.txt
├── PMC3893193
│   └── fulltext.xml
├── PMC3893247
│   └── fulltext.xml
...
```

We transform the fulltext.xml-files into sHTML. This can be done in bulk by passing the project folder with `-q`. The input/output-parameters `-i` and `-o` are verbose at the moment, we are working on simplifying to command, so that only the transformation parameter `--transform nlm2html` is needed. It is possible that norma prints a lot of debugging information, this does not automatically mean that something is wrong.

```bash
$ norma \
     -q dinosaurs-xmls \
     -i fulltext.xml \
     -o scholarly.html \
     --transform nlm2html
```

If a `scholarly.html`-file has been added to each ctree, norma was succesful.

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


### HTML to sHTML

![normahtml2shtml](../../resources/images/software/norma/normahtml2shtml0.png)

**Overview**

![html2shtml](../../resources/images/software/norma/html2shtml.png)

The path from a fulltext.html to a scholarly.html is a few steps longer. We begin with the results of a quickscrape of a list of fulltext-urls (this has to be provided by you).

```bash
$ quickscrape \
    -r urllist.txt \
    -o dinosaurs-htmls \
    -d journal-scrapers/scrapers
```

getpapers creates a project folder again containing ctrees. The metadata files are now within each ctree.
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

We first need to tidy up the `fulltext.html`-files. For this we pass the project folder with `q`, specify input and output files with `-i and `-o` and use `--html jsoup` for cleaning the html.
```bash
$ norma \
    -q dinosaurs-html \
    -i fulltext.html \
    -o fulltext.xhtml \
    --html jsoup
```

This procedures creates an intermediary document `fulltext.xhtml`. 
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

We perform the final transformation to shtml with a specific transformation, which requires specific html-styles for each publisher. At the moment only one is available for testing purposes.
```bash
$ norma \
    -q dinosaurs-htmls/ \
    -i fulltext.xhtml \
    -o scholarly.html \
    --transform nature2html
```

We then have a project folder with ctrees, that trace the path from `fulltext.html` to `scholarly.html`
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


### PDF collections to TXT

This is still in an experimental stage. It works, but the output is not very clean and not ready for further preprocessing. But if you want to extract the *raw content* of a PDF, this is possible here.

![pdf2txt](../../resources/images/software/norma/pdf2txt.png)

PDF is a notoriously bad format for automatic processing. While understandable for the human reader, PDF is a real obstacle for content mining. This lies in the nature of the document, which - from a machine's perspective - is essentially a 2-dimensional plane with symbols on it. The only information that a machine readily knows about any symbol is it's x- and y-location on the plane. Meaning, relations with other symbols, or logical concepts are not present in a PDF and have to be constructed by input from the outside.

If you have an existing collection of PDFs, norma can apply some simple normalizations to them as well. Before that, we have to move them into a ctree-similar structure. If you change into your folder with PDFs (e.g. `cd dinosaur-pdfs`), you may find something like this:

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



**Next steps**
* Continue to [sHTML](../sHTML/sHTML-overview.md) if you want to learn more about scholarly HTML.
* Continue to [ami](../norma/ami-tutorial.md) for the next step of the ContentMine pipeline.