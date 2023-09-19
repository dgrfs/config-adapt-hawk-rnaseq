# RNASeq NF-Core pipeline setup

Config (and required resources) will depend on the data, the workflow.. so each config will likely need to be adapted to the specific project. This worked for me, for 12 rnaseq *fastq files. More resources may need to be given for larger-scale projects.

Base config file for SCW can be found:
https://nf-co.re/configs/scw
and
https://github.com/nf-core/configs/blob/master/docs/scw.md

The only change I made was to boost memory across all processes. The above scw markdown describes how to give more resources to specific processes e.g. 'highmem' partition for Salmon process

Suggested institutional profile as '-profile scw'. can be used if standard config does not need to be changed. config file is optional.

The files in the repository are kept in the primary directory with the *.fastq files in a subdirectory 'source_data'

No software needs to be downloaded. however, during debugging, the intial setup can be sped up by downloading some data beforehand:
git clone a static version of NF-Core RNASeq https://github.com/nf-core/rnaseq/releases
Set up a conda environment for launching nextflow https://nf-co.re/tools#installation (nextflow module available on Hawk but static version is preferred for debugging)

set up tmux session to run nextflow, which will supervise the jobs sent throughout the pipeline

```
module load tmux
tmux
```

set up environment
```
module load singularity/3.8.5
mamba activate nf-core
```

run nextflow with adapted config, static NF-Core tools, static nf-core/RNASeq
```
nextflow run ~/scratch/nf-core-rnaseq_3.12.0/3_12_0/ -profile singularity -params-file params.yaml -c adapted.scw.config
```
