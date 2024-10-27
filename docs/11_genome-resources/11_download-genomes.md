---
title: Download Genomes
parent: Genome Resources
has_children: false
nav_order: 10
published: true
---

### Download and configure genomes

All agFree analysis requires haploid reference genome resource files.

Pipeline actions simplify download of required genome
resource files that will be properly formatted for use. Use them directly, or
call them using the {% include templates.html type="download" %}.

It is possible to use preexisting genome resource files,
but you may find it problematic if they don't conform to format expectations.

```sh
af genome download --help
af genome download ...  # always required
af zoo download ...     # required to perform cross-species read alignment
af submit <jobFile>.yml
```

It takes many minutes for either command to obtain all genome resource files.
Commands are typically run in an interactive shell.

Depending on the genome you are downloading, you may be shown information
for how to manually download and rename a BED file of excluded regions.

Downloaded genome files are typically placed into your installation's `recources/genomes` folder.
