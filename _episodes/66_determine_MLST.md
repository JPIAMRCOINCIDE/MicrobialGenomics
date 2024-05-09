---
title: "MLST"
start: false
teaching: 10
exercises: 30
questions:
- "How can we detect the MLST type"
objectives:
- "What is MLST?"
- "Detect the MLST type"
- "Compare with results from other groups"
keypoints:
- "Multi Locus Sequence Typing can be used to track strains"
- "MLST resolution is not very high but it is still useful"
- "Sequence errors can lead to incomplete MLST types"
---

## MLST

Now that we have assembled our genome we are going to determine the MLST type. We are going to use a loop to cycle through the files. This is handy if you have multiple files to process.

Running it to display it on a screen:
~~~
$ mlst --list #check if there is an ecoli scheme (there are two, ecoli_achtman_4 is the correct one). 
$ cd ~/assembly
$ for sample in barcode02 barcode03
> do
>  mlst $sample/assembly.fasta
> done
~~~
{: .bash}

The output of the mlst contains many lines as it checks for every possible species. You can specify an MLST scheme using the --scheme option and you can make it less chatty using the --quiet option. You can also store the actual MLST output in a file and only display the rest of the diagnostic messages on a screen.
~~~
$ cd ~/assembly
$ for sample in barcode02 barcode03
> do
>  mlst $sample/assembly.fasta
> done > mlst_output.txt
$ cat mlst_output.txt
~~~
{: .bash}

> ## Challenge: Why are some MLST types not found. Compare with your classmates Are they still the same strain?
>
> Find out what the allele differences are. If there are new alleles, try to get the novel allele sequences.  
> 
>
> Hint:
> ~~~
> $ run mlst --help for a hint
> ~~~
> 
> 
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/assembly/barcode02/
> > $ mlst --scheme ecoli_achtman_4 --novel novel_alleles.txt assembly.fasta
> >  
> > 
> > 
> > 
> > 
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Do you have novel alleles? what could be the reason? Compare with your sequence coverage. Why is it good to run specifically the E. coli scheme? what happens if you don't? 


{% include links.md %}
