---
title: ONT Basecalling
parent: File Formats
has_children: false
nav_order: 30
published: true
---

### OPTIONAL: ONT basecalling input format (POD5)

For Oxford Nanopore long-read data, the `basecall ONT` pipeline action
expects one or more 
[POD5 format files](https://github.com/nanoporetech/pod5-file-format)
as input. Briefly, POD5 files encode the electrical signal generated
by a nanopore channel for each read.

Specify the path to a directory of input POD5 files using
option `---pod5-dir`. You can also provide a tar archive of POD5 files.

All POD5 files are expected to come from a single source sample.

### OPTIONAL: ONT basecalling output format (UBAM)

The `basecall ONT` pipeline action generates unaligned bam (UBAM)
files carrying read sequences, base quality strings, and additional
metadata tags carried forward from the POD5 files.

UBAM files are placed into directory `<--output-dir>/<--data-name>/ubam`.

See the 
[SAM format specification](https://samtools.github.io/hts-specs/SAMv1.pdf)
and
[samtools documentation](https://www.htslib.org/doc/samtools.html)
for details.
