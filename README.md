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
Get reference files (OPTIONAL, but improves reporducibility and saves time)
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
