---
title: Library Types
parent: Sample Analysis
has_children: false
nav_order: 5
published: true
---

The agFree pipelines are general-purpose tools that can analyze read 
data from a variety of sequencing platforms generated using a variety 
of libary construction strategies, including but not limited to 
the expected agFree approaches. 

### Supported agFree sequencing platforms

The sequencing platform, communicated via option `--sequencing-platform`,
defines how reads were generated when sequencing.
Currently supported sequencing platforms are:

- **IlluminaPE** = Illumina paired end reads
- **AvitiPE** = Element Aviti paired end reads
- **AvitiSE** = Element Aviti single reads
- **Ultima** = Ultimate single reads
- **ONT** = Oxford Nanpore single reads
- **PacBio** = PacBio Revio single reads

These platforms are defined in file 
[agFree/shared/modules/agFree/platforms.csv](https://github.com/wilsontelab/agFree/blob/main/shared/modules/agFree/platforms.csv),
which can be modifed in your installation if you would like to define
a new platform (follow the pattern and comments in the file) or adjust
the default platform behaviors.

A few key properties of the different platforms are whether 
reads are expected to be single or paired reads, whether reads always
go end-to-end on the sequenced insert, expected base quality, etc.

### Supported agFree library types

The library type, communicated via option `--library-type`, 
defines how sequencing inserts were prepared prior to sequencing.
Currently supported sequencing libary types are:

- **tagFreeSR** = short reads generated by tagmentation of RE-fragmented DNA
- **tagFreeLR** = long reads generated by tagmentation of RE-fragmented DNA
- **ligFreeSR** = short reads generated by adapter ligation to RE-fragmented DNA
- **ligFreeLR** = long reads generated by adapter ligation to RE-fragmented DNA
- **otherTruSeq** = another Tru-seq compatible library prep (short read only)
- **otherTn5SR** = another short-read tagmentation libary
- **otherTn5LR** = another long-read tagmentation libary

These libary types are defined in file 
[agFree/shared/modules/agFree/library_types.csv](https://github.com/wilsontelab/agFree/blob/main/shared/modules/agFree/library_types.csv),
which can be modifed in your installation if you would like to define
a new type (follow the pattern and comments in the file) or adjust
the default libary behaviors.