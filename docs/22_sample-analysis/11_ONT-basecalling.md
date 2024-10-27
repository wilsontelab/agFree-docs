---
title: ONT Basecalling
parent: Sample Analysis
has_children: false
nav_order: 10
published: true
---

### OPTIONAL: Perform offline ONT basecalling

If you are analyzing ONT long-read data, you may want to perform
offline basecalling with the highest accuracy 'sup' Dorado model.
The command below performs basecalling plus customized adapter trimming
that benefits from the knowledge that ligFree reads should fuse adapters
to blunt RE half-sites.

Use the following command directly, or call it using the [ONT job file templates](44_job-file-templates/ONT).

```sh
af basecall ONT --help
af basecall ONT ...
af submit <jobFile>.yml
```

It takes many hours to days to basecall large ONT data sets with the sup model.
You will typically submit these jobs to your shared server job scheduler
or use another compute resource with dedicated GPUs.

Basecalling places unaligned bam (ubam) files into <--output-dir>/<--data-name>/ubam.

### OPTIONAL: Trim adapters from pre-basecalled ONT reads

If you performed live ONT or other basecalling, you should trim adapters from
those FASTQ files in a RE-aware manner as described above.
The `trim` pipeline can also report a summary of ONT trimming results from ubam.

Use the following commands directly, or call them using the [ONT job file templates](44_job-file-templates/ONT).

```sh
af trim --help
af trim ONT --help
af trim ONT ...
af trim report ...
af submit <jobFile>.yml
```

It takes minutes to hours to trim ONT reads, depending on your data yield.
You will typically submit these jobs to your shared server job scheduler.

Output is ubam similar to basecalling, above.
