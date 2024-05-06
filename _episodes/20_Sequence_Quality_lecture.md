---
start: false
title: "Sequence Read Quality Lecture"
exercises: 0
teaching: 30
questions:
- "How does sequencing work"
- "Where do the errors come from"
objectives:
- "Recap sequence data quality"
- "Understanding read data"
keypoints:
- "Determining sequence quality of reads"
---

## Sequencing quality

Next up is a presentation on sequence read quality and sequencing methods. The slides are available at [https://klif.uu.nl/klif/mgen/MicrobialGenomics_sequencingQuality_Linda.pdf](https://klif.uu.nl/klif/mgen/MicrobialGenomics_sequencingQuality_Linda.pdf) .

If you are following this course on your own you can make use of a prerecorded lecture by Professor Bas Dutilh, from Theoretical Biology and Bioinformatics at UU and Jena University in Germany. [https://www.youtube.com/watch?v=sdxVDy0lSAE](https://www.youtube.com/watch?v=sdxVDy0lSAE)

The lecture will take approximately 20 minutes. After that there is time for a break and time for asking questions.

Checking the Nanopore sequencing quality can be done using [NanoStat](https://github.com/wdecoster/nanostat) . it generates a quick summary of the number of bases, the number of reads, the length of the reads and the quality of the reads. in general we expect about 40x more bases than the size of the genome and a median read length of >3kb. 

```
$ cd ~/reads
$ NanoStat --fastq barcode02.fastq
$ NanoStat --fastq barcode03.fastq
```




{% include links.md %}
