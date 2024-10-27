---
title: Job File Templates
has_children: false
nav_order: 80
published: true
---

## {{page.title}}

We provide {% include templates.html type="" %} for all agFree pipeline actions.

[MDI job files](https://midataint.github.io/mdi/docs/job_config_files.html) 
are YAML-formatted option sets for one or more pipeline actions.
They are a convenient and powerful way to organize, track, and maintain a history of 
your agFree pipeline execution history.

In most use cases, we recommend using job files to make calls to agFree pipelines,
although you can also call all agFree pipeline actions from the command line or within 
a bash script. 

Note that agFree job file templates are broken down into pieces
to introduce their concepts and structures. In actual use,
you may want to use composite job files that string
calls to multiple pipelines together in a single file, 
[as described here](https://midataint.github.io/mdi/docs/job_config_files.html#multi-pipeline-job-files).

To illustrate the relationship between job files and command line options,
the following command lists all options for pipeline action `align genome`:

```sh
af align genome --help
```

and the following command shows how those options are parsed into a corresponding job file template:

```sh
af align template --all-options
```

Most options have defaults and do not need to be specified every time.
Accordingly, our {% include templates.html type="" %} are shorter than the complete option templates.

Using job file templates additionally exposes the many useful 
[MDI command line interface tools](https://midataint.github.io/mdi/docs/commands/management.html)
that you can use to monitor your work jobs.
