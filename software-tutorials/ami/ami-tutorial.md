![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is ami?

ami is a collection of plugins that .... The better question would be: What do the different plugins do?


[1. Installation](#installation)



### Installation

norma can be installed from the latest `.deb`-file, [Download here](https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb)

```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

The tutorial has been developed on 
```
$ norma --version
norma(0.1.8)
```

### How to use ami-plugins

### How to interpret ami-results

#### ami2-bagOfWords


#### ami2-species


minimum
```bash
$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species
```

options
```
--sp.type genus binomial genussp
```

#### ami2-regex


How to construct a regex query

https://github.com/ContentMine/ami/blob/master/regex/agriculture.xml


#### ami2-gene


only type human available at the moment
```bash
ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type human
```

#### ami2-sequence



### Next steps


**Next steps**
* Continue to [sHTML](../sHTML/sHTML-overview.md) if you want to learn more about scholarly HTML.
* Continue to [ami](../norma/ami-tutorial.md) for the next step of the ContentMine pipeline.

