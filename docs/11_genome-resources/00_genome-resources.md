---
title: Genome Resources
has_children: true
nav_order: 20
published: true
---

## {{page.title}}

In addition to code and 3rd-party programs installed above,
the agFree pipelines require and/or support genome resources as follows:

- haploid reference genome downloads in a standarized format
- OPTIONAL: diploid genome graphs, i.e., genotypes
- lists of expected RE sites based on _in silico_ RE digestion of genomes or genotypes

Standard usage exploits a simple haploid reference genome,
like many other genomics pipelines. 

Alternatively, variant calling accuracy can be improved by
building and using a genome graph from two previously sequenced haplotypes
matching your gDNA source. agFree pipelines provide tools to build, index, and
digest these custom reference genotype graphs.

In many downstream steps you will specify actions by the type
of reference you are using, e.g., `align genome` vs. `align genotype`.
