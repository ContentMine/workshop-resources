![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

[1. Installation](# Installation)

[2. Usage](# Usage)

[3. Construct a simple query and compare results](# Construct a simple query and compare results)

[4. Getting pdfs and other files](# Getting pdfs and other files)

[5. Complex queries for EPMC](# Complex queries for EPMC)

## What is getpapers?
<!-- (describe core functionality) -->

getpapers is together with [quickscrape](quickscrape.md) one of the entry points of the ContentMine pipeline. getpapers can fetch article metadata, fulltexts (PDF or XML), and supplementary materials. It's designed for use in content mining, but you may find it useful for quickly acquiring large numbers of papers for reading, or for bibliometrics. getpapers accesses APIs (EUPMC, IEEE, Arxiv), queries them for search terms and returns specific datastructures (metadata, PDFs, XMLs). In contrast, quickscrape take URLs as input and scrapes the whole page.

## How do I use it?
<!-- (explain options) -->

This part covers the installation of getpapers, explains possible options, demonstrates how to construct simple and complex queries, and shows what output can be expected from getpapers.

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

Three APIs are available at the moment, EuropePMC, IEEE, and ArXiv. Each API has its own query format. Usage guides are provided on our wiki:

- [EuropePMC query format](https://github.com/ContentMine/getpapers/wiki/europepmc-query-format)
- [IEEE query format](https://github.com/ContentMine/getpapers/wiki/ieee-query-format)
- [ArXiv query format](https://github.com/ContentMine/getpapers/wiki/arxiv-query-format)

### Construct a simple query and compare results

A basic query consists of free text, without any further specification. Please note, that a very **generic query** will result in a **huge number of results**. Unless intended, you can cancel a search with ```Ctrl+C``` in the command line.

A minimum query consists of ```-q 'query terms' -o outdirectory```. 

```getpapers -q 'molecuar biology' -o test```

This query results in around 200.000 papers to be downloaded, so we cancel with ```Ctrl+C``` and refine our query. Without a specification of the API, getpapers will chose EuropePMC.

We will now compare the results of a query for *dinosaurs* on EuropePMC, IEEE and ArXiv.

```bash
getpapers -q 'dinosaurs' --api eupmc -o test_eupmc
getpapers -q 'dinosaurs' --api ieee -o test_ieee
getpapers -q 'dinosaurs' --api arxiv -o test_arxiv
```

getpapers now queries each API for "dinosaur biology" and stores the results in separate folders. We can look into the files of each folder with ls folder, e.g.

```bash
ls test_eupmc/
eupmc_results.json  fulltext_html_urls.txt
```

We can then look at the content of "fulltext_html_urls.txt" with [cat](https://en.wikipedia.org/wiki/Cat_%28Unix%29), [head](https://en.wikipedia.org/wiki/Head_%28Unix%29) or [tail](https://en.wikipedia.org/wiki/Tail_%28Unix%29).

```bash
cat test_eupmc/fulltext_html_urls.txt
# prints the whole file to the terminal
head test_eupmc/fulltext_html_urls.txt
head -5 test_eupmc/fulltext_html_urls.txt
# prints the top 10/5 lines
tail test_eupmc/fulltext_html_urls.txt
tail -5 test_eupmc/fulltext_html_urls.txt
# prints the last 10/5 lines
```

We can also see how many fulltext results we may get by counting the lines of the fulltext_html_urls.txt with [wc](https://en.wikipedia.org/wiki/Wc_%28Unix%29). For some queries and APIs it may happen that no fulltext is found, in this case there is no fulltext_html_urls.txt created.

```bash
wc -l test_eupmc/fulltext_html_urls.txt
wc -l test_ieee/fulltext_html_urls.txt
wc -l test_arxiv/fulltext_html_urls.txt
```

At this point you can use the urls.txt as input for [quickscrape](../quickscrape), but for the moment we'll continue exploring getpapers.
The other file the simple query returns is called *apiname*_results.json [JSON](https://en.wikipedia.org/wiki/JSON). This is a detailed, lengthy file containing metadata (e.g. doi, publication id, authors, ...), using cat produces many lines of not very readable output. But we can filter for words we are interested in with [grep](https://en.wikipedia.org/wiki/Grep). This returns only lines containing the word.

```bash
grep dinosaur test_eupmc/eupmc_results.json
```

If we want to read the abstracts, which are stored under the "abstractText" attribute, we tell grep to return one line after "abstractText". 

```bash
grep -A1 abstractText test_eupmc/eupmc_results.json
```

These tools are useful in getting some first idea of the content of files, but ContentMine provides some more advanced tools in later stages of the pipeline ([ami](../ami/ami-tutorial.md)). For now we continue with more advanced queries.

### Getting pdfs and other files

PDF files can be retrieved by adding a ```-p``` flag to the query:

```bash
getpapers -q 'dinosaurs' --api eupmc -o test_eupmc -p
getpapers -q 'dinosaurs' --api ieee -o test_ieee -p
getpapers -q 'dinosaurs' --api arxiv -o test_arxiv -p
```

For every pdf found, getpapers creates a new folder containing a fulltext.pdf within the test_eupmc folder. After such a search, the folder structure looks like this:

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

Since not all queries returned PDFs, we now try another query with ```-x``` for xml-results.


```bash
getpapers -q 'dinosaurs' --api eupmc -o test_eupmc -x
getpapers -q 'dinosaurs' --api ieee -o test_ieee -x
getpapers -q 'dinosaurs' --api arxiv -o test_arxiv -x
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

Important to demonstrate EPMC filters fairly extensively, to build confidence e.g. JOURNAL:”PNAS” , FIRST_PDATE:[YYYY-MM-DD TO YYYY-MM-DD] 
permitted boolean operators etc…
‘dinosaurs’ turns out to be a nice query that gives a reasonably low number of results across IEEE, arXiv and EPMC (I think). Fun to read how the string ‘dinosaurs’ is used in IEEE papers!



## What can go wrong, how do I solve problems?

Be careful to warn participants that too general a search will result in a huge number of hits. 
Teaching Ctrl+C to cancel a getpapers search would be very useful! 
rm -rf to delete unwanted search folders would be useful to teach as well, with appropriate warnings over usage.