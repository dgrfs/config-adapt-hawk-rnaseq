module load singularity/3.8.5
mamba activate nf-core
# https://nf-co.re/configs/scw
# https://github.com/nf-core/configs/blob/master/docs/scw.md
# suggested institutional profile as '-profile scw'. can be used if standard config does not need to be changed. config file is optional.
nextflow run ~/scratch/nf-core-rnaseq_3.12.0/3_12_0/ -profile singularity -params-file params.yaml -c adapted.scw.config
