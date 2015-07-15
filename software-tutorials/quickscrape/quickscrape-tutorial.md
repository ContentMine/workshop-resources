

## What is quickscrape?


[1. Installation](#installation)

[Summary](#summary)



### Installation

### Scraper definitions

**Quickscrape is not complete without the scraper definitions**. They are developed individually for each journal to accustom for different page layouts or html-tags.

You can download the newest scraper definitions with this command:
```bash
git clone https://github.com/ContentMine/journal-scrapers.git
```
The scraper definitions will then be found in `your_path/journal-scraper/scrapers/`. You need to remember this path when using quickscrape. 

There are two possible inputs for quickscrape, a single url, or list of urls. The single url may be passed as a parameter from the command line, the list of urls may be curated either manually by you, or may be taken from a basic getpapers query.

### Single URL and URL-lists



### Results

The output we get from a basic getpapers query returns only metadata (e.g. `getpapers -q 'dinosaurs' --api eupmc -o test_eupmc`). The search results folder contains a *apiname*_results.json, which contains metadata about the search results, and a fulltext_html_urls.txt, which contains, well, URLs of hopefully fulltext papers.

```
test_eupmc
├─ eupmc_results.json
└─ fulltext_html_urls.txt
```

We can now take the fulltext_html_urls.txt as input for quickscrape, which will then visit every URL in this list and grab everything sensible it can. This includes section tags, images, or tables, but depends heavily on the format that is provided by the publishers. At the moment there exist definitions for following journals

```bash
$ quickscrape -r test_eupmc/fulltext_html_urls.txt -d journal-scrapers/scrapers/ -o test_eupmc-qs
```




## Summary and next steps

* A minimum query consists of a URL (or URL-list) and the path to a specific scraper (or a folder containing scraper definitions).
* Please be a respectful and responsible miner and apply a reasonable rate limit `-r` (recommended between 3 and 6).
* The result will be a collection of folders, 

* Continue to [journal-scrapers](../quickscrape-tutorial.md) for an introduction to scraping, and how to define your own scraper.
* Continue to [norma](../norma-tutorial.md) for the next step of the ContentMine pipeline.
* Continue to [ctree](../ctree-introduction.md) for an introduction of the main datastructure.