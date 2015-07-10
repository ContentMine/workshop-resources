![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is getpapers?
<!-- (describe core functionality) -->

Introduction to ctree structure probably appropriate in this module.
distinction to quickscrape

getpapers is together with [quickscrape](quickscrape.md) one of the entry points of the ContentMine pipeline. getpapers can fetch article metadata, fulltexts (PDF or XML), and supplementary materials. It's designed for use in content mining, but you may find it useful for quickly acquiring large numbers of papers for reading, or for bibliometrics.

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

### How to construct a query for EPMC

Important to demonstrate EPMC filters fairly extensively, to build confidence e.g. JOURNAL:”PNAS” , FIRST_PDATE:[YYYY-MM-DD TO YYYY-MM-DD] 
permitted boolean operators etc…
‘dinosaurs’ turns out to be a nice query that gives a reasonably low number of results across IEEE, arXiv and EPMC (I think). Fun to read how the string ‘dinosaurs’ is used in IEEE papers!

A basic query consists of free text, without any further specification. Please note, that a very **generic query** will result in a **huge number of results**. Unless intended, you can cancel a search with ```Ctrl+C``` in the command line.

A minimum query consists of ```-q 'query terms' -o outdirectory```. 

```getpapers -q 'molecuar biology' -o test```

This query would result in around 200.000 papers to be downloaded, so we cancel with ```Ctrl+C``` and refine our query. Without a specification of the API, getpapers will chose EuropePMC.

We will now compare the results of a query for *dinosaur biology* on EuropePMC, IEEE and ArXiv.

```bash
getpapers -q 'dinosaur biology' --api eupmc -o test_epmc
getpapers -q 'dinosaur biology' --api ieee -o test_ieee
getpapers -q 'dinosaur biology' --api arxiv -o test_arxiv
```



## What can go wrong, how do I solve problems?

Be careful to warn participants that too general a search will result in a huge number of hits. 
Teaching Ctrl+C to cancel a getpapers search would be very useful! 
rm -rf to delete unwanted search folders would be useful to teach as well, with appropriate warnings over usage.