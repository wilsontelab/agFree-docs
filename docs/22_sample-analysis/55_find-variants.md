---
title: Find Variants
parent: Sample Analysis
has_children: false
nav_order: 40
published: true
---

### Analyze aligned reads for structural variants (SVs) and single-nucleotide variants (SNVs)

Armed with read alignments and site lists, you will now examine reads for evidence
of chimeras and other artifacts to perform RE-based error correction. 

Use the following commands directly, or call them using the {% include templates.html type="analyze" %},
to convert alignments into fragment maps (required) and parse those maps
into lists of SVs and/or SNVs.

```sh
af analyze --help
af analyze fragments --help
af analyze fragments ... # first required action for both SVs and SNVs
af analyze SVs ...       # then analyze either SVs, SNVs, or both (as needed)
af analyze SNVs ...      # SNV analysis does not apply to ONT reads with low base quality
af submit <jobFile>.yml
```

It takes minutes (long-read) to hours (short-read) to map and characterize fragments.
You will typically submit these jobs to your shared server job scheduler.

Various output files are placed in `<--output-dir>/<--data-name>`.
