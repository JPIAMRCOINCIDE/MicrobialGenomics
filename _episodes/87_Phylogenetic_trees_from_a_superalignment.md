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
- "A tree can be generated from a combined set of proteins for better resolution. More proteins = more resolution"
---

## Building a super alignment tree

A phylogenetic tree from a single gene may be less informative because it does not contain enough information. One solution is to build a tree from a set of concatenated core genes. This is a fairly difficult exercise so this will be done step-by-step. An alternative is to have this done by Roary using the "-e -n" option, where it will use all core genes determined automatically. This takes a lot of time and may not be feasible when you are following this course instructor-led.  

## Extracting sequences of ribosomal proteins

Ribosomal proteins are generally quite stable and are not under strong diversifying selection, making them ideal candidates to infer the relationship of a set of isolates. We can use grep to extract which genes are annotated as ribosomal proteins.

~~~
$ grep "ribosomal subunit protein" gene_presence_absence.csv
~~~
{: .bash}

Thats a large output. As we only want genes present in all isolates we will extract the column with the names (column 1) and the number of isolates (column 5) and use grep to select these. We have 33 isolates that have good quality genomes so we grep for the number 33. 

~~~
$ grep  "ribosomal subunit protein" gene_presence_absence.csv |cut -f 1,4,5 -d "," |grep 33
~~~
{: .bash}

Question: how many ribosomal proteins are there? (use wc or just count them)

Next we do some shell magic to go from a column with names to a comma separated string of gene names and we put that list in the shell variable "list" . We only want proteins that occur in 33 isolates and 1 copy per genome (so 33 proteins in total per gene). 

~~~
$ list=`grep "ribosomal subunit protein" gene_presence_absence.csv | cut -f 1,4,5 -d "," |grep '"33","33"' | cut -f 1 -d "," |tr -d \" |tr "\n" "," |sed 's/,$//'`
$ echo $list
~~~
{: .bash}

To extract the protein sequences from the gff files, we will make use of the roary script "query_pan_genome". This script can extract information from the gene presence absence file and the gff files which were already prepared before. 

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
$ cd ~/orthology
$ catfasta2phyml.pl aligned* -f  > superalignment.fasta
$ FastTree superalignment.fasta > superalignment.tree
~~~
{: .bash} 

If you have made the core gene alignment using the -e -n option in Roary (see previous exercise), then we can build the super alignment tree from all core genes. This may take some time.

You can choose two methods:

Quick method. Less precise. This extracts only the SNPs and uses FastTree in the fastest setting
~~~
$ cd ~/orthology_en
$ snp-sites core_gene_alignment.aln > snpsites.core_gene_alignment.aln
$ FastTree -nt -fastest snpsites.core_gene_alignment.aln > snpsites.core_gene_alignment.tree
~~~
{: .bash} 

OR

More precise method. Can be very slow. This uses the complete core gene superalignment
~~~
$ cd ~/orthology_en
$ FastTree -nt core_gene_alignment.aln > core_gene_alignment.tree
~~~
{: .bash} 

Inspect the superalignment. How many bases are in the alignment?. Also download the tree and view it using Figtree which can be downloaded here: [https://github.com/rambaut/figtree/releases](https://github.com/rambaut/figtree/releases) . An alternative is [iTOL](https://itol.embl.de/upload.cgi) . Does it look comparable to the gene presence absence tree?  Look at the reference isolate. FastTree is a tool for a quick first tree. Better tools would be RaxML (https://cme.h-its.org/exelixis/web/software/raxml/) or IQ-TREE (http://www.iqtree.org/) however these take quite some time to run.

## Visualizing phenotypes

It is possible to color the labels of the trees using the "Import Annotations" option in Figtree or the "Datasets" option in [https://itol.embl.de/](iTOL) (use the paste function in the raw data table). Download the file with annotations here: [annotations.txt](../files/annotations.txt) and import this file. Click on the triangle next to tip labels and select "Colour by". Select "Resistance" in the dropdown box. The isolates in red are the susceptible isolates. 

Is there a specific clone associated with resistance? We have seen it already in the gene presence absence tree. Inspect both the gene presence absence tree and the tree from the superalignment of ribosomal proteins/all core genes. 

