# smed-calbicans-infection-timecourse
The analysis pipeline for a bulk RNA-seq study of Schmidtea mediterranea infected with Candida albicans, sampled across a time course (uninfected control, 6 hours, and 1, 3, 5, and 7 days post-infection). The pipeline covers read alignment to the S. mediterranea reference genome, differential expression analysis, and Gene Ontology enrichment.
# Planarian *Candida albicans* Infection Time Course RNA-seq

RNA-seq analysis of *Schmidtea mediterranea* (planarian) response to *Candida albicans* infection across a time course, from bulk paired-end Illumina HiSeq sequencing. This repository contains the full analysis pipeline: read alignment, differential expression, and Gene Ontology enrichment.

## Experimental design

18 libraries, 3 biological replicates per condition:

| Condition | Sample IDs        | Description                  |
|-----------|-------------------|-------------------------------|
| Control   | CP1, CP2, CP3     | Uninfected                    |
| 6hpi      | S1, S2, S3        | 6 hours post-infection        |
| 1dpi      | S4, S5, S6        | 1 day post-infection          |
| 3dpi      | S7, S8, S9        | 3 days post-infection         |
| 5dpi      | S10, S11, S12     | 5 days post-infection         |
| 7dpi      | S13, S14, S15     | 7 days post-infection         |

Each infected time point is contrasted against the Control group throughout the analysis.

## Pipeline overview

Scripts are R Markdown files, meant to be run in this order:

1. **`Calbicans_Infection_Alignments.Rmd`**
   Aligns raw paired-end fastq.gz reads to the *S. mediterranea* reference genome using Rsubread (`buildindex`/`align`), generates a raw count matrix with `featureCounts`, and produces QC bar charts of alignment/mapping statistics per sample.

2. **`Calbicans_DEanalysis.Rmd`**
   Filters and normalizes the raw counts (`edgeR`/`limma-voom`), fits a linear model with one contrast per infected time point vs. Control, and reports differentially expressed genes (topTables, CPM z-scores, per-time-point and shared-across-time-point heatmaps). Also includes a `decideTests()`/`vennDiagram()` summary of up/down-regulated gene counts and their overlap across time points.

3. **`Calbicans_GOanalysis.Rmd`**
   Maps the differential expression results to Gene Ontology terms using `clusterProfiler`, running both over-representation analysis (ORA) and rank-based gene set enrichment analysis (GSEA/`cameraPR`) for each time point. Includes a `goseq`-based length-bias sensitivity analysis to check whether enrichment results are being driven by transcript length rather than real biological signal.

## Reference genome and annotation

Reads are aligned to the *S. mediterranea* genome assembly published by the Rink lab:

> Ivanković, M., Brand, J.N., Pandolfini, L. et al. A comparative analysis of planarian genomes reveals regulatory conservation in the face of rapid structural divergence. *Nat Commun* 15, 8215 (2024). https://doi.org/10.1038/s41467-024-52380-9

Whole-genome sequencing data: NCBI BioProject [PRJNA1052007](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1052007), SRA accession SRR27325393.

Gene ID-to-GO term mappings and the GTF annotation used for `featureCounts`/`goseq` are shared across related projects in this lab and are not duplicated in this repository (see script comments for expected file names/paths).

## Requirements

R (>= 4.4) with the following packages:

```
Rsubread, edgeR, limma, tidyverse, ComplexHeatmap, circlize,
clusterProfiler, GO.db, enrichplot, AnnotationDbi,
goseq, rtracklayer, GenomicRanges, data.table, stringr
```

Package versions used for the final analysis are recorded at the end of `Calbicans_GOanalysis.Rmd` (`sessionInfo()` output, also exported as a CSV).

## Data availability

Raw sequencing reads are being deposited in NCBI SRA. BioProject/SRA accession numbers will be added here once the submission is complete.

## Citation

Citation details will be added upon publication.
