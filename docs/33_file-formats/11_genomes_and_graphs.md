---
title: Genomes and Graphs
parent: File Formats
has_children: false
nav_order: 10
published: true
---

### Haploid reference genome(s) (FASTA)

The required haploid genome sequence resource file, e.g., as obtained by
`genome download`, should be in unzipped FASTA format, i.e., `<--genome>.fa`.
The path to this file is communicated via options `--genome-dir` and `--genome`.

Genome FASTA files will be automatically indexed as needed by `samtools faidx` and `minimap2`.

Genomes in a zoo of species for cross-species alignment are in similar FASTA format.

### Haplotype assemblies (FASTA)

Files providing the sequence of input phased haplotype contigs should also be in FASTA format,
provided as one FASTA file per haplotype and communicated via options
`--haplotype1-fasta` and `--haplotype2-fasta`.

### Genotype graphs (various)

The path to genotype graph files are communicated via options `--genotype-dir` and `--genotype`.

Pipeline action `af genotype construct` populates the genotype directory with a variety of 
graph file formats as created by 
[Minigraph-Cactus Pangenome](https://github.com/ComparativeGenomicsToolkit/cactus/blob/master/doc/pangenome.md),
including the `gaf.gz`, `gbz`, `gfa.gz`, `paf`, and `xg` formats.
