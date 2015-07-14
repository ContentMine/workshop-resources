

## Get or update journal scraper definitions

Quickscrape is not complete without the scraper definitions. They are developed individually for each journal to accustom for different page layouts or html-tags.

You can download the newest scraper definitions with this command:
```bash
git clone https://github.com/ContentMine/journal-scrapers.git
```
The scraper definitions will then be found in `your_path/journal-scraper/scrapers/`. You need to remember this path when using quickscrape.

start with urls.text from getpapers

The minimum is (url OR urllist) AND (scraper OR scraperdir).


for dn in $(find -type f -name "*.xml"); do 
norma -q $(dirname $dn) -i fulltext.xml -o scholarly.html --transform nlm2html; 
done
