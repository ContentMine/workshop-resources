![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is ami?

ami is a collection of plugins that .... The better question would be: What do the different plugins do?


[1. Installation](#installation)



### Installation

ami can be installed from the latest `.deb`-file, [Download here](https://jenkins.ch.cam.ac.uk/view/AMI2/job/ami-plugin/org.xml-cml$ami2/lastSuccessfulBuild/artifact/org.xml-cml/ami2/0.1-SNAPSHOT/ami2-0.1-SNAPSHOT.deb)

```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/ami-plugin/org.xml-cml$ami2/lastSuccessfulBuild/artifact/org.xml-cml/ami2/0.1-SNAPSHOT/ami2-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

### How to use ami-plugins


#### ami2-species


minimum: `$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species --sp.type [genus|binomial|genussp]`


#### ami2-regex


How to construct a regex query

https://github.com/ContentMine/ami/blob/master/regex/agriculture.xml


#### ami2-gene


only type human available at the moment `$ ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type [human]`


#### ami2-sequence

minimum: `$ ami2-sequence -q dinosaurs-xmls/ -i scholarly.html --sq.sequence --sq.type [dna|rna|prot|prot3|carb3]`


### How to interpret ami-results


### Next steps


**Next steps**
* Continue to [cat](../cat/cat-tutorial.md) for the next step of the ContentMine pipeline.

