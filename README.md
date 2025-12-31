# Chuanxiao-Xie-s-Lab-DropCode-
DorpCode(GEAtools) is a bioinformatics software designed for high-throughput detection of mutation target sites in gene-edited materials. It supports multi-task analysis, barcode matching, Q30 data filtering, sample demultiplexing, sequence alignment, BAM file generation, and comprehensive analysis reporting.

🧬 Features

Barcode Matching & Demultiplexing: Automatically assigns samples based on barcode files.

Q30 Quality Filtering: Filters low-quality reads using Fastp.

Sequence Alignment: Aligns reads to a reference genome using BWA.

BAM File Generation: Outputs sorted BAM files and index files for visualization.

Multi-Sample Support: Processes multiple samples in batch mode.

Cross-Platform: Runs on Windows 10/11 with WSL2 (Ubuntu 22.04 LTS).

🛠️ Installation

Prerequisites
Windows 10/11 with WSL2 enabled

Ubuntu 22.04 LTS (via WSL)

Python 3, Bash

Steps：
1、Enable WSL2 and install Ubuntu from the Microsoft Store.

2、Download the installation package to the D: drive of your computer, locate the setup file in the GEAtools folder, and double-click and run it.

3、A desktop shortcut named GEAtool will be created.

🚀 Usage

1、Place your input files in D:\GEAtool\raw_data\Sample_X:

   reference.fasta

   target.fasta

   *_1.fq.gz, *_2.fq.gz

   barcode.xlsx

2、Double-click the GEAtool desktop shortcut to start analysis.

3、View results in D:\GEAtool\raw_data\BAM\Sample_X:

   *.sorted.bam

   *.bai (index)

   *_fastp_reporter.html (QC report)

4、Visualize BAM files using IGV.

📁 Project Structure

Key scripts include:

barcode1.py, barcode2.py – Barcode processing

fastp_q30.sh – Q30 filtering

split4_timer.py – Sample demultiplexing

EG_BAM.sh – BWA alignment & BAM generation

GEAtool.sh – Main workflow controller
