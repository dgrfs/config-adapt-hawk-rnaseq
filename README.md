# RNASeq NF-Core pipeline setup

example data taken from:
https://github.com/hartwigmedical/testdata

NF-CORE RNASeq processing pipeline v3.12.0
https://nf-co.re/rnaseq/3.12.0

Dependencies (available modules on HPC)
 - Nextflow Tools
 - Singularity
 - TMUX

## Getting started
```
git clone https://github.com/dgrfs/config-adapt-hawk-rnaseq
```

## Download genome reference files
Get reference files (OPTIONAL, but improves reproducibility and saves time)
This may take some time depending on server demand
```
cd ref
bash get-refs.sh

# back to working directory when download is complete
cd ..
```

Base config file for SCW can be found: https://nf-co.re/configs/scw <br />
Note options on the scw config README for moving particular processes to 'highmem' partition (required if using HISAT2)

## Set up environment
```
# set up screen/tmux to run in background
module load tmux
tmux
```

```
# load dependencies
module load singularity-ce/3.11.4
module load nextflow/22.10.6
```

## Run Nextflow pipeline 
```
nextflow run nf-core/rnaseq \
    -r 3.12.0 \
    -params-file params.yaml \
    -profile singularity \
    -c adapted.scw.config \
    --fasta $PWD/ref/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz \
    --gtf $PWD/ref/Homo_sapiens.GRCh38.108.gtf.gz 
```

## OPTIONAL: Faster startup with saved references
Nextflow builds references. If you are running the pipeline multiple times, to speed up the process you can set save_reference to TRUE in params.yaml, and then refer to those saved references in future runs. <br />
After references have been made in the first run, add the following to params.yaml (change PATH to your references where appropriate):
```
fasta: $PWD/ref/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz
gtf: $PWD/ref/Homo_sapiens.GRCh38.108.gtf.gz
hisat2_index: $PWD/ref/genome/index/hisat2
gen_bed: $PWD/ref/genome/Homo_sapiens.GRCh38.108.bed
transcript_fasta: $PWD/ref/genome/genome.transcripts.fa
splicesites: $PWD/genome/index/Homo_sapiens.GRCh38.108.splice_sites.txt
rsem_index: $PWD/ref/genome/rsem
salmon_index: $PWD/ref/genome/index/salmon
```
## FAQ:
### I have multiple SLURM accounts. How do I specify which SLURM account to charge to?
Nextflow uses your default SLURM account to charge jobs to. 
To find out SLURM accounts you are a member of, run the following command:
```
sacctmgr show user withassoc \
        format=account,user,defaultaccount where user=$USER
```

On the adapted.scw.config file, uncomment the clusterOptions command, and change [MY_ACCOUNT_NAME] to the account you would like to charge to.


## ABOUT:
SANDBOX | NF-Core RNAseq | SCW Hawk --> Example data and worksheets for immediate test run of the nextflow nf-core RNAseq pipeline. To be used for debugging, testing.
