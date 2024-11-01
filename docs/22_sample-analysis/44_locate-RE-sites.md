---
title: Locate RE Sites
parent: Sample Analysis
has_children: false
nav_order: 30
published: true
---

### ligFree ONLY: Locate RE sites not previously known from _in silico_ digestion

The exact gDNA you sequence may have restriction fragment length polymorphisms (RFLPs), i.e.,
novel RE sites not previously known from _in silico_ digestion,
as compared to even the most closely matched reference genome or genotype graph. 
Use the following commands directly, or call them using the {% include templates.html type="locate" %},
to use recurrent ligFree read 5' endpoints to infer the locations of RFLP sites.
The site list can be used for `af analyze` below.

The `af locate` pipeline action you choose must match the `af align` pipeline action used above,
i.e., either `genome` or `genotype`.

```sh
af locate --help
af locate genome --help
af locate genome ...    # for haploid reference genomes
af locate genotype ...  # for diploid phased genotypes
af submit <jobFile>.yml
```

It takes minutes (long-read) to hours (short-read) to locate RFLP sites.
You will typically submit these jobs to your shared server job scheduler.

Output is placed in `<--output-dir>/<--data-name>` and **can be re-used**
over many subsequent sample analyses from the same genotype
by providing it to `af analyze` as option `--re-sites-dir`. 
If you have access to both ligFree and tagFree data for a sample genotype,
`af locate` output can be used for tagFree analysis also.

In all agFree files, RE site coordinates are the position just after the cleaved
phosphodiester bond, where an actual site exists or where an RFLP site would have been
annotated had it existed in the genome or genotype.  So, in the example below,
base number 5, marked with `*`, is the RE site coordinate for  cleaved phosphodiester bond `|`.

```
1234 56789
----|*----
```
