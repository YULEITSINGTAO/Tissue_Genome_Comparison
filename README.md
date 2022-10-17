# Tissue Genome Comparison
---
Author:
  - Lei Yu, lyu062@ucr.edu, University of California, Riverside
  - Weiming Chen, labghost1@163.com, Guizhou University
---


  * [Introduction](#introduction)
  * [Method](#method)
    + [Workflow](#workflow)
    + [Step 1 Sequencing QC and trim](#step-1-sequencing-qc-and-trim)
    + [Step 2 MultiQC](#step-2-multiqc)
    + [Step 3 Alignment](#step-3-alignment)
    + [Step 4 GATK recalibrate and SNP&INDEL filter](#step-4-gatk-recalibrate-and-snp-indel-filter)
    + [Step 5 Tissue comparison](#step-5-tissue-comparison)
    + [Step 6 Visualization](#step-6-visualization)
  * [Bash Scripts](#bash-scripts)
  * [Snakemake](#snakemake)
  * [Example](#example)


## Introduction
Genetic variation between tumor cells and normal cells has been extensively investigated. However, genome differences between tissues within individuals are far less understood. It is commonly assumed that all normal tissue from the same zygote sharing the same content but gets difference from the epigenetics effect. In most of the inheritance and cancer research, use blood tissue replaces the germline cell. In this repository, we developed the snakemake pipeline to compare the genome between blood tissue and testis based on the next generation sequencing result. 

## Method
### Workflow
### Step 1 Sequencing QC and trim
[fastp](https://github.com/OpenGene/fastp) was applied to control the quality and trim adaptor of fastq files.

```
fastp -i in.R1.fq.gz -I in.R2.fq.gz -o out.R1.fq.gz -O out.R2.fq.gz
```
### Step 2 MultiQC
[multiqc](https://multiqc.info/docs/) can generate the merged reports from different 
### Step 3 Alignment
### Step 4 GATK recalibrate and SNP&INDEL filter
### Step 5 Tissue comparison
### Step 6 Visualization

## Bash Scripts

## Snakemake 

## Example