---
title: "plasmid typing and classification"
start: false
teaching: 10
exercises: 30
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
> Find out where the genes are located. Pick a resistance gene or point mutation, and see if you can figure out if it is on the chromosome or on a plasmid. 
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

Where are the genes and where are the point mutations? Can you try other Abricate databases? e.g. VFDB that checks for virulencefactors or ecoh that checks for E. coli serotype genes.


{% include links.md %}
