[Official Documentation, Appendix 1](http://europepmc.org/docs/EBI_Europe_PMC_Web_Service_Reference.pdf)

[Restrict search by bibliographic metadata](#Restrict search by bibliographic metadata)

[Restrict by article metadata](#Restrict by article metadata)

[Section-level search](#Section-level search)


### Restrict search by bibliographic metadata

| Field     | Description   | Example		|
|-----------|---------------|---------------|
| `PMCID:`    | Search for a publication by its PubMed Central ID, where applicable (i.e. available as full text) | `PMCID:PMC1287967` |
| `TITLE:`    | Search for a term or terms in publication titles  					| `TITLE:aspirin, TITLE:”protein knowledgebase”` |
| `ABSTRACT:` | Search for a term or terms in publication abstracts					| `ABSTRACT:malaria`, `ABSTRACT:”chicken pox”`  |
| `AUTH:`     | Search for a surname and (optionally) initial(s) in publication author lists	| `AUTH:einstein`, `AUTH:”Smith AB”` |
| `JOURNAL:`  | Journal title – searchable either in full or abbreviated form 		| `JOURNAL:”biology letters”`, `JOURNAL:”biol lett”` |
| `LICENSE:`  | Search for content according to the assigned Creative Commons license (where provided).	| `LICENSE:"cc by" OR LICENSE:"cc-by"`, `LICENSE:cc` |
| `PUB_YEAR:` | Search by year of publication in YYYY format; note syntax for range searching. | PUB_YEAR:2000, PUB_YEAR:[2000 TO 2001] |
| `E_PDATE:` | Electronic publication date, when an article was first published online.	| E_PDATE:2013-12-15, E_PDATE:20070930, E_PDATE:[2000-12-18 TO 2014-12-30], E_PDATE:[20040101 TO 20140101] |
| `FIRST_PDATE:` | The date of first publication, whichever is first, electronic or print publication. Where a date is not fully available e.g. year only, an algorithm is applied to determine the value. 	| FIRST_PDATE:1995-02-01, FIRST_PDATE:20000101, FIRST_PDATE:[2000-10-14 TO 2010-11-15], FIRST_PDATE:[20040101 TO 20140101] |
| `P_PDATE:` | Print publication date of journal issue, when an article appeared in print format. | P_PDATE:1982-10-01, P_PDATE:20140101, P_PDATE:[2000-12-18 TO 2014-12-30], P_PDATE:[20031114 TO 20141115] | 


### Restrict by article metadata

| Field         | Description                                      | Example                                               |
|---------------|--------------------------------------------------|-------------------------------------------------------|
| `DISEASE:`      | Search for mined diseases                        | `DISEASE:dysthymias`                                    |
| `GENE_PROTEIN:` | Search for records that have GENE_PROTEINS mined | `GENE_PROTEIN:gng11`                                    |
| `GOTERM:`       | Search for records that have GOTERM mined        | `GOTERM:apoptosis`                                      |
| `CHEM:`         | Limit your search by MeSH substance              | `CHEM:propantheline`, `CHEM:”protein kinases”`            |
| `ORGANISM:`     | Search for mined organisms                       | `ORGANISM:terebratulide`                                |
| `PUB_TYPE:`     | Limit your search by publication type            | `PUB_TYPE:review`, `PUB_TYPE:”retraction of publication”` |

### Section-level search

| Field      | Description                                                          | Example                        |
|------------|----------------------------------------------------------------------|--------------------------------|
| `INTRO:`   | Find articles with a phrase in the Introduction & Background section | `INTRO:“protein interactions”` |
| `METHODS:` | Find articles with a phrase in the Materials & Methods section       |  `METHODS:“yeast two-hybrid”`  |
| `RESULTS:` | Find articles with a phrase in the Results section                   | `RESULTS:"in vivo"`            |
| `DISCUSS:` | Find articles with a phrase in the Discussion seciton                | `DISCUSS:cardivascular`        |
| `ACK_FUND:` | Find articles with a phrase in the Acknowledgements & Funding section | `ACK_FUND:ERC`				 |
