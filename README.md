# COMP383 Python Pipeline Project (Snakemake)

## Overview

This repository automates COMP383 Pipeline Project Steps 2–5 using a single Snakemake workflow (Snakefile).

Step 1 (SRA download) is performed manually.

Final output:
PipelineReport.txt

---

## Dependencies

- Snakemake
- Bowtie2
- Samtools
- SPAdes
- BLAST+
- Biopython
- sra-tools
- curl

Create environment:

conda env create -f environment.yml
conda activate comp383_pipeline

---

## Step 1 – Download FASTQ Files

Inside data/:

fasterq-dump SRR5660030 --split-files
gzip SRR5660030_1.fastq SRR5660030_2.fastq

fasterq-dump SRR5660033 --split-files
gzip SRR5660033_1.fastq SRR5660033_2.fastq

fasterq-dump SRR5660044 --split-files
gzip SRR5660044_1.fastq SRR5660044_2.fastq

fasterq-dump SRR5660045 --split-files
gzip SRR5660045_1.fastq SRR5660045_2.fastq

---

## Run the Pipeline

From repo root:

snakemake --cores 4

---

## Output

PipelineReport.txt contains:
- Read counts before and after filtering
- Assembly statistics
- Top BLAST hits

