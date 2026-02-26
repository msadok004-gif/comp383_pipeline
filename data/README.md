# COMP383 Python Pipeline Project (Snakemake)

## Overview

This repository automates COMP383 Pipeline Project **Steps 2–5** using a single Snakemake workflow (`Snakefile`).

Step 1 (SRA download) is performed manually as required by the assignment.

The pipeline:
- Downloads the HCMV reference genome
- Builds a Bowtie2 index
- Maps paired-end reads to HCMV
- Filters mapped read pairs
- Assembles reads using SPAdes (k=99)
- Computes assembly statistics
- Identifies the longest contig
- Builds a local Betaherpesvirinae BLAST database
- BLASTs the longest contig
- Generates a final report

Final output file:
```
PipelineReport.txt
```

---

## Repository Structure

```
.
├── Snakefile
├── config.yaml
├── environment.yml
├── scripts/
├── data/
├── results/
└── README.md
```

---

## Dependencies

This pipeline requires:

- Snakemake
- Bowtie2
- Samtools
- SPAdes
- BLAST+
- Biopython
- sra-tools
- curl

### Installation

Create the conda environment:

```bash
conda env create -f environment.yml
conda activate comp383_pipeline
```

---

## Step 1 – Download FASTQ Files (Manual)

The following SRA samples are required:

- SRR5660030
- SRR5660033
- SRR5660044
- SRR5660045

From inside the `data/` directory:

```bash
fasterq-dump SRR5660030 --split-files
gzip SRR5660030_1.fastq SRR5660030_2.fastq

fasterq-dump SRR5660033 --split-files
gzip SRR5660033_1.fastq SRR5660033_2.fastq

fasterq-dump SRR5660044 --split-files
gzip SRR5660044_1.fastq SRR5660044_2.fastq

fasterq-dump SRR5660045 --split-files
gzip SRR5660045_1.fastq SRR5660045_2.fastq
```

After this step, your `data/` folder should contain:

```
SRR5660030_1.fastq.gz
SRR5660030_2.fastq.gz
SRR5660033_1.fastq.gz
SRR5660033_2.fastq.gz
SRR5660044_1.fastq.gz
SRR5660044_2.fastq.gz
SRR5660045_1.fastq.gz
SRR5660045_2.fastq.gz
```

---

## Running the Pipeline (Steps 2–5)

From the repository root:

```bash
snakemake --cores 4
```

This will automatically:

1. Download the HCMV reference genome
2. Build Bowtie2 index
3. Map reads to HCMV
4. Filter mapped read pairs
5. Assemble reads with SPAdes (k=99)
6. Count contigs >1000 bp
7. Calculate total bp in contigs >1000 bp
8. Extract longest contig
9. Build local Betaherpesvirinae BLAST database
10. BLAST the longest contig
11. Generate `PipelineReport.txt`

---

## Sample Test Run (<2 Minutes)

To create smaller FASTQ files for quick testing:

```bash
mkdir -p data/sample_reads

zcat data/SRR5660030_1.fastq.gz | head -n 40000 | gzip > data/sample_reads/SRR5660030_1.fastq.gz
zcat data/SRR5660030_2.fastq.gz | head -n 40000 | gzip > data/sample_reads/SRR5660030_2.fastq.gz
```

You may temporarily replace the full FASTQ files with these smaller files for testing:

```bash
cp data/sample_reads/SRR5660030_1.fastq.gz data/
cp data/sample_reads/SRR5660030_2.fastq.gz data/
```

Then run:

```bash
snakemake --cores 4
```

---

## Output

The final output file:

```
PipelineReport.txt
```

This file contains:

- Read pair counts before and after Bowtie2 filtering
- Number of contigs >1000 bp
- Total bp in contigs >1000 bp
- Top 5 BLAST hits for each sample

---

## Notes

- All file paths are relative (no hardcoded absolute paths).
- The workflow can be reproduced by cloning the repository and following the instructions above.
- The pipeline is fully automated using a single Snakemake file.
