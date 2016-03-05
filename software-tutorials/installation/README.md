# Contributing

This README explains how to install the ContentMine tools on your local machine. It also explains how to contribute to the development and improvement of the source code.

## Table of contents

1. [Description](#description)
1. [Preparations](#preparations)
1. [Installing the software](#installing-the-software)
1. [Testing](#testing)
1. [Summary and next steps](#summary-and-next-steps)

## Description

There are two main functions fulfilled by ContentMine:

* Creating an initial corpus of papers to work with - this is done with [getpapers](../getpapers) and [quickscrape](../quickscrape).
* Normalizing the literature, extracting facts and aggregating them - this is done with [norma](../norma) and [ami-plugins](../ami).

It is possible to use both functions separately, here we describe how to install the tools individually.

We provide [VirtualBox images](../vms) where the tools are already installed and can be used in a sandbox environment. They come with certain restrictions (no easy transfer of data between host system and the virtual machine). If you are experienced with the [command line](../shell) and already have compiled and installed software from source code, you may want to install the tools locally, without the restrictions of the virtual machine.

## Preparations

### Requirements

getpapers and quickscrape are based on [Node.js](https://en.wikipedia.org/wiki/Node.js). norma and ami-plugin are based on [Java](https://en.wikipedia.org/wiki/Java_(programming_language)). 

#### Installing Node.js

##### Unix/Linux

You'll need Node.js (node), a platform which enables standalone JavaScript apps. You'll also need the Node package manager (npm), which usually comes with Node.js. Installing Node.js via the operating system's package manager leads to issues. If you already have Node.js installed, and it requires sudo to install node packages, that's the wrong way. The easiest way to do it right on Unix systems (e.g. Linux, OSX) is to use NVM, the Node version manager.

First, install NVM:
```
curl https://raw.githubusercontent.com/creationix/nvm/v0.24.1/install.sh | bash
```
or, if you don't have curl:
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.24.1/install.sh | bash
```

Then, install the latest Node.js, which will automatically install the latest npm as well, and set that version as the default:

```
source ~/.nvm/nvm.sh
nvm install 0.10
nvm alias default 0.10
nvm use default
```
Now you should have node and npm available. Check by running:

```
node -v
npm -v
```

##### OSX

Download and run the Mac OS X Installer provided by [Nodejs](https://nodejs.org/en/download/).

##### Windows

Download and run the Windows installer provided by [Nodejs](https://nodejs.org/en/download/).


#### Installing Java

openjdk-7-jre and openjdk-7-jdk or openjdk-8-jre and openjdk-8-jdk, or java 8 JRE + JDK

##### Unix/Linux


##### OSX


##### Windows


### Installing the software

#### getpapers

Open a command line (Windows) or terminal (Linux/OSX) and type:

```
npm install --global getpapers
```

#### quickscrape

Open a command line (Windows) or terminal (Linux/OSX) and type:

```
npm install --global quickscrape
```

#### norma

* build and install from source
* install from package

#### ami-plugin

* build and install from source
* install from package


### Testing