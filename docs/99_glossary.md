---
title: Glossary of Terms
has_children: false
nav_order: 90
published: true
---

## agFree - Glossary of Terms and Identifiers

This page defines hierarchical terms used by the `agFree` data analysis pipelines.
These keywords are used throughout the code and documentation, 
so it is important to understand their precise meaning.

## Biological Sources

### species
The standard biological meaning, with corresponding base reference genomes, e.g., hg38 for human.

### genotype
A specific representative of a species, e.g., a human individual, clonal cell line, or mouse strain.
- genotypes differ from a species base reference genome on one or both alleles
- ideally, genotypes are tracked with custom references with diploid phasing (see below)

#### sample
A specific biological source of gDNA from a genotype, e.g., a tissue block or cell pool, or the corresponding gDNA.
- it is possible for samples to be admixtures of genotypes, but this adds complexity

## Sequencing Sources

### insert
One adapter-ligated gDNA fragment from a sample.

### library
A prepared collection of many inserts from a sample.
- adapter ligation to gDNA invariably creates chimeric inserts prior to sequencing!

### platform
The sequencing technology used to obtain sequence data from a library.
- different sequencing platforms provide either:
    - single reads or two paired reads per event
    - end-to-end or partial reads of library inserts
    - fixed-length or variable-length reads
- explicitly supported platforms are: Aviti, Illumina, Ultima, ONT, PacBio

### run
One time that a DNA sequencing platform was applied to one or more pooled libraries.
- sample/library/run identifiers are found as file and analysis names, read-group tags, and/or table columns
- tags and columns allow data from multiple samples/libraries/runs to be tracked while being analyzed together

### event
One sequenced library insert, as judged by the platform, e.g., a cluster, polony, trace, etc.
- QNAME in FASTQ and BAM files is the event identifier assigned by the platform
- QNAME is the same for all reads derived from a single event
- events on most platforms correspond to one insert, but ONT events can comprise two inserts fused via "follow-on"
- events/inserts should - but don't always! - correspond to a single true fragment from a sample's gDNA
- most platforms do not provide raw event data, but some do, e.g., ONT POD5 files

### read 
A series of DNA bases resulting from basecalling as applied to an event's acquired raw data.
- thus, a single, contiguous DNA sequence as initially provided by the platform (could be chimeric!)
- on some platforms, reads come in pairs per event

### orphan
One read of a pair which has lost its mate due to quality filtering or alignment failure.

## Hierarchical Sequence Relationships

### sequence
Either a single read as is, a read pair merged to a single read, or two unmerged paired reads with missing bases.
- sequence identifiers are the same as for events/reads, i.e., QNAME
- single reads and merged pairs are identifed because the PAIRED flag bit is unset
- merged pairs further have mergeLevel=2 set during alignment, while unmerged pairs with no confirmed overlap have mergeLevel=0
- in merged read pairs, overlap bases always derive from the SEQ and QUAL values of read1
- in unmerged read pairs, the strand of read2 is flipped to switch from FR to FF pairing for path continuity

### molecule
A presumptive unique library insert, after collapsing any duplicate sequences.
- collapsing ONT duplex sequences prevents overcounting two-strand sequencing as independent events
- PCR-free agFree libraries on modern sequencing platforms have ~no other duplicates to collapse
- molecule identifiers are the same as for reads and sequences
- duplicate sequences are purged and a collapsed/duplex flag is set on the remaining molecule

### contig
A portion of a molecule after splitting chimeras at ligation artifacts.
- contigs are taken as true source gDNA base strings present in a sample (until proven otherwise)
- contig identifiers are QNAME:contigN to enforce independence of chimeric molecule halves
- a contig is the unit of base sequence to which all variant calling and assembly is applied

### chimera
A sequence/molecule with two or more contigs connected by an SV artifact process.
- all platforms exhibit chimeras formed by intermolecular ligation!
- ONT exhibits additional chimeras when two inserts are mistakenly reported as one event/read
    - ligation artifact generally do not contain adapters in junctions, whereas two-insert events generally do 
- agFree libraries are PCR-free and use ddBTP during A-tailing, so DNA polymerization cannot create chimeras

### proper
An adjective describing molecules/contigs that match the genome structurally, i.e., that do not contain SV junctions.
- proper molecules are themselves RE fragment contigs 
- in agFree, proper molecules are not tabulated individually, but are aggregated and counted 

### anomalous
An adjective describing molecules/contigs that do not match the genome structurally, i.e., that do contain SV junctions.
- anomalous sequences are described in edge files where:
    - duplex purging to molecules removes duplicate sequences on the less-frequent strand
    - molecule splitting to contigs removes false chimeric junctions and renumbers segmentN, blockN, edgeN
- a primary goal of agFree analysis is to prevent chimeras from being considered as tue anomalies

## Genome Sequences and Subsequences

### reference
A general term for one or a set of haplotypes from a species to which sequencing reads are aligned.
- there are two types of references recognized by agFree: genomes and genotype graphs
- if not otherwise clarified, reference by default refers to a single haploid genome reference
- all coordinates in agFree are reported relative to a single haploid genome reference, even if aligned to a genotype

