---
published: true
---

These pages provide a step-by-step guide for using the **agFree** repository for 
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
    - trimming ONT reads
    - aligning reads to a genome or genotype graph
    - locating observed RE RFLP sites in read data (ligFree only)
    - finding high-quality, error-corrected SVs and SNVs

Steps are presented in the order in which you will typically execute them. 

Commands are structured in a hierarchical manner where _pipelines_ (_e.g._, `align`)
have one or more _actions_ (_e.g._, `genome`), to yield calls like `af align genome`.

We provide 
[job file templates](44_job-file-templates) 
to help execute all essential pipeline actions. 
It is generally easier and better to use job files to execute agFree pipelines
rather than constructing command lines calls, although both are possible.
