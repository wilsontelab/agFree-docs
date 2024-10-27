---
title: Sample Analysis
has_children: true
nav_order: 30
published: true
---

## {{page.title}}

The following sequence describes, in order, the logical steps of agFree sample 
data analysis as organized into the tool suite's pipelines and actions.

For nanopore read analysis only:

    - ONT basecalling
    - trimming ONT reads

For all input read types:

    - aligning reads to a genome or genotype graph
    - locating observed RE RFLP sites in read data (ligFree only)
    - finding high-quality, error-corrected SVs and SNVs
