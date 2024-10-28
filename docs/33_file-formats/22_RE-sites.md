---
title: RE Sites
parent: File Formats
has_children: false
nav_order: 20
published: true
---

### RE site coordinate definition

At all stages, agFree pipelines identify the haploid reference coordinate position 
AFTER the cleaved phosphodiester bond as the RE site position, e.g., `5'-gat|Atc`,
according to the following diagram:

```
1234 56789
----|*----
```

When sites are inferred from read data, any clips on the read terminus are used to
adjust the site position to the location it would have been if the clip was aligned.
Trimmed ONT adapter bases are ignored during this process, i.e., site positions are
determined from trimmed but unclipped reads.

### Genome and genotype in silico RE digestion

The `af digest` pipeline actions scan reference sequences for _in silico_ RE sites.

For haploid reference genomes, a series of headerless files are created for all predefined
REs as `<--genome-dir>/RE_maps/<--genome>.digest.<--enzyme-name>.txt.gz`.
Each file has columns:
- **chrom** = the chromosome, i.e., RNAME (typically including the "chr" prefix)
- **sitePos1** = the 1-referenced site position on RNAME as defined above

For genotype graphs, a single folder
`<--genotype-dir>/<--genotype>_<--enzyme-name>`
contains output files for one enzyme.
File `<--genotype>.<--enzyme-name>.sites.gz` in that folder has columns:
- **chromIndex1** = the 1-referenced chromosome index according to the haploid genome reference fa.fai file
- **sitePos1** = the 1-referenced site position as defined above
- **haplotypes** = a sum of matching haplotype bits:
    - reference = 1, thus, an odd number for reference sites, even for haplotype-specific sites
    - haplotype1 = 2 
    - haplotype2 = 4, thus 7 when both haplotypes matched a reference site, etc.

### RE site localization and indexing

Beyond _in silico_ digestion, an input sample genotype subjected to ligFree
sequencing can be analyzed by the `af locate` pipeline actions to generate known + RFLP
site lists. This process results in a set of resuable files that index the
haploid reference genome for rapid matching of read ends to RE sites.

The files of located RE sites for a genotype are placed into a folder named `<--genotype>_<--enzyme-name>`.

Three files are used for site indexing (where 'fitering_sites' means the sites
will ultimately be used to filter against non-matching read sequences):
- *.filtering_sites.closest_site = binary-encoded file, one value per RNAME refPos1:
    - **siteIndex1** = signed, 1-referenced index of the RE site closest to refPos1 (positive values mean refPos1 >= closestSitePos1)
- *.filtering_sites.site_data = binary-encoded file, one value pair per RE filtering site, with fields:
    - **sitePos1** = as above
    - **haplotypes** = as above
    - six haplotype-specific fragment sizes
- *.filtering_sites.index.txt = ASCII table of RNAME byte offsets for the previous two lookup files, with columns:
    - **chrom** = RNAME, as above
    - **chromIndex1** = as above
    - **nSites** = number of RE sites on chrom
    - **chromSize** = size of chrom in bp
    - **closestSiteOffset** = byte offset to chrom in filtering_sites.closest_site file
    - **siteDataOffset** = byte offset to chrom in filtering_sites.site_data file

Additionally, a tabix-indexed, bgzipped file of filtering sites is created 
for rapid retrieval and plotting as
`*.filtering_sites.txt.bgz[.tbi]`, with columns:
- **chromIndex1** = as above
- **sitePos1** = as above
- **haplotypes** = as above
- **inSilico** = integer boolean whether the site was expected from _in silico_ digestion
- **nObserved** = number of sample reads that ended at the RE site
