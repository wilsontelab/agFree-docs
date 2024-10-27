---
title: Read Alignment
parent: Sample Analysis
has_children: false
nav_order: 20
published: true
---

### Align reads to the genome or genotype using minimap2

agFree always uses `minimap2` for all read alignment as it has superior accuracy,
even compared to short read alignment with `bwa mem`, which calls more
artifactual SV junctions in low complexity regions. Proper filtering for 
high read quality ensures that `minimap2` aligns all read types quickly.

The `align` pipeline action you choose determines the type of reference
aligned to - either a linear reference genome or a genotype graph
as obtained above.

Use the following commands directly, or call them using the {% include templates.html type="align" %}.

```sh
af align --help
af align genome --help
af align genome ...    # for haploid reference genomes
af align genotype ...  # for diploid phased genotypes
af submit <jobFile>.yml
```

It takes hours to align most read data sets.
You will typically submit these jobs to your shared server job scheduler.

The output name-sorted bam file is placed into `<--output-dir>/<--data-name>`.
It has custom agFree tags and null values for SEQ and QUAL for non-variant reads.

### OPTIONAL: Realign variant reads to other species likely to contaminate libraries

Orthologous sequences from other species (food organisms, pets, laboratory animals...) 
that contaminate libraries can lead to false positive rare variant calls.
Use the following commands directly, or call them using the {% include templates.html type="align" %}.
to extract the QNAMEs of reads that align better to another genome in a 'zoo' of such organisms.
The read list is used by the `analyze` pipeline below.

```sh
af zoo filter --help
af zoo filter ...
af submit <jobFile>.yml
```

It takes hours to align most read data sets (fewer reads are aligned, but to more genomes).
You will typically submit these jobs to your shared server job scheduler.

The output QNAME list is placed into `<--output-dir>/<--data-name>` alongside the name-sorted bam.
