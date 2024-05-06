---
title: "Introduction"
exercises: 10
teaching: 20
questions:
- "Where does the dataset come from?"
- "How to login"
- "Where are the files located"
objectives:
- "Understand the data"
- "Choose login details"
- "Familiarize yourself with the environment"
keypoints:
- "Sequencing *E.coli*  isolates to determine assocations of bacterial genes with antibiotic resistance"
---

## Introduction

We will be making use of the command line interface on the [Jupyterhub site](https://klif2.uu.nl/). 

### Dataset

Introduction to the dataset is given in the [presentation](https://klif.uu.nl/klif/coincide/coincide_microbialgenomics_introduction.pdf). In total we will be analyzing 50 E. coli genomes sequencing using Nanopore.

### How to login

Follow the instructions at as before. 
The server we will be using has host address [Jupyterhub site](https://klif2.uu.nl/). Please login using your webbrowser. Click on "New", top left and open a "Linux Terminal". Bookmark it and give it an appropriate name so you can find it again later. 

### Where are the files located

In your home folder (~/), you may find different files. It is your own responsibility to take care of your files. We will create the folders you will be using and download the read files that are part of this study. As assembling of all the genomes in this study would be too time consuming, we will assembling only two genomes per person. We will combine the outputs of each person later on for the genome comparisons.
  
### Getting the Nanopore read files

First we need to make an appropriate folder for your read files. In the example we will be making use of the folder called "reads"

~~~
$ cd ~
$ mkdir reads
$ ls
~~~

You will see you have created the folder reads. Next we need to get the appropriate files from the server. Go to the website [klif.uu.nl/klif/coincide/reads](https://klif.uu.nl/klif/coincide/reads/) and download the appropriate files. You will need one file for each sample. In the example I have picked the top two, but please take a look at the  the [Google Sheets table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit?usp=sharing) and write your name in the appropriate field to find out which two samples are assigned to you.

~~~
$ cd ~/reads
$ wget https://klif.uu.nl/klif/coincide/reads/ERR326690_1.fastq.gz
$ wget https://klif.uu.nl/klif/coincide/reads/ERR326690_2.fastq.gz
$ ls
~~~

In the above example we have downloaded the read files. Some sequencing methods have one file (Nanopore) and others have two files (Illumina). Why is that? Please continue on with the next part of the course which is a lecture on sequence quality of read files. 
