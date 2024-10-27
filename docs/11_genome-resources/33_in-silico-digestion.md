---
title: In Silico RE Digestion
parent: Genome Resources
has_children: false
nav_order: 30
published: true
---

### Perform _in silico_ restriction digestion of a genome or genotype graph

A first step toward matching alignments to expected 
restriction enzyme (RE) sites is _in silico_ digestion of a relevant 
reference genome or genotype graph obtained above. Genome digestion is done for many 
REs simultaneously, wheres genotype digestion is done one RE at a time.

Use the following commands directly, or call them using the {% include templates.html type="digest" %}.

```sh
af genome digest --help
af genome digest ...    # for haploid reference genomes
af genotype digest ...  # for diploid phased genotypes
af submit <jobFile>.yml
```

It takes minutes to a half hour to digest a genome or genotype.
You will typically submit these jobs to your shared server job scheduler.

Digestion outputs include site lists and genome lookup
indices placed into the pre-existing genome or genotype folder. Genome digestion
also places a data package in `<--output-dir>/<--data-name>` for visualization 
in a Stage 2 app.

### tagFree ONLY: Index the _in silico_ RE sites for use in tagFree data analysis

If you are analyzing tagFree libraries, you must further create a static index 
of each genome's _in silico_ sites obtained above for each relevant RE
(as opposed to using the `locate` pipeline in ligFree, as described below).

Site indexing is performed automatically during genotype _in silico_ digestion,
so this section only applies to haploid reference genomes.

Use the following command directly, or call it using the {% include templates.html type="digest" %}.

```sh
af genome index --help
af genome index ...
af submit <jobFile>.yml
```

It takes a few minutes to index one RE's sites for a given genome.
You will typically submit these jobs to your shared server job scheduler,
but could do it in an interactive shell. 

Indices are placed in the pre-existing genome folder.

You could use `genome index` output as `--re-sites-dir` for ligFree analysis,
but using post-alignment `locate`, below, provides better RFLP sensitivity.
