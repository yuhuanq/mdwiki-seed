# RNA-seq pipeline

## Software overview

`FASTQ > Tophat > Cufflinks ||> Cuffdiff`

## bcl2fastq

If necesary, to convert bcl files, use `bcl2fastq`. i.e.,

```bash
$ bcl2fastq -R <path to runfolder> -o <output dir> --sample-sheet samplesheet.csv
```

## Tophat

Mapping short reads from spliced transcripts to whole genome.

```bash
tophat -p 8 \
-o tophat/sample1 \
-G $REF_ANNOT/genes.gtf \
genome.fa \
<path to R1.fastq> \
<path to R2.fastq>
```

`-p` opton specifies number of threads to use.

`-G` takes in GTF/GFF filename with known transcripts.

## Cuffdiff

`cuffdiff` calculates differential expression between two sets of RNA-seq data
(treatment vs control), using the BAM files created by tophat alignment.

```bash
cuffdiff \
-o <output-dir> \
-p 8 \
$REF_ANNOT/genes.gtf \
T1_r1_hits.bam, T1_r2_hits.bam \
C1_r1_hits.bam, C1_r1_hits.bam
```

**use qsub**

`<command> --help` to see all the options and `man <command>` if manual pages exist,
otherwise look at online documentation for a more detailed overview.

