---
title: "Annotation"
start: false
teaching: 10
exercises: 50
questions:
- "How are proteins predicted from a DNA sequence?"
objectives:
- "Search for open reading frames"
- "Predict a protein from an open reading frame"
- "Annotate a genome"
keypoints:
- "Genome annotation includes prediction of protein-coding genes, as well as other functional genome units"
- "It often starts by identifying open reading frames"
- "Predicted sequences are further analysed with BLAST"
- "Larger DNA sequences or genomes require automated prediction and annotation"
---

## Annotation

Now that we have assembled the data into contigs the next natural step to do is annotation of the data. The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed to search for similarity to known elements, for example with BLAST.


Have a look at the 50 first lines of your genomes using head. In this example the folder with assembly barcode02 is used. This example assumes your assembly outputs from Flye are in the folder "assembly".

~~~
$ head -n50 ~/assembly/barcode02/assembly.fasta
~~~
{: .source}


Let's copy these first 50 lines of the scaffold including the header by moving over it with the mouse and pressing the left mouse button to copy.

> ## Exercise: Annotate a gene
> Go to [ORFfinder](https://www.ncbi.nlm.nih.gov/orffinder/). Paste the sequence including the header into the query 
> field. Choose the genetic code (translation table) 11. Start the search by pressing 'submit'. 
> This will open the Open Reading Frame viewer.
> 
> Search for the longest ORF. If you have found it, click on 'Mark'. Submit the longest ORF to BLAST, use the non redundant protein (nr) database.
> 
> How will this ORF be annotated? Is it a gene or something else? What does the gene do? Fill your annotation into the
> [table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit#gid=0) under the header ORF.
> 
{: .challenge}


## Automated Annotation

Now we will annotate all genomes with an automated approach. Prokka is a pipeline script which coordinates a series of genome feature predictor tools and sequence similarity tools to annotate the genome sequence or contigs. 
A range of programs are available for these tasks but here we will use [PROKKA](https://github.com/tseemann/prokka), which is a pipeline comprising several open source bioinformatic tools and databases. A newer alternative is [BAKTA](https://github.com/oschwengers/bakta), which generally gives better annotation. It however is slower. 

PROKKA automates the process of locating ORFs and RNA regions on contigs, translating ORFs to protein sequences, searching for protein homologs and producing standard output files. For gene finding and translation, PROKKA makes use of the program Prodigal. Homology searching (via BLAST and HMMER) is then performed using the translated protein sequences as queries against a set of public databases (CDD, PFAM, TIGRFAM) as well as custom databases that come with PROKKA.

The names of the contigs produced by some assemblers are quite long (Flye works well, but e.g. SPAdes makes very long contig names). PROKKA needs name which are shorter than 20 characters. We will therefore shorten these names first by making a copy of the contig files. We will make use of a for loop, which takes does three things, it gets takes the first 2 columns in your fasta file, delimited by an underscore. Then it renames the long word "NODE" to the letter "C" in you r file and then it outputs the resulting fasta file to a file called genome1.fasta or genome2.fasta. Replace genome1 and genome2 by the names of your folders. 


~~~
$ cd ~/assembly
$ for sample in barcode02 barcode03
> do
>  cut -f 1,2 -d "_" "$sample"/scaffolds.fasta | sed s/contig_/C/g > "$sample".fasta
> done
~~~

Let's see what this has done. First let's have a look at the original files.

~~~
$ head -n10 barcode02/assembly.fasta
~~~
{: .bash}

~~~
>contig_1
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

Now, let's inspect the new file

~~~
$ head -n10 barcode02.fasta
~~~
{: .bash}

~~~
>C1
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

The header was shortened  a lot. It is important that each header is still unique. We can check this by inspecting a few headers.

~~~
$ grep  ">" barcode02.fasta
~~~
{: .bash}

~~~
>C1
>C2
>C3
>C4
~~~
{: .output}

These are the last few lines of our output. Every line has a unique number.

We still need to make a new folder to contain our annotation results.

~~~
$ cd ~/
$ mkdir annotation
~~~
{: .bash}

Now we are all set to annotate our contigs with PROKKA. Again, this will run for a while.
The parameter --outdir tells PROKKA which output directory to write to. This needs to be a new directory. 
The parameter --prefix assigns the sample name as a prefix to all files. If we ommit this, all output files would have the same name. The parameter --cpus 1 tells it to use 1 cpu, the parameters --usegenus and --genus Escherichia will tell prokka to use the Escherichia annotation database.

~~~
$ cd ~/
$ for sample in barcode02 barcode03 ; do
  prokka --outdir annotation/"$sample" --prefix $sample assembly/"$sample".fasta --usegenus -genus Escherichia --cpus 1 --rawproduct --locustag $sample
done
~~~
{: .bash}

Let's check the output:

~~~
$ cd ~/annotation/
$ cat */*.gbk |head -n 100
$ cat */*.gff |head -n 400
$ cat */*.txt
~~~
{: .bash}

The .txt files contain some statistics on how many annotated genes are found. The .gbk file contains a human readable format of the annotation, the .gff file a tabular format. Please inspect these files so that you are familiar with their contents. See [https://en.wikipedia.org/wiki/General_feature_format](https://en.wikipedia.org/wiki/General_feature_format) for some more information.  


> ## Challenge: How many coding regions did PROKKA find in the contigs?
>
> Find out how many coding regions there are in the *E. coli* isolates. Enter your solution in the
> [table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit#gid=0) under the head 'Number of CDS'
>
> Hint:
> ~~~
> $ grep "CDS" 
> ~~~
> prints matching lines for each input file.
> 
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/annotation/
> > $ grep CDS */*.txt
> >  
> > barcode02/barcode02.txt:CDS: 5500
> > barcode03/barcode03.txt:CDS: 5514
> > 
> > These *E. coli* genomes contain approx 5000 coding regions.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Is your solution the same or do you get other numbers of coding regions? What could be possible explanations 
if the solution differs? 

Repeat the same exercise with the key word 'tRNA' to see how many tRNAs there are and fill into the [table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit#gid=0) under the head 'tRNA'


> ## Challenge 2: How many hypothetical proteins are in the annotation?
>
> Find out how coding regions without a function assigned there are in the *E. coli* isolates. Enter your solution in the
> [table](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit#gid=0) under the head 'hypothetical proteins'
>
> Hint:
> ~~~
> $ grep "hypothetical protein" 
> ~~~
> prints matching lines for each input file.
> 
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/annotation/barcode02
> > $ grep -c "hypothetical protein" barcode02.gbk
> >  
> > barcode02/barcode02.txt:CDS: 479
> > 
> > About 2500 E. coli genes cannot be assigned a function by Prokka. What would be a method to figure out what these genes are coding for? What could be the reason the number of hypothetical proteins is lower?
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

{% include links.md %}
