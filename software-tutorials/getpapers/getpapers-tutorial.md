![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is getpapers?

`getpapers` is together with [quickscrape](../quickscrape/quickscrape-tutorial.md) one of the entry points of the ContentMine pipeline. getpapers can fetch article metadata, fulltexts (PDF or XML), and supplementary materials. It's designed for use in content mining, but you may find it useful for quickly acquiring large numbers of papers for reading, or for bibliometrics. getpapers accesses APIs (EUPMC, IEEE, Arxiv), queries them for search terms and returns specific datastructures (metadata, PDFs, XMLs). In contrast, quickscrape takes URLs as input and scrapes the whole page.

This tutorial covers the installation of getpapers, explains possible options, demonstrates how to construct simple and complex queries, and shows what output can be expected from getpapers.

[1. Installation](#installation)

[2. Usage](#usage)

[3. Construct a simple query and compare results](#construct-a-simple-query-and-compare-results)

[4. Getting pdfs and other files](#getting-pdfs-and-other-files)

[5. Complex queries for EPMC](#complex-queries-for-epmc)

[6. Complex queries for ArXiv](#complex-queries-for-arxiv)

[7. Complex queries for IEEE](#complex-queries-for-ieee)

[8. Summary and next steps](#summary-and-next-steps)


### Installation

```bash
$ npm install --global getpapers
```

### Usage

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

A basic query consists of free text, without any further specification. This query **returns only metadata**. Without a specification of the API, getpapers will chose EuropePMC.

A minimum query consists of `-q 'query terms' -o outdirectory`. 

We will now compare the results of a query for *"dinosaurs"* on EuropePMC, IEEE and ArXiv.

```bash
$ getpapers -q 'dinosaurs' --api eupmc -o test_eupmc
$ getpapers -q 'dinosaurs' --api ieee -o test_ieee
$ getpapers -q 'dinosaurs' --api arxiv -o test_arxiv
```

getpapers now queries each API for "dinosaurs" and stores the results in separate folders. We can look into the files of each folder with `ls folder`, e.g.

```bash
$ ls test_eupmc/
eupmc_results.json  fulltext_html_urls.txt
```

We can then look at the content of "fulltext_html_urls.txt" with [cat](https://en.wikipedia.org/wiki/Cat_%28Unix%29), [head](https://en.wikipedia.org/wiki/Head_%28Unix%29) or [tail](https://en.wikipedia.org/wiki/Tail_%28Unix%29).

```bash
$ cat test_eupmc/fulltext_html_urls.txt
# prints the whole file to the terminal
$ head test_eupmc/fulltext_html_urls.txt
$ head -5 test_eupmc/fulltext_html_urls.txt
# prints the top 10/5 lines
$ tail test_eupmc/fulltext_html_urls.txt
$ tail -5 test_eupmc/fulltext_html_urls.txt
# prints the last 10/5 lines
```

We can also see how many fulltext results we may get by counting the lines of the fulltext_html_urls.txt with [wc](https://en.wikipedia.org/wiki/Wc_%28Unix%29). For some queries and APIs it may happen that no fulltexts are found, in which case no fulltext_html_urls.txt is created.

```bash
$ wc -l test_eupmc/fulltext_html_urls.txt
$ wc -l test_ieee/fulltext_html_urls.txt
```

At this point you can use the urls.txt as input for [quickscrape](../quickscrape/quickscrape-tutorial.md#scraping), but for the moment we'll continue exploring getpapers.

The other file a simple query returns is called *apiname*_results.[json](https://en.wikipedia.org/wiki/JSON). This is a detailed, lengthy file containing metadata (e.g. doi, publication id, authors, ...) in an {attribute:value}-format. Using cat produces many lines of not very readable output. We can filter for words we are interested in with [grep](https://en.wikipedia.org/wiki/Grep). This returns only lines containing the search word.

```bash
$ grep dinosaur test_eupmc/eupmc_results.json
```

If we want to read the abstracts, which are stored under the "abstractText" attribute, we tell grep to return one line after "abstractText" with `-A1`.

```bash
$ grep -A1 abstractText test_eupmc/eupmc_results.json
```

These tools are useful in getting some first idea of the content of files, but ContentMine provides some more advanced tools in later stages of the pipeline ([ami](../ami/ami-tutorial.md)). For now we continue with more queries with getpapers.

### Getting pdfs and other files

Until now, our queries only resulted in metadata and a list of urls. PDF files can be retrieved by adding a `-p` flag to the query. Please note, that a very **generic query** will result in a **huge number of results**. Unless intended, you can cancel a search with `Ctrl+C` in the command line.

```bash
$ getpapers -q 'dinosaurs' --api eupmc -o test_eupmc -p
$ getpapers -q 'dinosaurs' --api ieee -o test_ieee -p
$ getpapers -q 'dinosaurs' --api arxiv -o test_arxiv -p
```

For every PDF found, getpapers creates a new folder containing a fulltext.pdf within the test_eupmc folder. After such a search, the folder structure looks like this:

```
test_eupmc
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
$ getpapers -q 'dinosaurs' --api eupmc -o test_eupmc -x
$ getpapers -q 'dinosaurs' --api ieee -o test_ieee -x
$ getpapers -q 'dinosaurs' --api arxiv -o test_arxiv -x
```

Results are added to the existing results, so the folder structure may look like this now:

```
test_eupmc
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

This is the beginning of the [ctree](../ctree/ctree-overview.md)-structure, which is the main data structure of the ContentMine pipeline, and any further operations are going to be centered around the ctree.

### Complex queries for EPMC

Queries are processed by EuropePMC. In their simplest form, they can be free text, like this:

```bash
$ getpapers -q 'dinosaurs' --api eupmc -o test_eupmc
```

But they can also be much more detailed, using the EuropePMC webservice's query language. A selection of the most commonly useful search fields is explained [here](getpapers-eupmc-queries.md), and a complete documentation of possible queries is in Appendix I of the [EuropePMC reference PDF](http://europepmc.org/docs/EBI_Europe_PMC_Web_Service_Reference.pdf).

For example we can restrict our search to only papers that mention 'dinosaurs' in the abstract. Note that the query has to be encapsuled by single quotations marks '', and further specifications by double quotation marks "".

```bash
$ getpapers -q 'ABSTRACT:"dinosaurs"' --api eupmc -o test_eupmc
```

Or to only get papers with a CC-BY license:

```bash
$ getpapers -q 'LICENSE:"cc by" OR LICENSE:"cc-by"' --api eupmc -o test_eupmc
```

We combined two restrictions using the logical `OR` keyword. We can also use `AND`, and can group operations using brackets:

```bash
$ getpapers -q '(LICENSE:"cc by" OR LICENSE:"cc-by") AND ABSTRACT:"dinosaurs"' --api eupmc -o test_eupmc
```

Some more examples, combined from the [documentation](getpapers-eupmc-queries.md):

Search for papers which contain the phrase "dinosaurs" in the introduction section and the phrase "survey" in the methods section.
```bash
$ getpapers -q 'INTRO:"dinosaurs" AND METHODS:"survey"' --api eupmc -o test_eupmc
```

Search for papers where the authors contain "Smith" and which were published in either "Biology" or "Cell".
```bash
$ getpapers -q 'AUTH:"Smith" AND (JOURNAL:"biology" OR JOURNAL:"cell")' --api eupmc -o test_eupmc
```

Search for papers that contain "dinosaurs" in the abstract and were published between 2010 and 2012.
```bash
$ getpapers -q 'ABSTRACT:dinosaurs AND PUB_YEAR:[2010 TO 2012]' --api eupmc -o test_eupmc
```

Search for papers that contain "dinosaur" in the title and were first published between July 2009 and June 2013.
```bash
$ getpapers -q 'TITLE:dinosaurs AND FIRST_PDATE:[2009-07-01 TO 2013-06-30]' --api eupmc -o test_eupmc
```

Search for papers where the European Research Council ("ERC") is mentioned in the acknowledgements section.
```bash
$ getpapers -q 'ACK_FUND:ERC' --api eupmc -o test_eupmc
```


### Complex queries for ArXiv

ArXiv has a nice, clearly defined format. Queries can target individual fields of the articles records. A selection possible search fields is explained [here](getpapers-arxiv-queries.md), and a complete documentation of possible queries is provided by [ArXiv](http://arxiv.org/help/api/user-manual). 

Search for papers that contain "dinosaurs" in the abstract.
```bash
$ getpapers -q 'abs:dinosaurs' --api arxiv -o test_arxiv
```

Queries may be combined with boolean operators `AND, OR, ANDNOT`. ANDNOT is a particularly helpful operator, it excludes results that contain a phrase, and therefore works as a filter.

Search for papers that contain dinosaurs in the abstract, but not physics in any search field.
```bash
$ getpapers -q 'abs:dinosaurs ANDNOT all:physics' --api arxiv -o test_arxiv
```

Search for papers that contain dinosaurs in the abstract, but neither "quantum physics" or "biology" in any search field (yes, those combinations exist!)
```bash
$ getpapers -q 'abs:dinosaurs ANDNOT (all:"quantum physics" OR all:biology)' --api arxiv -o test_arxiv
```

Note that "quantum physics" has been grouped into a single phrase by double quotes "". 


### Complex queries for IEEE


IEEE does not provide fulltext XML, and their fulltext PDFs are not easily downloadable (though we're working on it). `getpapers` will output metadata for the search results, and will attempt to reconstruct the fulltext HTML URLs for any papers that have fulltext HTML.

A selection of options is described [here](getpapers-ieee-queries.md) and the complete IEEE query format is loosely documented at [IEEE Xplore Gateway](http://ieeexplore.ieee.org/gateway/). In general, anything that works in the website search will also work in `getpapers` with the `--api ieee` option enabled. As with the other APIs, any search is automatically restricted to Open Access papers. 

```bash
$ getpapers -q 'dinosaurs' --api ieee -o test_ieee
```

Search for papers containing the phrase "mining" in the abstract, and which appear between 2010 and 2014:
```bash
$ getpapers -q 'ab=mining pys=2010 pye=2014' --api ieee -o test_ieee
```

Search for papers containing the phrase "mining" in the title and filtered for the content type "Conferences":
```bash
$ getpapers -q 'ti=mining ctype=Conferences' --api ieee -o test_ieee
```

### Summary and next steps

* A minimum query consists of `getpapers -q "query terms" -o outdir` and returns only metadata.
* Use `-x` for machine-readable fulltext results, because XML-files provide better mining results in later stages of the tool chain.
* Use `-p` if you want to retrieve human-readable fulltexts in PDF-format.
* Unless you use `-a`, only Open Access papers will be returned.
* Each API has a different query language, please refer to the documentation ([EUPMC](getpapers-eupmc-queries.md), [ArXiv](getpapers-arxiv-queries.md), [IEEE](getpapers-ieee-queries.md))

**Next steps**
* Continue to [quickscrape](../quickscrape/quickscrape-tutorial.md) for an introduction to scraping.

