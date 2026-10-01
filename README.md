# RNA-seq Differential Expression Pipeline

A reproducible **bulk RNA-seq workflow built with Nextflow** to investigate transcriptional changes associated with **gefitinib treatment in PC-9 lung adenocarcinoma cells**.

This project takes publicly available sequencing data through the major stages of an RNA-seq analysis, from raw sequencing reads and quality control to alignment, gene-level quantification, differential expression, and biological interpretation.

## Project Status

**In progress**

### Completed

-  Selected and evaluated a suitable public RNA-seq dataset
-  Identified biological replicates and experimental conditions
-  Downloaded sequencing data from NCBI SRA
-  Converted SRA data to FASTQ
-  Organized raw data and project structure

### In progress / Planned

-  Raw-read quality control with FastQC
-  Quality and adapter trimming with fastp
-  Post-trimming quality assessment with MultiQC
-  Human reference genome preparation
-  Read alignment with STAR
-  Alignment processing and quality assessment
-  Gene-level quantification with featureCounts
-  Differential expression analysis with DESeq2
-  PCA and sample-level exploratory analysis
-  MA plot and volcano plot
-  Differential-expression heatmap
-  Biological interpretation of significant genes
-  Complete Nextflow workflow
-  Reproducibility and workflow documentation

---

## Biological Question

The project uses a controlled comparison between **untreated and gefitinib-treated PC-9 cells**.

The primary question is:

> **How does gene expression change in PC-9 cells following gefitinib treatment?**

The selected dataset contains three untreated samples and three gefitinib-treated samples, providing biological replicates for differential expression analysis.

```text
                  PC-9 cells
                      │
          ┌───────────┴───────────┐
          │                       │
     Untreated                 Gefitinib
      Day 0                     Day 3
       n = 3                      n = 3
          │                       │
          └───────────┬───────────┘
                      ↓
             RNA-seq comparison
                      ↓
          Differential expression
```

The goal is not simply to generate a list of differentially expressed genes, but to build a **reproducible computational workflow** that can take raw sequencing data through the complete RNA-seq analysis process.

---

## Dataset

The analysis uses a selected bulk RNA-seq subset from **GEO study GSE162045**, accessed through the NCBI Sequence Read Archive (SRA).

The selected samples are:

| Sample | Condition | SRA Run |
|---|---|---|
| GSM4932167 | PC-9, Day 0, Untreated | SRR13118806 |
| GSM4932168 | PC-9, Day 0, Untreated | SRR13118807 |
| GSM4932169 | PC-9, Day 0, Untreated | SRR13118808 |
| GSM4932170 | PC-9, Day 3, Gefitinib | SRR13118809 |
| GSM4932171 | PC-9, Day 3, Gefitinib | SRR13118810 |
| GSM4932172 | PC-9, Day 3, Gefitinib | SRR13118811 |

The selected runs are **single-end Illumina NextSeq 500 RNA-seq data with approximately 76 bp reads**.

The study contains additional sequencing data; this project uses the above six-run subset to create a manageable, well-defined treatment-versus-control analysis with biological replicates.

---

## Workflow

The planned workflow is:

```text
                 Raw sequencing reads
                         │
                         ↓
                       FASTQ
                         │
                         ↓
                      FastQC
                         │
                         ↓
                       fastp
                         │
                         ↓
                      MultiQC
                         │
                         ↓
                        STAR
                         │
                         ↓
                      SAMtools
                         │
                         ↓
                   featureCounts
                         │
                         ↓
                       DESeq2
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
            PCA      Volcano Plot   Heatmap
             │           │           │
             └───────────┴───────────┘
                         ↓
              Biological interpretation
```

The final implementation will use **Nextflow** to connect these stages into a reproducible workflow.

---

## Why This Dataset?

This dataset was selected because it provides:

- A clearly defined treatment and control comparison
- Three biological replicates per condition
- A manageable dataset size for workflow development
- Publicly accessible raw sequencing data
- Real RNA-seq data suitable for differential expression analysis

The dataset therefore serves as a real biological test case for developing and validating the pipeline.

---

## Data Acquisition

The sequencing runs were downloaded from the NCBI Sequence Read Archive using the SRA Toolkit.

The downloaded `.sra` files are stored locally under:

```text
data/sra/
```

The SRA files are **not uploaded to GitHub** because of their large size.

The accession list used for data acquisition is maintained in:

```text
data/sra_accessions.txt
```

---

## SRA → FASTQ Conversion

The downloaded SRA runs were converted to FASTQ using `fasterq-dump`.

For example:

```bash
fasterq-dump \
    data/sra/SRR13118806/SRR13118806.sra \
    --outdir data/fastq \
    --threads 4
```

The six sequencing runs were converted to FASTQ and stored locally under:

```text
data/fastq/
```

The resulting FASTQ files occupy approximately **36 GB** in total.

Raw FASTQ files are excluded from Git version control.

---

## Analysis Goals

The completed pipeline will be used to:

1. Assess raw sequencing quality.
2. Identify and remove adapter or low-quality sequence where necessary.
3. Evaluate sequencing quality after preprocessing.
4. Align reads to the human reference genome.
5. Generate gene-level read counts.
6. Identify genes showing significant expression differences between untreated and gefitinib-treated samples.
7. Explore sample relationships using PCA.
8. Visualize differential expression using MA and volcano plots.
9. Visualize expression patterns of selected genes using heatmaps.
10. Interpret the resulting transcriptional changes biologically.

---

## Tools

The workflow is being developed using:

- **Nextflow** — workflow orchestration
- **FastQC** — sequencing quality control
- **fastp** — read preprocessing
- **MultiQC** — QC report aggregation
- **STAR** — RNA-seq read alignment
- **SAMtools** — alignment processing
- **featureCounts** — gene-level quantification
- **R / DESeq2** — differential expression analysis
- **Git / GitHub** — version control and project documentation

---

## Project Objective

The primary objective is to demonstrate the development of a **reproducible NGS/RNA-seq analysis pipeline**, rather than simply performing a one-off differential expression analysis.

The project focuses on understanding and implementing the complete workflow:

```text
Public sequencing data
        ↓
Data preparation
        ↓
Quality control
        ↓
Preprocessing
        ↓
Alignment
        ↓
Quantification
        ↓
Statistical analysis
        ↓
Visualization
        ↓
Biological interpretation
```

The final Nextflow implementation will allow the individual analysis steps to be executed as a connected, reproducible workflow.
