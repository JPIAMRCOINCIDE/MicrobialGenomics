---
title: "Plasmid typing and classification"
start: false
teaching: 10
exercises: 50
questions:
- "How can we detect which contig is a plasmid and which is from the chromosome"
objectives:
- "Classifying plasmid contigs"
- "Typing plasmids"
- "Check which plasmids contain resistance genes"
keypoints:
- "Resistance genes can be found on the chromosome and on plasmids"
- "Plasmid classification by hand is time consuming. Especially with lots of contig, automated analysis is better"
- "There are also plasmids without resistance genes"
---

## RFPlasmid

[RFPlasmid](https://github.com/aldertzomer/RFPlasmid/tree/master) is often used to classify contigs in plasmid and chromosome. RFPlasmid processes folders, therefore we don't need a loop to process files.

~~~
$ cd ~/assembly
$ mkdir ~assembly/genomes
$ cp ~/assembly/barcode02/assembly.fasta ~/assembly/genomes/barcode02.fasta
$ cp ~/assembly/barcode03/assembly.fasta ~/assembly/genomes/barcode03.fasta
$ cd ~/assembly
$ rfplasmid --specieslist
$ rfplasmid --jelly --species Enterobacteriaceae --input genomes --out rfplasmid
$ cd rfplasmid
$ cat prediction.csv
~~~
{: .bash}

An alternative for RFPlasmid is [Platon](https://github.com/oschwengers/platon). It has very similar performance to RFPlasmid and is slighlty more conservative. It can be run using the following commands:

~~~
$  platon assembly.fasta --output output --threads 2 --verbose --db /mnt/data/db/platon/db
~~~
{: .bash}

Please try it yourself. Try to find the output and open the files. Are there differences in the classification between RFPlasmid and Platon? 

## Plasmidtyping
It is useful to know which type of plasmids are present in your bacterial genome. We use [Abricate](https://github.com/tseemann/abricate) and the [PlasmidFinder](https://bitbucket.org/genomicepidemiology/plasmidfinder/src/master/) database to find the Inc types or incompability groups. Abricate has different kinds of databases it can use to query a genome. 

~~~
$ abricate --list # we check which databases are available. 
$ cd ~/assembly
$ for sample in barcode02 barcode03
> do
>  echo $sample
>  abricate --db PlasmidFinder $sample/assembly.fasta
>  echo
> done
~~~
{: .bash}


> ## Challenge: Are the resistance genes found on a plasmid or on a chromosome?
>
> Find out where the genes are located. Pick a resistance gene or point mutation from the previous exercise, and see if you can figure out if it is on the chromosome or on a plasmid using this data. Also find out the incompability group of the plasmid (the PlasmidFinder Inc result). 
> 
>
> Hint:
> Which contig is it on
> 
> 
> > ## Solution
> >
> > Investigate the RfPlasmid or Platon classification of the contig. Is the resistance gene contig on a chromosomal or plasmid contig.  
> > {: .output}
> {: .solution}
{: .challenge}

Can you try other Abricate databases? e.g. VFDB that checks for virulencefactors or ecoh that checks for E. coli serotype genes. 


{% include links.md %}
