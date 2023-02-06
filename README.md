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

```{figure} ./figures/workflow.jpg
---
height: 150px
name: Workflow
---
Workflow!
```


## Method
### Workflow and bash script
In order to illustrate the workflows more clear, we show the bash script first followed with the instruction of snake workflow.  


```
## Add a figure here
```
### Step 1 Sequencing QC and trim
[fastp](https://github.com/OpenGene/fastp) was applied to control the quality and trim adaptor of fastq files.

```
fastp -i in.R1.fq.gz -I in.R2.fq.gz -o out.R1.fq.gz -O out.R2.fq.gz
```
### Step 2 MultiQC
[multiqc](https://multiqc.info/docs/) can generate the merged reports from each samples in the cohort.
```
fastp -i in.R1.fq.gz -I in.R2.fq.gz -o out.R1.fq.gz -O out.R2.fq.gz
```

### Step 3 Alignment
echo "Convert Sample $id sam file to bam file"
$samtools view -bS $output_folder$id.sam > $output_folder$id.bam && echo "Convert Sample $id sam file to bam file finished"
```

## Step 3 Sort the bam files

echo "Sort sample $id bam file"
$samtools sort -@ 20 $output_folder$id.bam -o $output_folder$id.sorted.bam && echo "Sort sample $id bam file finished" && rm $output_folder$id.bam

```
### Step 4 GATK recalibrate and SNP&INDEL filter
Followed by the 
```
## Step 4 Markduplications

echo "Sample $id mark duplications"
java -jar $picard MarkDuplicates \
      I=$output_folder$id.sorted.bam \
      O=$output_folder$id.marked.sorted.bam \
      M=$output_folder$id.marked_dup_metrics.txt && echo "Sample $id mark duplications finished" &&
      rm $output_folder$id.sorted.bam


## Step 5 Create the index for the bam file

echo "Create sample $id bam index"
$samtools index $output_folder$id.marked.sorted.bam && echo "Create sample $id bam index finished"

## Step 6 Recalibrate Base quality scores
## Step 6.1 Create recal data table

echo "Create sample $id recal"
$gatk BaseRecalibrator \
   -I $output_folder$id.marked.sorted.bam \
   -R $reference \
   --known-sites $dbsnp138 \
   --known-sites $knownindel \
   --known-sites $gold_standard_ind \
   -O $output_folder$id.recal_data.table && echo "Create sample $id recal finished"

echo "Apply BQSR on sample $id"

## Step 6.2 Apply BQSR

$gatk ApplyBQSR \
   -R $reference \
   -I $output_folder$id.marked.sorted.bam \
   --bqsr-recal-file $output_folder$id.recal_data.table \
   -O $output_folder$id.marked.sorted.bqsr.bam && echo "Apply BQSR on sample $id finished"

## Step 6.3 Check the marked.sorted.bqsr.bam can be used for SNP/Indel call

java -jar $picard ValidateSamFile \
      I=$output_folder$id.marked.sorted.bqsr.bam \
      MODE=SUMMARY

echo "Check $id.marked.sorted.bqsr.bam finished"
## If shows no errors found, then can do next steps
```
### Step 5 Tissue comparison

### Step 6 Visualization

## Snakemake 

## Example