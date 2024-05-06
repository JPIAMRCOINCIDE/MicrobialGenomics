---
title: "Inspecting sequence graphs"
teaching: 20
exercises: 70
questions:
- "Can we find out which scaffolds or contigs are connected?"
objectives:
- "Find a circular contig"
- "Figure out the copy number"
- "To which contigs is this contig connected "
keypoints:
- "A genome assembly can be fragmented because of repeats in the genome. The assembly graph display possible connections between contigs."
---

## Introducing the sequence graph

In the assembly lecture the concept of a sequence graph was introduced. It is possible to view this graph using the tool Bandage [https://rrwick.github.io/Bandage/](https://rrwick.github.io/Bandage/). You can download the file in the link, unzip it, and follow the installation instructions. 

### Examining the file containing the sequence graph

View the sequence graph using less:

~~~
$ cd ~/assembly/barcode02/
$ ls
$ less -S assembly_graph.gfa
H       VN:Z:1.0
S       edge_1  CGCTATCGAAGCGTATGTTTCTATTGCCGCTACGCCGATCACTGACGCTTGCGCACTGAAAGCCGTGACCATGATTGCCGAAAACCTGCCGTTAGCCGTTGAAGATGGCAGTAATGCGAAAGCGCGTGAAGCAATGGCTTA>
S       edge_2  AACAAGGAATATAGATATAACAGTCACCATAAAGAAATAAACTACTGTTGTGAGAGTTATATTTCCGCTTGATGGGACTGTTTGCTCTACAAAGCTGATGACGAAGCTGAACGATATACTATAAAAAAGATTTAGCAATAT>
S       edge_3  TGTTCAGATGGTCGCTGATGCACAGCGTTCGCGTGCCCCCATCCAGAGAATGGCTGACAGCGTTTCAGGCTGGTTTGTTCCTCTGGTGATACTTATCGCGGTTGTTGCTTTCGTGATCTGGTCTGTCTGGGGGCCCGAGCC>
S       edge_4  CTGTATCGCGCCAATCGCCTGTTTATGGAGGCGTATACCGAAATTCTGGAACTGGATTCCTGCTTCAAAGATTGAGAAGGAGACGAGAATGTACGGAACATGCGAAACGCTGTGCCGGGAGCTGGCAGTAAAGTATCCGGG>
L       edge_1  +       edge_1  +       0M      RC:i:18
L       edge_2  +       edge_2  +       0M      RC:i:47
L       edge_3  +       edge_3  +       0M      RC:i:41
L       edge_4  +       edge_4  +       0M      RC:i:34
P       contig_1        edge_1+ *
P       contig_2        edge_2+ *
P       contig_3        edge_3+ *
P       contig_4        edge_4+ *

~~~
{: .bash}

Can you understand how the sequence graph works? Look at the text. In the example all 4 contigs are only connected to themselves. What does that mean?

###  Visualizing the sequence graph

Download a the sequence graph from your assembly. Open it in Bandage. Click on Draw graph. What does this remind you of?
Click on a few contigs. What is the average sequencing depth? 

### Selecting a smaller circular contig

Now, we will select a contig which is small and circular (if present). Find a contig which is between 5 and 100 kb. 

### Investigating a contig with high depth using Bandage and BLAST

Click on this contig. What do you think this contig is?  In the "Output" menu, at the top, the sequence of the selected contig can be copied to the clipboard. Figure out what the contig codes for using BLASTN [https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome).  Also record the contig number. Can you calculate the number of copies of this contig compared to the rest of the genome?

### Where are these sequences located?

Although the sequence is a separate contig in the fasta file, we can figure out to which contigs it is connected if it is not a circular contig. We could for instance investigate the sequence graph in a text editor. Alternatively, we select scope "Around nodes" and fill in the number of our selected contig. Next, we fill in 4 for the distance and press Draw Graph. What do you think this does? Try to figure out where the contig of interest is connected to. What can you do with this kind of information?


