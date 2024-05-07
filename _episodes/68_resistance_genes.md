---
title: "Resistance genes"
start: false
teaching: 10
exercises: 50
questions:
- "How can we detect the resistance genes and point mutations"
objectives:
- "Finding resistance genes"
- "Finding mutations that cause resistance"
- "Compare with results from other groups"
keypoints:
- "Resistance genes can be found on the chromosome and on plasmids"
- "Point mutations are often in essential chromosomal genes"
- "The translation from resistance genotype to phenotype is not always easy"
---

## ResFinder

[ResFinder](https://bitbucket.org/genomicepidemiology/resfinder/src/master/) is often used to find resistance genes and mutations. The output is human readable. Now that we have assembled our genome we are going to determine which resistance genes and point mutations there are. We are going to use a loop to cycle through the files. This is handy if you have multiple files to process. Resfinder is written in Python, so it must be run through that interpreter.

~~~
$ cd ~/assembly
$ for sample in barcode02 barcode03
> do
>  python -m resfinder  -s "Escherichia coli" --point  -db_point_kma /mnt/data/db/pointfinder_db/ -ifa  $sample/assembly.fasta -o $sample/resfinder
> done
$ cd barcode02
$ ls
$ cat resfinder
~~~
{: .bash}

## AMRFinderPlus

[AMRFinderPlus](https://github.com/ncbi/amr) is also used to find resistance genes and mutations. The output is in the form of a table. It is often installed in its own conda environment
Running it to display it on a screen:
~~~
$ cd ~/assembly
$ conda activate amrfinderplus
$ for sample in barcode02 barcode03
> do
>  amrfinder --organism Escherichia --nucleotide $sample/assembly.fasta > $sample/amrfinderplus.txt
> done
$ conda activate base
$ cd barcode02
$ ls
$ cat amrfinderplus.txt
~~~
{: .bash}


> ## Challenge: Are the resistance genes found on a plasmid or on a chromosome?
>
> Find out where the genes are located. Pick a resistance gene or point mutation, and see if you can figure out if it is on the chromosome or on a plasmid. Remember the assembly info file, the sequence graph analysis etc. 
> 
>
> Hint:
> Which contig is it on
> 
> 
> > ## Solution
> >
> > Investigate the assembly graph using Bandage. Was it on a smaller circular contig or a large 5 megabase contig?  
> > {: .output}
> {: .solution}
{: .challenge}

Where are the genes and where are the point mutations? 


{% include links.md %}