### genome
A single, specific set of linear reference chromosome sequences that represent a species, e.g., hg38.
- chromosomes/chroms are identified by their RNAME (reference name, e.g., chr17)

### (genotype) assembly
The set of (sub)chromosome-level sequence contigs that represent a genotype of a species.
- in agFree, "assembly" is not typically used to refer to haploid reference genomes, just (two) phased genotype alleles, i.e., haplotypes
- haplotype assemblies are used together with a haploid reference genome to construct a genotype graph

### graph
The non-linear, branched representation of the haplotypes found within a genotype.
- an agFree genotype graph comprises:
    - one genome reference haplotype, e.g., hs1
    - one or two genotype-specific haplotypes (usually two for a diploid genotype, for three total haplotypes)
- agFree uses graphs to create RE site maps that are projected back to the haploid
- pipelines do not currently use graphs for read alignment, favoring iterative realignment to
haplotypes that supports genotype masking of reference-alignment variants

## Restriction Enzymes and Sites

### RE, or bRE
A single, specific (blunt) restriction enzyme used to generate library inserts from a sample, e.g., EcoRV.

### site
A specific numbered position in a haploid reference genome where a RE cleaves.
- sites are tracked as the 1-referenced position AFTER the cleaved phosphodiester bond, e.g., 5'-gat|Atc
- most sites are predicted in advance by in silico digestion of the genome or genotype
- site identifiers are either (where suffix 1 denotes that values are 1-referenced):
    - RNAME:sitePos1, which defines the genomic location of the site
    - RNAME:siteIndex1, i.e, the ordered numerical index of a site on a chromosome
        - RNAME-specific lookup tables allow conversion from siteIndex1 to sitePos1

### RFLP
A genotype or sample-specific site inferred from examining a sample's read data.

### fragment
A portion of a haplotype released by cleavage at sites/RFLPs by digestion with a RE.
- most contigs are expected to match a single RE fragment
- some contigs result from partial RE digestion and thus have an internal RE site
- rare contigs/inserts derive from non-RE, randomly sheared fragments, which are discarded
- the uninformative, outermost, telomeric fragments on chromosomes are ignored/lost during analysis
- fragments identifiers are RNAME:\<leftmost siteIndex1 along chrom\>, i.e.,
```
--unused--|--frag1--|--frag2--|--unused--  (numbering resets on each chrom)
        site1     site2     site3
```
### partial
Incomplete digestion of gDNA with a RE, resulting in uncleaved sites in multi-fragment contigs.
- do not confuse partial digestion with incomplete reads

### end-to-end
An adjective describing a sequence that spanned to include both RE-cleaved ends of an insert, i.e., a complete fragment.
- end-to-end sequences are either:
    - a complete single read
    - a read pair with an internal alignment gap
- some platforms guarantee end-to-end reads, e.g., PacBio, others do not

### incomplete
An adjective describing a sequence that does not extend end-to-end on an insert.
- incomplete molecules are either:
    - a truncated single-read from a relevant platform, e.g., Ultima, ONT
    - an orphan read with one read missing from a pair

## Encoding Sequence Structures as Paths

Some of the following keywords are shared by sequence graph encoding as they are general terms derived from graph theory,
but in agFree they refer to the way that sequence paths are encoded as linear representations of single observed sequences.
The general difference between sequence graphs and agFree paths is that graph nodes represent base sequences whereas agFree
nodes represent numbered positions in a haploid genome reference.

### node (node position)
A specific numbered position on a genome strand; two nodes define an edge.
- node identifiers are RNAME:pos1:strand(+|- or 0|1)

### edge
A connection between two nodes, either an alignment, a read projection, a pair gap, or an SV junction.
- edges are numbered sequentially starting from 1 within each contig

### alignment
An edge that corresponds to a portion of a sequence/molecule/contig aligned contiguously to the genome.
- alignments are discovered using sequences, but carry forward without modification through molecules and contigs
- SV error correction entails dropping false junctions and ignoring low quality junctions between alignments
- alignments roughly correspond to a sequence of bases defined by the reference genome, but may have SNPs and indels carried in edge metadata

### projection
An edge that corresponds to the 3'-most bases of incompletely sequenced RE fragments that were inferred based on RE sites.
- both alignments and projections correspond to base runs on the reference genome, but only alignments were sequenced outright
- projections to predicted next RE sites may be found on variable-read platforms (ONT, Ultima) and for orphaned paired reads

### gap
An edge that corresponds to the connection between two unmerged reads of a pair.
- gaps lengths can be positive (true gap in a large insert) or negative (overlap too short for fastp to merge)
- SVs are never called within gaps, they are non-informative for SV discovery and error correction in agFree

### junction 
An edge that nominates a SV by connecting two distant alignments through the sequenced bases of a contig.
- each read thus has a series of edges as: alignment[-junction|gap-alignment...][-projection]
- junction edges carry metadata on the number and identity of inserted or microhomologous bases

### path
A set of nodes and edges that completely describes one contig (or a portion of one).

THIS SECTION STILL UNDER CONSTRUCTION
