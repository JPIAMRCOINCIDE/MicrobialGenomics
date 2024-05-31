---
start: false
title: "Bacterial GWAS"
teaching: 0
exercises: 90
questions:
- "Which genes are associated with resistance"
objectives:
- "Determine genes significantly associated with resistance and speculate why"
keypoints:
- "Contigency testing for gene presence absence to associate a genotype with a phenotype, similar to GWAS in clinical genetics is possible with bacterial genomes"
---

## Introduction

As mentioned in the first part of this exercise series, the 34 *E.coli* isolates were from resistant bacteria. We will therefore try to find out if there are specific genes that are associated with resistance. 

### Bacterial Genome Wide Association Studies (GWAS)

An excellent primer on bacterial GWAS is available here: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3743258/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3743258/) . We will make use of the tool "Scoary" [https://github.com/AdmiralenOla/Scoary](https://github.com/AdmiralenOla/Scoary), a tool complementary to Roary. Scoary has implemented several GWAS methods into one tool. Scoary needs a **comma** separated list of strain characteristics or phenotypes called traits.csv. 

Copy and paste the columns with the names and resistance from the tab "annotations.txt" in a file from the assembly statistics excel file, including the "Name<tab>Resistance" header. Any texteditor can be used but in this example nano is used. You can also get the file from the previous lesson. The file can also be made on your own computer and uploaded using your webbrowser. The header (first line) should always start with "Name" followed by the phenotype (Resistance). 

~~~
$ cd ~/orthology
$ nano annotations.txt #paste the sheet annotations.txt into the file. please remove any empty lines at the bottom of the textfile
$ cat annotations.txt |tr "\t" "," > traits.csv
~~~
{: .bash}

We can now combine our gene presence absence table with the patient data and check if there are indeed specific genes associated with resistance. We're using the special keyword "ALL" the include all annotation data. The P-value cutoff is 0.05 to limit the output

~~~
$ cd ~/orthology
$ scoary -t traits.csv -g gene_presence_absence.csv  -p 0.05 --include_input_columns ALL
~~~
{: .bash}


> ## Discussion: Which genes are associated with resistance. Where are they located? what is their function?
> Download the file resistance_<date>.results.csv and open in Google Spreadsheet or Excel or any other spreadsheet tool. If you have Dutch regional settings, Excel won't recognize the file properly, you may have to adjust these (or use Google Sheets). Investigate the outcome,. Is there something special about the order and location of the genes?  Also: Remember the figtree visualization of the tree and the coloring of the phenotypes? Generate a similar annotation file with the locus you think is involved with resistance and display it in figtree. Does a specific clone carry the gene(s)? 
{: .discussion}

