---
title: Install agFree
parent: Code Installation
has_children: false
nav_order: 10
published: true
---

### Acquire and configure agFree code

Follow 
[these instructions](https://github.com/wilsontelab/agFree/tree/main?tab=readme-ov-file#quick-start-single-suite-installation-recommended) 
to
clone the agFree gitHub repository and 
install its [MDI](https://midataint.github.io/) framework dependencies.

This initial agFree code installation takes only a minute or two.

Instructions below assume you created an alias to your
agFree installation called `af. Substitute your alias or command as needed.

Notice the folder structure of your agFree installation (abbreviated):

```sh
agFree
├── mdi
│   ├── config       # system configuration files you may need to edit
│   ├── containers   # where required singularity containers are placed
│   ├── environments # where conda environments are placed
│   ├── library      # where R libraries are placed when using Stage 2 apps
│   └── resources
│       ├── genomes     # default location for downloaded genomes
│       ├── genotypes   # default location for built genotype graphs
│       ├── nanopore    # default location for downloaded ONT software and models
│       └── zoo         # default location for multi-genome alignment indices
└── run # the run command script to which `af` is aliased
```
