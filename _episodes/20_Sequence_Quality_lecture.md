---
start: false
title: "Sequence Read Quality Lecture"
exercises: 30
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

Checking the Nanopore sequencing quality can be done using [NanoStat](https://github.com/wdecoster/nanostat) . it generates a quick summary of the number of bases, the number of reads, the length of the reads and the quality of the reads. in general we expect about 30x more bases than the size of the genome and a mean read length of >3kb. The quality can range between 11 and 18 depending on the sequencing kit, flowcell, basecalling model used.

```
$ cd ~/reads
$ NanoStat --fastq barcode02.fastq
$ NanoStat --fastq barcode03.fastq
```

Record the number of bases, the estimated sequencing depth (number of bases divided by expected size of the genome) and the mean read length in the [Google Docs file](https://docs.google.com/spreadsheets/d/1KI0KA0Rcbg3pKrFRDKikrj4Mdo5pmV60nOodNzNtZp4/edit#gid=0)

Sometimes the Nanopore output is really high. [Filtlong](https://github.com/rrwick/Filtlong) can be used to reduce the amount of read data to more manageable levels. Using this command we reduce the expected coverage to about 100x. This may not be necessary in the example.

```
$ cd ~/reads
$ filtlong -t 550000000 barcode02.fastq >filtered.barcode02.fastq
$ filtlong -t 550000000 barcode03.fastq >filtered.barcode02.fastq
```


{% include links.md %}
