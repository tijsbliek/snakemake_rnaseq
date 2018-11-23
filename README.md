# RNA_seq_Snakemake
A snakemake pipeline for the analysis of RNA-seq data that makes use of [hisat2](https://ccb.jhu.edu/software/hisat2/index.shtml) and [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html).

[![Snakemake](https://img.shields.io/badge/snakemake-≥5.2.0-brightgreen.svg)](https://snakemake.bitbucket.io)
[![Miniconda](https://img.shields.io/badge/miniconda-blue.svg)](https://conda.io/miniconda)

# Aim
To align, count, normalize couts and compute gene differential expressions between conditions using paired-end Illumina RNA-seq data.

# Description
This pipeline analyses the raw RNA-seq data and produce a file containing normalized counts, differential expression and functions of transcripts. The raw fastq files will be trimmed for adaptors and quality checked with trimmomatic. Next, the necessary genome fasta sequence and transcriptome references will be downloaded and the trimmed reads will be mapped against using hisat2. with stringtie and a refference annotation a new annotation will be created. This new annotation will be used to obtain the raw counts and do a local blast to a transcriptome fasta containing predicted functions. The counts are normalized and differential expressions are calculated using DESeq2. This data is combined with the predicted functions to get the final results table.


# Content
- `Snakefile`: a master file that contains the desired outputs and the rules to generate them from the input files.
- `config/`: a folder containing the configuration files making the Snakefile adaptable to any input files, genome and parameter for the rules.
- `data/`: a folder containing samples.txt (sample descriptions) and subsetted paired-end fastq files used to test locally the pipeline. Generated using [Seqtk](https://github.com/lh3/seqtk):
`seqtk sample -s100 {inputfile(can be gzipped)} 250000 > {output(always gunzipped)}`
- `envs/`: a folder containing the environments needed for the conda package manager. If run with the `--use-conda` command, Snakemake will install the necessary softwares and packages using the conda environment files. 


# Usage

## Configuration file
Make sure you have changed the parameters in the `configs/config.yaml` file that specifies where to find the sample data file, the genomic and transcriptomic reference fasta files to use and the parameters for certains rules etc.  
This file is used so the `Snakefile` does not need to be changed when locations or parameters need to be changed.

## Snakemake execution
The Snakemake pipeline/workflow management system reads a master file (often called `Snakefile`) to list the steps to be executed and defining their order. It has many rich features. Read more [here](https://snakemake.readthedocs.io/en/stable/).

## Dry run
Use the command `snakemake --use-conda -np` to perform a dry run that prints out the rules and commands.

## Real run
Simply type `Snakemake --use-conda` and provide the number of cores with `--cores 10` for ten cores for instance.  
For cluster execution, please refer to the [Snakemake reference](https://snakemake.readthedocs.io/en/stable/executable.html#cluster-execution).

# Main outputs
- the RNA-Seq read alignment files __*.bam__
- the fastqc report files __\*.html__
- the unscaled RNA-Seq read counts: __counts.txt__
- the differential expression file __results.tsv__
