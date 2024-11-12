# R5020 ChIP-seq Analysis

This repository contains scripts and workflows for analyzing ChIP-seq data of R5020 vs. vehicle-treated MCF7 and T47D cells. The primary goal of this analysis is to investigate the effects of progesterone receptor (PR) signaling and examine PR, ER, and p300 gene targets in treated versus untreated samples to determine if a specific gene of interest is regulated by progesterone signaling.

## Dataset Information

The ChIP-seq dataset used in this analysis is publicly available from GEO (GSE68355):
- **GEO Accession**: [GSE68355](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE68355)
- **Cell Lines**: MCF7 and T47D
- **Conditions**: R5020 (progesterone agonist) treated vs. vehicle

## Workflow Overview

The analysis is performed using nf-core workflows:
- **Data Fetching**: `nf-core/fetchngs` is used to download raw data from GEO.
- **ChIP-seq Analysis**: `nf-core/chipseq` is used to process, align, and analyze ChIP-seq data, with outputs including peak calling and differential binding analysis.

## Repository Structure

```plaintext
R5020_chipseq/
├── data/
│   ├── raw/               # Raw data files downloaded from GEO
│   └── design.csv         # Design file (optional) for sample conditions
├── results/
│   └── chipseq/           # Results from the ChIP-seq analysis
├── scripts/
│   ├── chipseq_config.yaml # Config file for nf-core/chipseq
│   └── fetchngs.sh         # Script for data fetching with nf-core/fetchngs
└── README.md               # Project documentation
