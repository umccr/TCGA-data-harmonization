# TCGA expression data

This repository summarises [TCGA](https://tcga-data.nci.nih.gov/) expression data from various cancer types to be collated and the steps for their harmonization.

## Table of contents

<!-- vim-markdown-toc GFM -->
* [Pan-Cancer normalised data](#pan-cancer-normalised-data)
* [GDC counts data](#gdc-counts-data)
	* [Data download](#data-download)
* [Pan-Cancer clinical data](#pan-cancer-clinical-data)

<!-- vim-markdown-toc -->
<br>


## Pan-Cancer normalised data

Normalised expression data across 33 cancer types integrated by **[TCGA Pan-Cancer Atlas](https://gdc.cancer.gov/about-data/publications/pancanatlas)** (PanCanAtlas, see paper [The Cancer Genome Atlas Pan-Cancer analysis project](https://www.ncbi.nlm.nih.gov/pubmed/24071849)) initiative is available on the [Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC) and accomapnying [*Cell* paper](https://gdc.cancer.gov/about-data/publications/PanCan-CellOfOrigin) webpages. The RNA batch corrected matrix file is **[EBPlusPlusAdjustPANCAN_IlluminaHiSeq_RNASeqV2.geneExp.tsv](http://api.gdc.cancer.gov/data/3586c0da-64d0-4b74-a449-5ff4d9136611)**. 

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)


## GDC counts data

Below are steps conducted to prepare published expression files and report individual datasets in form of read count data matrices ready for downstream analyses.


### Data download

Download and prepare the TCGA expression read **count data** for each of the **33 cancer types** (projects) using the [TCGAbiolinks_transcriptome_profiling_data.R](https://github.com/umccr/TCGA-data-prep/blob/master/TCGAbiolinks_transcriptome_profiling_data.R) script described [here](https://github.com/umccr/TCGA-data-prep/blob/master/TCGAbiolinks_transcriptome_profiling_data.md): 

```
Rscript $scripts/TCGAbiolinks_transcriptome_profiling_data.R --out_dir <project> --project_id TCGA-<project> --tissue 1 --workflow Counts
```

NOTE, in case of the Acute Myeloid Leukaemia (LAML) the `--tissue` argument was set to `3` (Primary Blood Derived Cancer - Peripheral Blood).

Here is the description of [TCGA barcodes](https://docs.gdc.cancer.gov/Encyclopedia/pages/TCGA_Barcode/) that are used to represent the metadata of the TCGA participants and their samples.

<br>
Work in progress...
<br>


### Data clean-up

The expression read count data from each of the 33 cancer types was then cleaned based on the quality metrics provided in the [Merged Sample Quality Annotations](http://api.gdc.cancer.gov/data/1a7d7be8-675d-4e60-a105-19d4121bdebf) file **[merged_sample_quality_annotations.tsv](http://api.gdc.cancer.gov/data/1a7d7be8-675d-4e60-a105-19d4121bdebf)**.

<br>
Work in progress...
<br>

## Pan-Cancer clinical data

The integrated clinical data from the **[TCGA Pan-Cancer Clinical Data Resource](https://gdc.cancer.gov/about-data/publications/PanCan-Clinical-2018)** is described in a paper [An Integrated TCGA Pan-Cancer Clinical Data Resource to Drive High-Quality Survival Outcome Analytics](https://www.ncbi.nlm.nih.gov/pubmed/29625055). 

[TCGA Pan-Cancer Clinical Data Resource](https://gdc.cancer.gov/about-data/publications/PanCan-Clinical-2018) provides resource of the clinical annotations for [TCGA](https://portal.gdc.cancer.gov/) data and provides recommendations for use of clinical endpoints.

It is strongly recommended that the available **[TCGA-CDR-SupplementalTableS1.xlsx
](https://api.gdc.cancer.gov/data/1b5f413e-a8d1-4d10-92eb-7c4ae739ed81)** file is used for clinical elements and survival outcome data to drive high quality analyses.

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)