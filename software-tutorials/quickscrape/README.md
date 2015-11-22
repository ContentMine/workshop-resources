# quickscrape

## DESCRIPTION
**What does quickscrape?**

`quickscrape` is together with [getpapers](../getpapers/getpapers-tutorial.md) one of the entry points of the ContentMine pipeline. quickscrape is not like other scraping tools. It is designed to enable large-scale content mining, and retrieve PDFs, images and fulltext-htmls of scientific literature.

**Why do we need getpapers?**


**How can I use getpapers?**

**Training material created with**
- linux 14.04 LTS
- node 0.10.24
- quickscrape 0.4.1

**Glossary**

## TUTORIAL
**Table of Content**
1. [Installation](#installation)
2. [Scraper definitions](#scraper-definitions)
3. [Scraping](#scraping)
4. [Other sources](#other-sources)
5. [Summary and next steps](#summary-and-next-steps)


### 1. Installation

```bash
sudo npm install --global quickscrape
```

You can find the technical documentation for `quickscrape` in its [repository](../quickscrape/).

**Scraper definitions**

Quickscrape is incomplete without the scraper definitions. They are developed individually for each journal to accustom for different page layouts or html-tags. Scraper definitions are maintained in a [separate repository](https://github.com/ContentMine/journal-scrapers.git), and it is possible to [create your own definition](../journal-scrapers/journal-scrapers-tutorial.md).

You can download the newest scraper definitions with this command:
```bash
git clone https://github.com/ContentMine/journal-scrapers.git
```

The scraper definitions will then be found in `your_path/journal-scraper/scrapers/`. You need to remember this path when using quickscrape. 

**Help**


The command line is going to be the main interface with ContentMine. Some basic are documented [here](../shell/), but please have a look if you are new to using the command line.

Use `getpapers --help` to see the command-line help:

### 2. Scraping

There are two possible inputs for quickscrape, a single url, or list of urls. The single url may be passed as a parameter from the command line, the list of urls may be curated either manually by you, or may be taken from a basic getpapers query. quickscrape will then visit every URL in this list and grab everything sensible it can. This includes sections according to tags, images, or tables, but depends heavily on the format that is provided by the publishers. 

At the moment there exist definitions for following publishers/journals:
* BMC
* PLoS
* PeerJ
* PNAS
* elife

#### Single URLs

A minimum query consists of a URL (or URL-list) and the path to a specific scraper (or a folder containing scraper definitions). You pass the url with ```-u``` or ```--url```, the scraper definition with ```-s``` or ```--scraper``` and the output-directory with ```-o``` or ```--output```.

```bash
quickscrape -u URL -s SCRAPERDIRECTORY -o OUTPUTFOLDER
```

The scraper you use should correspond the to URL you provide, if none is available, choose `generic_open.json`.

```bash
quickscrape \
  --url https://peerj.com/articles/384 \
  --scraper journal-scrapers/scrapers/peerj.json \
  --output peerj_single-url
```

![quickscrape-url](../../resources/images/software/quickscrape/quickscrape-url.png)

Quickscrape now creates a subfolder for each searchresult, describing the article source, a fulltext.html with the scraping results, and a results.json containing metadata of the article, e.g. authors, title, abstract and bibliographic data. It may include other files such as fulltext PDFs, fulltext XMLs, or scraped images.  This is also one of the starting points for a [ctree](../ctree-introduction.md), the main datastructure of the ContentMine pipeline.

```bash
tree peerj-384/
peerj-384/
└── https_peerj.com_articles_384
    ├── fig-1-full.png
    ├── fulltext.html
    ├── fulltext.pdf
    ├── fulltext.xml
    └── results.json

1 directory, 5 files
```

#### download with getpapers URL list

To download from a list of urls, we take the output we get from a [basic getpapers query](../getpapers/getpapers-tutorial.md#construct-a-simple-query_and-compare-results) (e.g. ```getpapers -q 'dinosaurs' --api eupmc -o eupmc```), which creates exactly the file ```fulltext_html_urls.txt``` we need for this. This returns two files in a search results folder. An *APINAME*_results.json, which contains metadata about the search results, and a fulltext_html_urls.txt, which contains a list of URLs of fulltext papers. A *valid list of URLs* is a textfile with exactly one valid URL per line. A *valid URL* is a URL that leads to a fulltext page, e.g. [https://peerj.com/articles/384](https://peerj.com/articles/384). 

```bash
tree eupmc
eupmc
├── eupmc_results.json
└── fulltext_html_urls.txt

cat eupmc/fulltext_html_urls.txt 
http://europepmc.org/articles/PMC4040045
http://europepmc.org/articles/PMC4055607
http://europepmc.org/articles/PMC4022087
http://europepmc.org/articles/PMC3394943
http://europepmc.org/articles/PMC4448809
http://europepmc.org/articles/PMC3115283
```

We now take the fulltext_html_urls.txt as input for quickscrape. quickscrape will choose a scraper automatically, if one is available. If not, a scraper with very generic definitions will be used, and the result will not be as precise.

```bash
quickscrape -r eupmc/fulltext_html_urls.txt -d journal-scrapers/scrapers/ -o eupmc
tree eupmc
eupmc
├── eupmc_results.json
├── fulltext_html_urls.txt
├── http_europepmc.org_articles_PMC1234567
│   ├── fulltext.html
│   └── results.json
├── http_europepmc.org_articles_PMC1234568
│   ├── fig-1-full.png
│   ├── fig-2-full.png
│   ├── fulltext.html
│   ├── fulltext.pdf
│   ├── fulltext.xml
│   └── results.json
├──
.
.
.
└── http_europepmc.org_articles_PMC4448809
    ├── fulltext.html
    ├── fulltext.pdf
    └── results.json

19 directories, 43 files

```

### DOI's

From other searches or citation data you may have a list of DOIs ([Digital Object Identifier](https://en.wikipedia.org/wiki/Digital_object_identifier)), such as ```https://dx.doi.org/10.7717/peerj.384```. This is not a valid URL input for quickscrape. You must first resolve the DOI, in this case [https://dx.doi.org/10.7717/peerj.384](https://dx.doi.org/10.7717/peerj.384) leads to [https://peerj.com/articles/384/](https://peerj.com/articles/384/). Other examples are [http://dx.doi.org/10.4103%2F1817-1745.131497](http://dx.doi.org/10.4103%2F1817-1745.131497) which leads to the [article on pediatricneurosciences.com](http://www.pediatricneurosciences.com/article.asp?issn=1817-1745;year=2014;volume=9;issue=1;spage=79;epage=81;aulast=Vitaliti), or [http://dx.doi.org/10.1074%2Fmcp.M111.014167](http://dx.doi.org/10.1074%2Fmcp.M111.014167) which leads to the [article on MCPOnline.org](http://www.mcponline.org/content/11/7/M111.014167). It is important to distinguish between the DOI and the landing page. **Content can only be scraped from the landing page.**

### Summary

* A minimum query consists of a URL (or URL-list) and the path to a specific scraper (or a folder containing scraper definitions).
* Please be a respectful and responsible miner and apply a reasonable rate limit `-r` (recommended between 3 and 6 scrapes per minute).
* The result will be a collection of [ctrees](../ctree/ctree-overview.md) containing fulltexts in various formats (PDF, XML), a results.json with metadata, and possibly images.

## NEXT STEPS

* Continue to [journal-scrapers](../journal-scrapers/journal-scrapers-tutorial.md) if you want to define your own scraper.
* Continue to [norma](../norma/norma-tutorial.md) for the next step of the ContentMine pipeline.
* Continue to [ctree](../ctree/ctree-overview.md) for an introduction of the main datastructure.

