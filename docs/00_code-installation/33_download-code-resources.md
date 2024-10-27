---
title: Download Code Resources
parent: Code Installation
has_children: false
nav_order: 30
published: true
---

### Download and configure non-conda code dependencies

Some agFree software dependencies are not available via conda
and must be downloaded as needed using the following
agFree pipeline actions. Use these actions directly, or
call them using the {% include templates.md type=download %}

[singularity](https://docs.sylabs.io/guides/latest/user-guide/) 
must be available on your system for some of these commands to succeed.
To activate singularity during command execution, _e.g._, on a shared compute
server resource, you many need to edit file 
`agFree/mdi/config/singularity.yml` in your agFree installation
to set option `load-command`.

```sh
af genotype download --help
af genotype download ...  # required to build diploid reference graphs (cactus, etc.)
af basecall download ...  # required for ONT basecalling (Dorado, etc.)
af submit <jobFile>.yml
```

It takes many minutes for either command to obtain all code resources.
Commands are typically run in an interactive shell.

Containers are placed into your installations's `containers` folder
and used from that location whenever you make a call to a pipeline.

Some Oxford Nanopore code tools are placed into the `recources/nanopore` folder.
