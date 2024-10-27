---
title: Build Genotype Graphs
parent: Genome Resources
has_children: false
nav_order: 20
published: true
---

### OPTIONAL: Build a diploid genotype graph for read alignment

If you have external FASTA file(s) that define the (two) haplotypes of
a (diploid) genotype from which source gDNA was obtained, 
you may build a custom genome graph for agFree read alignment. 
This helps identify and disregard clonal variants found in the genotype
but not the reference genome.

In addition to the haplotype(s), a genotype graph always includes a 
single reference genome previously obtained using `genome download`.
Ultimately, all variants are named relative that haploid reference.

Use the following command directly, or call it using the {% include templates.html type="genotype" %}.

```sh
af genotype construct --help
af genotype construct ... # `construct` because `build` is a reserved word
af submit <jobFile>.yml
```

It takes hours to build and index a genotype graph.
You will typically submit these jobs to your shared server job scheduler.

Built genotype graphs are typically placed into your installation's `recources/genotypes` folder.
Each genotype subfolder is populated with graph and index files in various formats.
