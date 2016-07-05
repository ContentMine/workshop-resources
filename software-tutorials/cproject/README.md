![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

# What is a CProject?

This page documents the current implementation of a CProject as of May 16, 2016.

The CProject is a naming convention for interacting with input und output files of ContentMine tools. A CProject is a filesystem tree containing a set of folders and files with reserved names. Any part of the the tool kit (quickscrape, getpapers, norma, ami, or custom downstream processing tools) will read and write to a CProject.

The structure of a CProject is being [discussed here](https://github.com/ContentMine/cmine/issues/10). Please join the debate with your suggestions and observations.

# How does a CProject look like?

#### Quickscrape

An example output folder after scraping two random papers one open access one not.

```
output
├── http_ijs.microbiologyresearch.org_content_journal_ijsem_10.1099_ijsem.0.001085
│   └── results.json
└── https_elifesciences.org_content_5_e10647v3
    ├── fulltext.pdf
    ├── fulltext.xml
    └── results.json
```

#### getpapers

An example output folder showing two papers from EuPMC. Notice that in this case the results file eupmc_results.json is a big blob for the whole project rather than one in each folder like in quickscrape. I'm not sure if this is a good thing because it means that the folder per-paper structure is missing a chunk on information. It has to be kept in the right CProject so that this data isn't lost. This could all be split up placed in a results.json like quickscrape. It also means that the data is stored in a file which changes name based upon which api it was obtained from.

```
output
├── eupmc_fulltext_html_urls.txt
├── eupmc_results.json
├── PMC4683095
│   └── fulltext.xml
├── PMC4690148
     └── fulltext.xml
```

#### ami

If you run a plugin (e.g. `ami2-gene --g.gene --g.type human --project malaria`) it results in a tree like

```
└── PMC4831192
    ├── fulltext.xml
    ├── results
    │   └── gene
    │       └── human
    │           └── empty.xml OR results.xml
    └── scholarly.html
```

After running all plugins from `ami` via the `cmine` command, the full extent of a CProject looks like like:

```
├── PMC4831192
│   ├── fulltext.xml
│   ├── gene.human.count.xml
│   ├── gene.human.snippets.xml
│   ├── results
│   │   ├── gene
│   │   │   └── human
│   │   │       └── empty.xml
│   │   ├── sequence
│   │   │   └── dnaprimer
│   │   │       └── empty.xml
│   │   ├── species
│   │   │   ├── binomial
│   │   │   │   └── results.xml
│   │   │   └── genus
│   │   │       └── empty.xml
│   │   └── word
│   │       └── frequencies
│   │           ├── results.html
│   │           └── results.xml
│   ├── scholarly.html
│   ├── sequence.dnaprimer.count.xml
│   ├── sequence.dnaprimer.snippets.xml
│   ├── species.binomial.count.xml
│   ├── species.binomial.snippets.xml
│   ├── species.genus.count.xml
│   ├── species.genus.snippets.xml
│   ├── word.frequencies.count.xml
│   └── word.frequencies.snippets.xml
├── sequence.dnaprimer.count.xml
├── sequence.dnaprimer.documents.xml
├── sequence.dnaprimer.snippets.xml
├── species.binomial.count.xml
├── species.binomial.documents.xml
├── species.binomial.snippets.xml
├── species.genus.count.xml
├── species.genus.documents.xml
├── species.genus.snippets.xml
├── word.frequencies.count.xml
├── word.frequencies.documents.xml
└── word.frequencies.snippets.xml
```
