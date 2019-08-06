# TCGA expression data

This repository summarises [TCGA](https://tcga-data.nci.nih.gov/) expression data from various cancer types to be collated and the steps for their harmonization.

## Table of contents

<!-- vim-markdown-toc GFM -->
* [Pan-Cancer normalised data](#pan-cancer-normalised-data)
* [GDC counts data](#gdc-counts-data)
	* [TCGA projects](#tcga-projects)
	* [Data download](#data-download)
	* [Output data directory structure](#output-data-directory-structure)
	* [Data clean-up](#data-clean-up)
* [Pan-Cancer clinical data](#pan-cancer-clinical-data)

<!-- vim-markdown-toc -->
<br>


## Pan-Cancer normalised data

Normalised expression data across **33 cancer types** integrated by **[TCGA Pan-Cancer Atlas](https://gdc.cancer.gov/about-data/publications/pancanatlas)** (PanCanAtlas, see paper [The Cancer Genome Atlas Pan-Cancer analysis project](https://www.ncbi.nlm.nih.gov/pubmed/24071849)) initiative is available on the [Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC) and accomapnying [*Cell* paper](https://gdc.cancer.gov/about-data/publications/PanCan-CellOfOrigin) webpages.

**Data**

The RNA batch corrected matrix file is **[EBPlusPlusAdjustPANCAN_IlluminaHiSeq_RNASeqV2.geneExp.tsv](http://api.gdc.cancer.gov/data/3586c0da-64d0-4b74-a449-5ff4d9136611)**. 

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)


## GDC counts data

Follow the steps below to download published [TCGA](https://portal.gdc.cancer.gov/) expression files and prepare the data from individual datasets to read count matrices ready for downstream analyses.


### TCGA projects

There are expression data from **33 cancer types** (<*projects*>) publically available in [TCGA](https://portal.gdc.cancer.gov/). The individual [TCGA](https://portal.gdc.cancer.gov/) projects are summarised in the [projects summary table](./TCGA_projects_summary.md).


### Data download

Download and prepare the [TCGA](https://portal.gdc.cancer.gov/) expression read **count data** for each of the **33 cancer type** (<*project*>, see [TCGA projects summary table](./TCGA_projects_summary.md)) using the [TCGAbiolinks_transcriptome_profiling_data.R](https://github.com/umccr/TCGA-data-prep/blob/master/TCGAbiolinks_transcriptome_profiling_data.R) script described [here](https://github.com/umccr/TCGA-data-prep/blob/master/TCGAbiolinks_transcriptome_profiling_data.md). The [TCGAbiolinks](http://bioconductor.org/packages/release/bioc/vignettes/TCGAbiolinks/inst/doc/index.html) package is used to download the most recent data files by accessing the [National Cancer Institute](https://www.cancer.gov/) (NCI) [GDC](https://gdc.cancer.gov/about-data/publications/pancanatlas) thorough its [GDC Application Programming Interface](https://gdc.cancer.gov/developers/gdc-application-programming-interface-api) (API).

```
Rscript $scripts/TCGAbiolinks_transcriptome_profiling_data.R --out_dir <project> --project_id TCGA-<project> --tissue 1 --workflow Counts
```

NOTE, in case of the Acute Myeloid Leukaemia (LAML) the `--tissue` argument was set to `3` (Primary Blood Derived Cancer - Peripheral Blood).


### Output data directory structure

The output data for each cancer type (<*project*>) follows the directory structure below:

```
|
|____<project>
  |
  |____transcriptome_profiling
    |
    |____Counts
      |____Counts.exp
      |____Counts_boxplot.pdf
      |____Counts_clinical_info.txt
      |____Counts_samples.txt
      |____gdc-client
      |____gdc-client_v1.1.0_OSX_x64.zip
      |____gdc_manifest.txt
      |____R_parameters.txt
      |____GDCdata
        |
        |____<project>
          |
          |____harmonized
            |
            |____Transcriptome_Profiling
            | |
            | |____Gene_Expression_Quantification
            |   |____…
            |   |____…
            |
            |____Clinical
              |
              |____Clinical_Supplement
                |____…
                |____…
```
<br />

The two key output files used for downstream analyses are:

File | Description
------------ | ------------
Counts.exp | Read count data matrix
Counts_samples.txt | Combined samples annotation and associated clinical information
<br />


The description of **[TCGA barcodes](https://docs.gdc.cancer.gov/Encyclopedia/pages/TCGA_Barcode/)** that are used to represent the metadata of the TCGA participants and their samples is [here]([TCGA barcodes](https://docs.gdc.cancer.gov/Encyclopedia/pages/TCGA_Barcode/)).

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)

### Data clean-up

The expression read count data from each of the 33 cancer types was then cleaned based on the quality metrics provided in the *Merged Sample Quality Annotations* file **[merged_sample_quality_annotations.tsv](http://api.gdc.cancer.gov/data/1a7d7be8-675d-4e60-a105-19d4121bdebf)** from [TCGA PanCanAtlas initiative webpage](https://gdc.cancer.gov/about-data/publications/pancanatlas).

The **exclusion criteria** (rather rigorous to minimise data variation due to unwanted factors) are listed in the table below. The samples were excluded if at least one of the criterium is meet.

Column | Inclusion | Exclusion
------------ | ------------ | ------------
`patient_annotation`\* | No comments | Issue (*Observation* or *Notification*) reported
`sample_annotation` | No comments | Issue (* Notification*) reported
`aliquot_annotation` | No comments | Issue (*CenterNotification*) reported
`AWG_excluded_because_of_pathology`\* (see also `AWG_pathology_exclusion_reason`\**) | 0 | 1
`Do_not_use` | 0 | 1

\* Except for LAML project where *Notification* is due to "Alternate sample pipeline: Biospecimens (...) were processed into analyte outside of the normal TCGA standardized laboratory pipeline" <br />
\** AWG - [Analysis Working Group](https://sagebionetworks.org/research-projects/the-cancer-genome-atlas-pancancer-analysis-working-group/)

<br />

In total, 1001 samples were excluded and **9438 samples remained** for downstream analyses (see the [TCGA projects summary table](./TCGA_projects_summary.md) for per-project summary).

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)


## Pan-Cancer clinical data

The Pan-Cancer clinical data was integrated by **[TCGA Pan-Cancer Clinical Data Resource](https://gdc.cancer.gov/about-data/publications/PanCan-Clinical-2018)** and is described in a paper [An Integrated TCGA Pan-Cancer Clinical Data Resource to Drive High-Quality Survival Outcome Analytics](https://www.ncbi.nlm.nih.gov/pubmed/29625055). 

[TCGA Pan-Cancer Clinical Data Resource](https://gdc.cancer.gov/about-data/publications/PanCan-Clinical-2018) provides resource of the clinical annotations for [TCGA](https://portal.gdc.cancer.gov/) data and provides recommendations for use of clinical endpoints.

**Data**

It is strongly recommended that the published **[TCGA-CDR-SupplementalTableS1.xlsx
](https://api.gdc.cancer.gov/data/1b5f413e-a8d1-4d10-92eb-7c4ae739ed81)** file is used for clinical elements and survival outcome data to drive high quality analyses.

**Data source**

[Genomic Data Commons](https://gdc.cancer.gov/about-data/publications/pancanatlas) (GDC)