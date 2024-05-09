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

Introduction to the dataset is given in the [presentation](https://klif.uu.nl/klif/coincide/coincide_microbialgenomics_introduction.pdf). In total we will be analyzing 40 E. coli genomes sequencing using Nanopore.

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

You will see you have created the folder reads. Next we need to get the appropriate files from the server. Go to the folder /mnt/netappits/users/courses/coincide/ using the terminal. You will need one folder for each sample. In the example I have picked the top two, but please take a look at the  the [Google Sheets table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit?usp=sharing) and write your name in the appropriate field to find out which two samples are assigned to you.

The MinKNOW software of Nanopore often generates several "barcode" folders of the samples you sequenced. Each barcode corresponds to one sample. In the folder you will find several files, each has several thousand nanopore reads. We need to combine these reads into a single file so that we can process these further. The command we will be using is [zcat](https://manpages.debian.org/testing/zutils/zcat.1.en.html). This commands combines unzipping a file with displaying it. We will redirect (>) the output into a new file which we can use.

~~~
$ cd /mnt/netappits/users/courses/coincide/
$ ls
$ zcat barcode02/*.fastq.gz > ~/reads/barcode02.fastq
$ zcat barcode03/*.fastq.gz > ~/reads/barcode03.fastq
$ cd ~/reads/
$ ls
~~~

Some sequencing methods have one folder or one file (Nanopore) and others have two files (Illumina). Why is that? Please continue on with the next part of the course which is a lecture on sequence quality of read files. 
