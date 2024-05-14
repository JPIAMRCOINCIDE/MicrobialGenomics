---
start: false
title: "Phylogenetic trees from the core genome"
teaching: 10
exercises: 25
questions:
- "What is better, a gene presence absence tree or a tree from core genes/proteins"
- "Is there a specific clone associated with resistance"
objectives:
- "How to determine a build a tree from a set of conserved proteins"
- "Compare the tree with the gene absence presence tree"
- "Colour the tree with annotation data"
keypoints:
- "A tree can be generated from a combined set of proteins for better resolution"
---

## Building a super alignment tree

A phylogenetic tree from a single gene may be less informative because it does not contain enough information. One solution is to build a tree from a set of concatenated core genes. This is a fairly difficult exercise so this will be done step-by-step. An alternative is to have this done by Roary using the "-e -n" option, where it will use all core genes determined automatically. This takes a lot of time and may not be feasible when you are following this course instructor-led.  

## Extracting sequences of ribosomal proteins

Ribosomal proteins are generally quite stable and are not under strong diversifying selection, making them ideal candidates to infer the relationship of a set of isolates. We can use grep to extract which genes are annotated as ribosomal proteins.

~~~
$ grep "ribosomal subunit protein" gene_presence_absence.csv
~~~
{: .bash}

Thats a large output. As we only want genes present in all isolates we will extract the column with the names (column 1) and the number of isolates (column 5) and use grep to select these. We have 34 isolates that have good quality genomes so we grep for the number 34. 

~~~
$ grep  "ribosomal subunit protein" gene_presence_absence.csv |cut -f 1,4,5 -d "," |grep 34
~~~
{: .bash}

Question: how many ribosomal proteins are there? (use wc or just count them)

Next we do some shell magic to go from a column with names to a comma separated string of gene names and we put that list in the shell variable "list" . We only want proteins that occur in 34 isolates and 1 copy per genome (so 34 proteins in total per gene). 

~~~
$ list=`grep "ribosomal subunit protein" gene_presence_absence.csv | cut -f 1,4,5 -d "," |grep '"34","34"' | cut -f 1 -d "," |tr -d \" |tr "\n" "," |sed 's/,$//'`
$ echo $list
~~~
{: .bash}

To extract the gff files, we will make use of the roary script "query_pan_genome". This script can extract information from the gene presence absence file and the gff files which were already prepared before. 

~~~
$ cd ~/orthology # if you run Roary multiple times, it will make a new folder called orthology_<random number>. 
$ query_pan_genome -a gene_multifasta -n $list ~/gff/*.gff
$ ls
~~~
{: .bash}

## Aligning the proteins

Query_pan_genome has generated for every gene a file with the unaligned protein sequences ( pan_genome_results_<protein> ). These need to be aligned. Fortunately the whole list is still available as shell variable ($list), but it is comma separated. We will replace the , with spaces and then make use of a for loop to align the sequences with mafft ( https://mafft.cbrc.jp/alignment/software/ ). Mafft is a very fast aligner. 
  
  ~~~
$ cd ~/orthology
$ genes=`echo $list |tr "," " "`
$ echo $genes
$ for gene in ${genes} ; do 
    mafft pan_genome_results_"$gene".fa  |cut -f 1 -d _ > aligned_"$gene".fa
  done
$ ls
  ~~~
{: .bash} 

## Building the tree

We have now aligned all protein sequences. Furthermore, we have replaced the names of the protein names with only their isolate names (using the cut -f 1 -d _ option). This is needed as we want to concatenate every sequence with the same name using catfasta2phyml.pl (https://github.com/nylander/catfasta2phyml), followed by building a tree with fasttree (http://www.microbesonline.org/fasttree/).

~~~
$ catfasta2phyml.pl aligned* -f  > superalignment.fasta
$ FastTree superalignment.fasta > superalignment.tree
~~~
{: .bash} 

Inspect the superalignment. How many residues are in the alignment?. Also download the tree and view it using Figtree which can be downloaded here: [https://github.com/rambaut/figtree/releases](https://github.com/rambaut/figtree/releases) . An alternative is [iTOL](https://itol.embl.de/upload.cgi) . Does it look comparable to the gene presence absence tree?  Look at the reference isolate. FastTree is a tool for a quick first tree. Better tools would be RaxML (https://cme.h-its.org/exelixis/web/software/raxml/) or IQ-TREE (http://www.iqtree.org/) however these take quite some time to run.

## Visualizing phenotypes

It is possible to color the labels of the trees using the "Import Annotations" option in Figtree. Download the file with annotations here: [annotations.txt](../files/annotations.txt) and import this file. Click on the triangle next to tip labels and select "Colour by". Select "Resistance" in the dropdown box. The isolates in red are the susceptible isolates. 

Is there a specific clone associated with resistance? Inspect both the gene presence absence tree and the tree from the superalignment of ribosomal proteins. 

