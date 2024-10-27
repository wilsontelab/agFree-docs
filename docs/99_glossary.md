---
title: Glossary of Terms
has_children: false
nav_order: 90
published: true
---

# agFree - Glossary of Terms and Identifiers

This page provides a glossary of hierarchical terms used by the **`agFree`** data analysis pipeline.
These keywords are used consistently throughout the code and comments, so it is essential that
you understand their precise meaning.

## Biological Sources

### species
The standard biological meaning, with corresponding base reference genomes, e.g., hg38 for human.
### genotype
A specific representative of a species, e.g., a human individual, clonal cell line, or a mouse strain.
- genotypes differ from a species base reference genome on one or both alleles
- ideally, genotypes are tracked with custom references with diploid phasing (see below)
### sample
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
- there are three types of references recognized by agFree: genomes, genotypes, and pangenomes
- if not otherwise clarified, reference by default refers to a single haploid genome reference
- all coordinates in agFree are reported relative to a single haploid genome reference, even if aligned to a genotype
### genome
A single, specific set of linear reference chromosome sequences that represent a species, e.g., hg38.
- chromosomes/chroms are identified by their RNAME (reference name, e.g., chr17)
### (genotype) assembly
The set of (sub)chromosome-level sequence contigs that represent a genotype of a species.
- in agFree, "assembly" is not typically used to refer to genomes, just genotypes
- an agFree genotype assembly comprises:
    - one genome reference haplotype, e.g., hs1
    - one or two genotype-specific haplotypes (usually two for a diploid genotype, for three total haplotypes)
### pangenome
A haploid genome combined with multiple additional haplotype sequences from multiple genotypes from a species.
### graph
The non-linear, branched representation of the haplotypes found within a genotype or pangenome.
- agFree uses graphs to create RE site maps that are projected back to the haploid reference genome
- it does not currently use graphs for read alignment, favoring iterative alignment to
haplotypes to support genotype masking of some reference-alignment variants

## Restriction Enzymes and Sites

### RE, or bRE
A single, specific blunt restriction enzyme used to generate library inserts from a sample, e.g., EcoRV.
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

## Encoding Sequence Structures

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
- paths are recorded in an ordered series of rows in a file of edges, such that:
    - each row describes one edge comprising two nodes
    - edges proceed from a contig 5' to 3' end as it was sequenced relative to read1
    - outermost contig nodes, usually corresponding to RE sites, are found once on the first and last path row
    - internal nodes in unmerged/SV contigs are found twice, once in an alignment and once in a gap/junction edge row

THIS SECTION STILL UNDER CONSTRUCTION

### segment       
a subset of a path considered to be a non-chimeric, high-quality, and confirmed true path
- segments are smaller than the contigs from which they arise, but are the trusted portion(s) of a contig
- unlike blocks, which are retained in a single segment row, segments are split from each other into separate rows
- a segment may contain SV junctions of any type if confirmed by multiple independent contigs
- segment identifiers are QNAME:contigN:segmentN 
### block
a subset of a path aligned to a single strand of a single chromosome over one or more alignments
- blocks may contain deletion, duplication and insertion junctions, but never gaps, inversions or translocations
- blocks are numbered sequentially starting from 1 within each contig
- inversion and translocation junctions receive a block number also, which is unique to that junction edge
- the utility of blocks is to support juction error correction via alignment bandwidth

# block         
a (subset of a) path whose outermost alignments predict traversal of a similar number of bases along:
#                       a single strand of a single chromosome of the reference genome
#                       a query contig as sequenced
#                   i.e., where the reference and contig base spans (ignoring internal junctions) differ by less than BANDWIDTH_TOLERANCE
#                   blocks are the result of junction error correction via alignment bandwidth
#                       junctions in blocks that fail bandwidth often arise from alignment errors at low quality bases
#                       mosaic SV junctions always occur between, never within, blocks
#                       multi-alignment blocks must have at least two junctions or indel operations, one that "reverses" the other
#                       as needed, multi-alignment blocks can be (conceptually) collapsed to a single edge of type BLOCK
#                   block-based error correction alone fails to detect alignment errors that extend to the end of a contig
#                   block identifiers are QNAME:contigN:blockN 

# segment       a (subset of a) path that contains only high-quality junctions as judged by flanking alignment specificity
#                   criteria for accepting a junction in a contig segment include MAPQ, alnQ and breakpoint realignment
#                   segment identifiers are QNAME:contigN:segmentN 

# clonal        adjective/flag identifying junctions found in >=MOSAIC_THRESHOLD contigs
#                   clonal junctions must pass segment criteria but may fail block bandwidth with sufficient contig counts
#                   junction matching is executed with fuzzy logic that allows up to JUNCTION_TOLERANCE node position distance
#                   clonal junctions may or may not be fully heterozygous or homozygous, i.e., may be subclonal
# mosaic        adjective/flag identifying junctions found in <MOSAIC_THRESHOLD contigs
#                   mosaic junctions must pass all segment criteria and block bandwidth
# singleton     adjective/flag identifying mosaic junctions found in exactly one contig
#-------------------------------------------------------------------------------------
# flanks        the two alignments on either side of a junction
#                   contigs are split into segments at junctions flanked by low quality alignments (among other reasons)
# bandwidth     a base span on a reference genome strand minus the corresponding base span on a contig
#                   no SVs are called inside a contig span with bandwidth < BANDWIDTH_TOLERANCE as they likely arise from misaligned low quality bases
#                   junctions that fail bandwidth do NOT split a contig into segments, those junctions are simply ignored
# canonical     the strand orientation agreed to represent a junction or path to allow comparison across contigs
#                   individual junction canonicity is determined from the nodes flanking the junction
#                   path canonicity is determined from the outermost alignment on the 5' end
#-------------------------------------------------------------------------------------
# in total, in decreasing order of hierarchy:
#   species have individuals with variable genotypes, from which samples are obtained
#   sequencing events describe sample gDNA library inserts in one or two basecalled reads
#   single reads, merged read pairs, and unmerged read pairs continue forward as independent sequences
#   sequences are deduplicated to yield unique, assembled library molecules
#       for ONT, duplex events yielding the same path on opposite strands are collapsed to single molecules on one strand
#   chimeric molecules are split at ligation artifacts inferred from RE site patterns to yield contigs
#       this chimera error correction is a primary objective of the agFree approach

#   contigs may be split to multiple segments at low-quality or unconfirmed junctions
#   a segment may contain one or more blocks separated by inversion or translocation junctions that may or may not call SVs
#   a block may contain one or more alignments separated by insertion, deletion, or duplication junctions that may or may not call SVs

#   an alignment may contain indel operations in its CIGAR string and cs tag that are never called as SVs
#=====================================================================================
