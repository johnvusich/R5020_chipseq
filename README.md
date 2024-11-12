# R5020 ChIP-seq Analysis

## Overview
This repository contains scripts and workflows for analyzing ChIP-seq data of R5020 vs. vehicle-treated MCF7 and T47D cells. The primary goal of this analysis is to identify progesterone receptor (PR), estrogen receptor (ER), and p300 gene targets in treated versus untreated conditions to investigate potential regulation of the gene of interest by progesterone signaling.

## Dataset Information
The ChIP-seq dataset used in this analysis is publicly available from GEO (GSE68355):
- **GEO Accession**: [GSE68355](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE68355)
- **Cell Lines**: MCF7 and T47D
- **Conditions**: R5020 (progesterone agonist) treated vs. vehicle

## Workflow Overview

The analysis is performed using nf-core workflows:
- **Data Fetching**: `nf-core/fetchngs` is used to download raw data from GEO.
- **ChIP-seq Analysis**: `nf-core/chipseq` is used to process, align, and analyze ChIP-seq data, with outputs including peak calling and differential binding analysis.

### Steps to Fetch Data:
1. Run the following command to fetch the data:
   ```bash
   nf-core fetchngs -g GSE68355 -o data/raw
   ```
   This command will download the raw data files into the `data/raw` directory.

## Analysis Workflow
The ChIP-seq data is analyzed using `nf-core/chipseq`. The workflow follows standard practices for ChIP-seq analysis, including quality control, alignment, peak calling, and differential analysis.

### Analysis Steps:
1. **Set Up Configuration File**: The configuration file for running `nf-core/chipseq` is located in `scripts/chipseq_config.yaml`.
   - Make sure the input, output directories, and reference genome are specified.

2. **Run the ChIP-seq Analysis**:
   Execute the following command to run the workflow:
   ```bash
   nextflow run nf-core/chipseq -c scripts/chipseq_config.yaml
   ```

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
```

## Tools and Dependencies
This analysis uses the following tools:
- **nf-core/fetchngs**: To fetch raw data from GEO.
- **nf-core/chipseq**: To run the ChIP-seq analysis pipeline.
- **Nextflow**: Workflow management tool.
- **Singularity**: For containerization to ensure reproducibility of analysis.

## Objectives
- Identify PR, ER, and p300 target genes in R5020-treated vs. vehicle-treated samples.
- Assess if the gene of interest is regulated by progesterone signaling.

## Contact
For questions or suggestions regarding this repository, please contact John Vusich at [GitHub](https://github.com/johnvusich).

