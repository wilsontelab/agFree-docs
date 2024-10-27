---
title: Build Conda Environments
parent: Code Installation
has_children: false
nav_order: 20
published: true
---

### Build the required conda enviroments

agFree uses [conda](https://docs.conda.io/en/latest/) environments to 
configure software dependencies whenever possible. To use agFree, you must build 
these environments as follows. 

All environments are listed, 
you may not need to build them all for your work.

`conda` must be available on your system for these commands to succeed.
To activate conda during command execution, _e.g._, on a shared compute
server resource, you many need to edit file 
`agFree/mdi/config/stage1-pipelines.yml` in your agFree installation
to set options `conda:load-command` and possibly `conda:profile-script`.

TODO: implement singularity container support

```sh
af genome conda --help
af genome conda --create    # always required
af align conda --create     # required for read alignment
af basecall conda --create  # required for ONT basecalling
```

It takes many minutes to create each environment.
Commands are typically run in an interactive shell.

Environments are placed into your installations's `environments` folder
and used from that location whenever you make a call to a pipeline.
