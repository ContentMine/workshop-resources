# getpapers

## Table of contents

1. [Description](#description)
1. [Installation](#installation)
2. [Construct a simple query and compare results](#construct-a-simple-query-and-compare-results)
3. [Getting pdfs and other files](#getting-pdfs-and-other-files)
4. [Complex queries for EPMC](#complex-queries-for-epmc)
5. [Complex queries for ArXiv](#complex-queries-for-arxiv)
6. [Complex queries for IEEE](#complex-queries-for-ieee)
7. [Summary and next steps](#summary-and-next-steps)

## Description

**What does getpapers?**

`getpapers` is together with [quickscrape](../quickscrape/README.md) one of the entry points of the ContentMine pipeline. getpapers can fetch article metadata, fulltexts (PDF or XML), and supplementary materials. It's designed for use in content mining, but you may find it useful for quickly acquiring large numbers of papers for reading, or for bibliometrics. getpapers accesses APIs (EUPMC, IEEE, Arxiv), queries them for search terms and returns specific datastructures (metadata, PDFs, XMLs). In contrast, quickscrape takes URLs as input and scrapes the whole page.

This tutorial covers the installation of getpapers, explains possible options, demonstrates how to construct simple and complex queries, and shows what output can be expected from getpapers.

**Why do we need getpapers?**


**How can I use getpapers?**


**Glossary**
- JSON ([Wikipedia](https://en.wikipedia.org/wiki/JSON))
- XML ([Wikipedia](https://en.wikipedia.org/wiki/XML))
- API ([Wikipedia](https://en.wikipedia.org/wiki/API))
- Query



### Installation

```bash
sudo npm install --global getpapers
```

You can find the technical documentation for `getpapers` in its [repository](https://github.com/ContentMine/getpapers).

**Help**


The command line is going to be the main interface with ContentMine. Some basic commands for using and navigating the command line are documented [here](../shell/README.md), please have a look if you are new to using the command line.

Use `getpapers --help` to see the command-line help:

```
Usage: getpapers [options]

Options:

  -h, --help              output usage information
  -V, --version           output the version number
  -q, --query <query>     Search query (required)
  -o, --outdir <path>     Output directory (required - will be created if not found)
  --api <name>            API to search [arxiv, eupmc, ieee] (default: eupmc)
  -x, --xml               Download fulltext XMLs if available
  -p, --pdf               Download fulltext PDFs if available
  -s, --supp              Download supplementary files if available
  -l, --loglevel <level>  amount of information to log (silent, verbose, info*, data, warn, error, or debug)
  -a, --all               search all papers, not just open access
```

### Construct a simple query and compare results

The most basic query **returns only metadata**. Without a specification of the API, getpapers will chose EuropePMC as default data source.

A query must consist of `-q 'QUERY' -o OUTPUTDIRECTORY`.
* QUERY: the term(s) you want to look for.
* OUTPUTDIRECTORY: Folder in which you want the output files and directory. The folder will be created if it does not already exist.

This query creates a ```eupmc_results.json``` file in the named directory, in which a detailed, lengthy file containing metadata (e.g. doi, publication id, authors, ...) in an {key:value}-format is stored.

Let us now compare the results of a query for ```dinosaurs``` on EuropePMC, IEEE and ArXiv. Getpapers queries each API for "dinosaurs" and stores the results in separate folders.

<em>We'll collect YOUR screenshots of the results and paste them into the docs here!</em>

```bash
getpapers -q 'dinosaurs' --api eupmc -o eupmc
```

<em> insert your results here SCREENSHOT</em>

```bash
getpapers -q 'dinosaurs' --api ieee -o ieee
```

<em> insert your results here SCREENSHOT</em>

```bash
getpapers -q 'dinosaurs' --api arxiv -o arxiv
```

<em> insert your results here SCREENSHOT</em>

We can look into the files of each folder with `ls`, e.g (Directory name is eupmc.

```bash
cd eupmc
ls
eupmc_results.json  fulltext_html_urls.txt
```

```fulltext_html_urls.txt``` contains the urls of all found publications which include the query term. With [cat](https://en.wikipedia.org/wiki/Cat_%28Unix%29), [head](https://en.wikipedia.org/wiki/Head_%28Unix%29) or [tail](https://en.wikipedia.org/wiki/Tail_%28Unix%29) we can have a short view into the files.

```bash
# prints the whole file to the terminal
cat eupmc/fulltext_html_urls.txt

# prints the top 10 lines
head eupmc/fulltext_html_urls.txt

# prints the last 5 lines
tail -5 eupmc/fulltext_html_urls.txt
```

If you want to know how many lines (publications) the query found, use the command `wc` ([doc](https://en.wikipedia.org/wiki/Wc_%28Unix%29)). 

```bash
wc -l eupmc/fulltext_html_urls.txt
```

It can happen, that for the query were found no fulltexts, in which case it the file `fulltext_html_urls.txt` will not be created.

At this point you could use the `fulltext_html_urls.txt` as input for [quickscrape](../quickscrape/README.md#scraping), but for the moment we'll continue exploring getpapers.

The other file a simple query returns is called ```APINAME_results.json``` and consists as described above the metadata of each found publication. 

Using ```cat``` produces many lines of not very readable output. We can filter for words we are interested in with [grep](https://en.wikipedia.org/wiki/Grep). This returns only lines containing the search word.

```bash
grep dinosaur eupmc/eupmc_results.json
```

If we want to read the abstracts, which are stored under the "abstractText" attribute, we tell grep to return one line after "abstractText" with `-A1`.

```bash
grep -A1 abstractText eupmc/eupmc_results.json
```

These tools are useful in getting some first idea of the content of files, but ContentMine provides some more advanced tools in later stages of the pipeline, like ([ami](../ami/README.md)). For now we continue with more sophisticated queries with getpapers.

### 3. Getting pdfs and other files

Until now, our queries only resulted in metadata and a list of urls. PDF files can be retrieved by adding a `-p` flag to the query. Please note, that a very **generic query** will result in a **huge number of results**. Unless intended, you can cancel a search with `Ctrl+C` in the command line.

```bash
getpapers -q 'dinosaurs' --api eupmc -o eupmc -p
getpapers -q 'dinosaurs' --api ieee -o ieee -p
getpapers -q 'dinosaurs' --api arxiv -o arxiv -p
```

For every PDF found, getpapers creates a [CTree](../ctree/README.md), which means in this case a new folder containing a fulltext.pdf within the eupmc folder. To understand the folder and file structure better, use the ```tree``` command. If you work on your local machine you may have to [install tree first](https://askubuntu.com/questions/431251/how-to-print-the-directory-tree-in-terminal).

```
tree eupmc
eupmc
├─ eupmc_results.json
├─ fulltext_html_urls.txt
├─ PMC1234567
│  └─ fulltext.pdf
├─ PMC1234568
│  └─ fulltext.pdf
├─ ...
```

Not all queries returned PDFs, we now try another query with `-x` for xml-results. In contrast to PDF, XML is a format that machines can understand well, and XML enables better mining results further down the pipeline.


```bash
getpapers -q 'dinosaurs' --api eupmc -o eupmc -x
getpapers -q 'dinosaurs' --api ieee -o ieee -x
getpapers -q 'dinosaurs' --api arxiv -o arxiv -x
```

The existing results only get update, no results (the pdf's) from before get lost as you can see:

```
tree eupmc
eupmc
├─ eupmc_results.json
├─ fulltext_html_urls.txt
├─ PMC1234567
│  ├─ fulltext.pdf
│  └─ fulltext.xml
├─ PMC1234568
│  └─ fulltext.pdf
├─ PMC1234569
│  └─ fulltext.xml
├─ ...
```

This is the beginning of the [ctree](../ctree/README.md)-structure, which is the main data structure of the ContentMine pipeline, and any further operations are going to be centered around the ctree.

### Complex queries for EuropePMC

[Europe PubMedCentral](https://europepmc.org/) (EUPMC)

**Data**


"The content scope of Europe PMC covers both abstracts and full text articles, with some full text
articles being available as Open Access. All the full text articles in Europe PMC have a corresponding PubMed abstract record, but only about 10-12% of PubMed abstracts have a corresponding Europe PMC full text article available (see figure above). In terms of the web service, all the content in Europe PMC can be searched. However, in the case of a full text search, in which the complete text of the article is searched, only the metadata and abstract will be sent in the response for most content. Each full text article therefore has a unique PMCID and a corresponding PubMed ID (PMID)."

DIAGRAMM


**Queries**


Queries are processed by EuropePMC. In their simplest form, they can be free text, like this one we executed before:

```bash
getpapers -q 'dinosaurs' --api eupmc -o eupmc
```

But they can also be much more detailed, using the EuropePMC webservice's query language. A selection of the most commonly useful search fields is explained [here](getpapers-eupmc-queries.md), and a complete documentation of possible queries is in Appendix I of the [EuropePMC reference PDF](http://europepmc.org/docs/EBI_Europe_PMC_Web_Service_Reference.pdf).

For example we can restrict our search to only papers that mention 'dinosaurs' in the abstract. Note that the query has to be encapsuled by single quotations marks ```'```, and further specifications by double quotation marks ```"```.

```bash
getpapers -q 'ABSTRACT:"dinosaurs"' --api eupmc -o eupmc_abstract
```

We can use the logical operations `ÀND` and `OR`, and can group operations using brackets:

```bash
getpapers -q '(LICENSE:"cc by" OR LICENSE:"cc-by") AND ABSTRACT:"dinosaurs"' --api eupmc -o eupmc_licence_abstract
```

So, we got now all Open Access publications about dinosaurs from EUPMC.

Here some more examples which show how powerful the EUPMC API is.

Search for papers which contain the phrase "dinosaurs" in the introduction section and the phrase "survey" in the methods section.
```bash
getpapers -q 'INTRO:"dinosaurs" AND METHODS:"survey"' --api eupmc -o eupmc_intro_methods
```

Search for papers where the authors contain "Smith" and which were published in either "Biology" or "Cell".
```bash
getpapers -q 'AUTH:"Smith" AND (JOURNAL:"biology" OR JOURNAL:"cell")' --api eupmc -o eupmc_author_journal
```

Downloads XML and PDF's for papers that contain "dinosaurs" in the abstract and were published between 2010 and 2012.
```bash
getpapers -q 'ABSTRACT:dinosaurs AND PUB_YEAR:[2010 TO 2012]' --api eupmc -o eupmc_abstract_year -px
```

Search for papers that contain "dinosaur" in the title and were first published between July 2009 and June 2013.
```bash
getpapers -q 'TITLE:dinosaurs AND FIRST_PDATE:[2009-07-01 TO 2013-06-30]' --api eupmc -o eupmc_title_pubdate
```

Search for papers where the European Research Council (ERC" is mentioned in the acknowledgements section.
```bash
getpapers -q 'ACK_FUND:ERC' --api eupmc -o eupmc_acknowledgement
```

You can find some more queries from the [documentation](getpapers-eupmc-queries.md)

### Complex queries for ArXiv

The ArXiv API has a nice, clearly defined format and offers full access to all publications on [arxiv.org](http://arxiv.org). Queries can target individual fields of the articles records. A selection of possible search fields is explained [here](getpapers-arxiv-queries.md), and a complete documentation is provided by [ArXiv](http://arxiv.org/help/api/user-manual). 

Search for papers that contain "dinosaurs" in the abstract.
```bash
getpapers -q 'abs:dinosaurs' --api arxiv -o arxiv_abstract -p
```

Queries may be combined with boolean operators `AND, OR, ANDNOT`. ANDNOT is a particularly helpful operator, it excludes results that contain a phrase, and therefore works as a filter.

Search for papers that contain dinosaurs in the abstract, but not physics in any search field.
```bash
getpapers -q 'abs:ABM ANDNOT all:physics' --api arxiv -o arxiv_abstract_not-physics -p
```

Search for papers that contain dinosaurs in the abstract, but neither "quantum physics" or "biology" in any search field (yes, those combinations exist!). Note that "quantum physics" has been grouped into a single phrase by double quotes ```"```. 
```bash
getpapers -q 'abs:dinosaurs ANDNOT (all:"quantum physics" OR all:biology)' --api arxiv -o arxiv_abstract_not-quantum-physics_not-biology -p
```

### Complex queries for IEEE

IEEE does not provide fulltext XML, and their fulltext PDFs are not easily downloadable (though we're working on it). `getpapers` will output metadata for the search results, and will attempt to reconstruct the fulltext HTML URLs for any papers that have fulltext HTML.

A selection of options is described [here](getpapers-ieee-queries.md) and the complete IEEE query format is loosely documented at [IEEE Xplore Gateway](http://ieeexplore.ieee.org/gateway/). In general, anything that works in the website search will also work in `getpapers` with the `--api ieee` option enabled. As with the other APIs, any search is automatically restricted to Open Access papers. 

```bash
getpapers -q 'dinosaurs' --api ieee -o ieee
```

Search for papers containing the phrase "mining" in the abstract, and which appear between 2010 and 2014:
```bash
getpapers -q 'ab=water pys=2010 pye=2014' --api ieee -o ieee_abstract_pubdate
```

Search for papers containing the phrase "mining" in the title and filtered for the content type "Conferences":
```bash
getpapers -q 'ti=mining ctype=Conferences' --api ieee -o ieee_title_content-type
```

## Summary and next steps

* A minimum query consists of `getpapers -q "query terms" -o outdir` and returns only metadata.
* Use `-x` for machine-readable fulltext results, because XML-files provide better mining results in later stages of the tool chain.
* Use `-p` if you want to retrieve human-readable fulltexts in PDF-format.
* Unless you use `-a`, only Open Access papers will be returned.
* Each API has a different native query language, please refer to the documentation ([EUPMC](getpapers-eupmc-queries.md), [ArXiv](getpapers-arxiv-queries.md), [IEEE](getpapers-ieee-queries.md))

**Next steps**
* Continue to [quickscrape](../quickscrape/README.md) for an introduction to scraping.
* Continue to [norma](../norma/README.md) for the next step of the ContentMine pipeline.
* Continue to [ctree](../ctree/README.md) for an introduction of the main datastructure.
