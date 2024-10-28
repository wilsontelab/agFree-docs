---
title: "agFree"
has_children: false
nav_order: 0
---

<!-- please do not alter the next line -->
{% include mdi-project-overview.md %}

> Initial beta documentation for the **agFree** tool suite.
> For developer use only.

These pages provide a step-by-step guide for using the **agFree** MDI tool suite for 
**tagFree** or **ligFree** data analysis, including:

- installing code tools
- acquiring static resources including:
    - ONT models
    - genome files
- executing one-time analyses including:
    - building simplified genotype graphs
    - performing _in silico_ digestion of genomes or genotypes
- executing iterative, sample-level analyses, including:
    - ONT basecalling (when applicable)
    - trimming adapters from ONT reads
    - aligning reads to a genome or genotype graph
    - locating observed RFLP sites in read data (ligFree only)
    - finding high-quality, error-corrected SVs and SNVs

Steps are presented in the order in which they are typically executed. 

Commands are structured in a hierarchical manner where _pipelines_ (e.g., `align`)
have one or more _actions_ (e.g., `genome`), to yield calls like `af align genome`.

We provide {% include templates.html type="" %}
to help execute all essential pipeline actions. 
It is generally easier and better to use job files to execute `agFree` pipelines
rather than constructing command lines calls, although both are possible.

<!-- please do not alter the next line -->
{% include mdi-project-documentation.md %}
