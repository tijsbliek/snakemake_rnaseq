# Submitting jobs to an HPC environment

<!-- MarkdownTOC autolink="true" levels="1,2,3" -->

- [SURFsara LISA cluster](#surfsara-lisa-cluster)
	- [Creating the batch script](#creating-the-batch-script)
- [Useful links](#useful-links)

<!-- /MarkdownTOC -->


Snakemake is designed to interact smoothly with HPC job scheduling systems such as [SLURM](https://slurm.schedmd.com/overview.html).

As such this RNA-seq pipeline should be executed on a HPC environment when the number of samples becomes important or if you simply want to execute the pipeline more rapidly.

# SURFsara LISA cluster
This section describes how to execute a Snakemake pipeline on the [LISA cluster](https://userinfo.surfsara.nl/systems/lisa/description). 
The LISA system makes use of the SLURM job scheduler to run jobs. 

## Creating the batch script

An example script called `my_snakemake_job.sh` could look like this:  

```
#!/bin/bash          
#SBATCH -t 00:05:00 
#SBATCH -p short 
#SBATCH --mail-user=<a valid email address> 
#SBATCH --nodes 1
#SBATCH --tasks-per-node 1
#SBATCH --cpus-per-tasks 10

module load singularity

singularity run /home/<yourname>/<subfolder>/snakemake_rnaseq_4.7.12.sif --cores 10
```

Remarks:
- In this example script, we would use a maximum number of 1 x 1 x 10 = 10 CPUs.
- The Singularity image `snakemake_rnaseq_4.7.12.sif` file was obtained by pulling a docker image: `singularity pull docker://bleekerlab/snakemake_rnaseq:4.7.12`  that Singularity then converts to a single `.sif` file.
- :warning: make sure that you specify the `results` and `working_dir` directory options in the `config/config.yaml` file accordingly.  Use the $TMPDIR and the $HOME environment variables. 

# Useful links
- [Snakemake with SLURM](https://accio.github.io/programming/2020/06/16/Snakemake-with-slurm.html)
- [`sbatch` manual with its options](https://slurm.schedmd.com/sbatch.html)
