---
title: Read Alignment
parent: File Formats
has_children: false
nav_order: 40
published: true
---

### Read alignment input format (FASTQ, UBAM, or SRA)

The `af align` pipeline actions will accept any of the following input read file formats:

- FASTQ, suffixed with extension `*.fastq.gz`
- UBAM, suffixed with extension `*.unaligned.bam` (e.g., as generated by `af basecall ONT`)
- SRA, from the [NIH Sequence Read Archive](https://www.ncbi.nlm.nih.gov/sra), suffixed with extension `*.sra`

Reads may be spread across one or more files corresponding to a single sample. 

For paired-end reads, there should be paired FASTQ files
or a single SRA file with interleaved reads (UBAM is not currently supported for paired reads).

Place all files into a single directory provided as option `--read-file-dir`
and use options `--read-file-prefix` and `--read-number-format` if you need to
further instruct agFree how to parse your file names. 

### Read alignment output format (BAM)

The `align` pipeline actions write read alignments into a single name-sorted
BAM file named `<--output-dir>/<--data-name>/<--data-name>.<--genome>.name.bam`.
The file is initially name sorted to support robust SV parsing.

To preserve disk space, the SEQ and QUAL fields are set to the null value '*'
when a read aligned perfectly to a genome or genotype with no variants.

If desired, platform QNAME values can be overridden to integer read indices by 
setting option `--drop-platform-names`. Regardless, QNAME values are always appended by the following
colon-delimated suffixes:
- **channel** = the nanopore channel that generated the read, 0 otherwise
- **trim5** = # of bases trimmed from the read 5' end
- **trim3** = # of bases trimmed from the read 3' end
- **mergeLevel** = 2 for merged read pairs, 0 otherwise
- **nRead1** = the number of read1 bases present in a final merged read (0 for single, unmerged or orphan reads)
- **nRead2** = the number of read2 bases present in a final merged read

In addition to the standard SAM format fields, the BAM files contain the following tags:
- `NM:i:`, `AS:i:`, `de:f:` and `cs:Z:` as defined in the [minimap2 documentation](https://lh3.github.io/minimap2/minimap2.html)
- agFree custom tags (some are only present for genotype alignment):
    - `xf:i:` = a bit-encoded flag indicating whether and where read (pairs) had S(N)Vs
    - `xh:i:` = flag with value 1 (haploid reference is best), 2 or 4 (one haplotype is best), or 6 (both haplotypes equally better than reference)
    - `xi:i:` = score improvement on the best haplotype (if any)
    - `xu:i:` = unmasked flag, i.e., the original reference-only flag

Note the presence of the `cs:Z:` tag in addition to the CIGAR field to 
support complete alignment and thus SNV parsing.

See the 
[SAM format specification](https://samtools.github.io/hts-specs/SAMv1.pdf)
and
[samtools documentation](https://www.htslib.org/doc/samtools.html)
for details.
