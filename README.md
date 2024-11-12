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

### Steps to Setup the Analysis
1. **Set up the configuration file**

   Create a configuration file named `icer.config` with the following content:

   ```groovy
   process {
       executor = 'slurm'
   }
   ```

2. **Run Fetchngs**
   First, create an `ids.csv` file that contains the list of GEO accession IDs to be fetched. The file should have the following format:

   ```csv
   GSE68355
   ```

   This file will be used as input for `nf-core/fetchngs`.

   Create a SLURM script to submit the job:

   ```bash
   #!/bin/sh
   #SBATCH --job-name=fetchngs
   #SBATCH --time=4:00:00
   #SBATCH --cpus-per-task=4
   #SBATCH --mem=16G
   #SBATCH --output=fetchngs-%j.out

   # Load Nextflow
   module load Nextflow

   # Run nf-core/fetchngs to fetch the data from GEO using an ids.csv file
   nextflow pull nf-core/fetchngs
   nextflow run nf-core/fetchngs -profile singularity \
     --input ./ids.csv \
     --outdir ./raw_data \
     --download_method sratools \
     --nf_core_pipeline rnaseq \
     -c icer.config
   ```
   Save the script as `fetchngs.sh`. Note: This is not rnaseq data, but there is no chipseq option for samplesheet creation for this pipeline.
   
   Submit the job to SLURM:

   ```bash
   sbatch fetchngs.sh
   ```

3. **Run ChIP-seq Analysis**

   After fetching the raw data, you can use the `nf-core/chipseq` pipeline to analyze the data.

   Create a SLURM script to submit the job:

   ```bash
   #!/bin/sh
   #SBATCH --job-name=chipseq_analysis
   #SBATCH --time=24:00:00
   #SBATCH --cpus-per-task=8
   #SBATCH --mem=32G
   #SBATCH --output=chipseq-%j.out

   # Load Nextflow
   module load Nextflow

   # Set the relative paths to the genome files
   GENOME_DIR="/mnt/research/common-data/Bio/genomes/Ensembl_GRCh38"
   GTF_FILE="$GENOME_DIR/Homo_sapiens.GRCh38.112.gtf.gz"
   FASTA_FILE="$GENOME_DIR/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz"

   # Run the ChIP-seq analysis
   nextflow pull nf-core/chipseq
   nextflow run nf-core/chipseq -profile singularity \
     --input ./raw_data/samplesheet/samplesheet.csv \
     --outdir ./results \
     --fasta $FASTA_FILE \
     --gtf $GTF_FILE \
     -c icer.config
   ```

   Save the script as `chipseq_analysis.sh`.
   
   Submit the job to SLURM:

   ```bash
   sbatch chipseq_analysis.sh
   ```

## Repository Structure

```plaintext
R5020_chipseq/
├── data/
│   ├── raw/               # Raw data files downloaded from GEO
│   └── samplesheet.csv    # Design file for sample conditions
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

