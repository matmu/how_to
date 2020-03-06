# Docker

## Manually build Docker image of VEP + Cache

```bash
docker pull ensemblorg/ensembl-vep

mkdir vep_cache
cd vep_cache
wget ftp://ftp.ensembl.org/pub/release-99/variation/indexed_vep_cache/homo_sapiens_merged_vep_99_GRCh38.tar.gz
tar zxf homo_sapiens_merged_vep_99_GRCh38.tar.gz
cd..

docker cp vep_data/ 0f44ccb48f3d:/opt/vep/.vep

docker run -it ensemblorg/ensembl-vep

docker commit 0f44ccb48f3d matmu/vep:99-GRCh38
```

## Build VEP + Cache using Dockerfile

More info: https://docs.docker.com/get-started/part2/

```bash
#Download base image ensemblorg/ensembl-vep
FROM ensemblorg/ensembl-vep:latest

# Use WORKDIR to specify that all subsequent actions should be taken from the directory 
# /usr/src/app in your image filesystem (never the host’s filesystem).
WORKDIR /opt/vep/

# Download cache data and install
RUN mkdir .vep; cd .vep; curl -O ftp://ftp.ensembl.org/pub/release-99/variation/vep/homo_sapiens_vep_99_GRCh38.tar.gz && tar xzf homo_sapiens_vep_99_GRCh38.tar.gz && rm homo_sapiens_vep_99_GRCh38.tar.gz
```

# Singularity
* requires root privileges to build from recipe -> use https://singularity-hub.org/ instead
* can't build from recipe on Mac

## Singularityhub
Build from recipes on Github: https://singularityhub.github.io/singularityhub-docs/docs/getting-started/recipes

## Manually convert/build Singularity image

Binding paths (disabled on OMICS cluster, by default /scratch is bound): https://singularity.lbl.gov/docs-mount
Singularity recipes: https://singularity.lbl.gov/docs-recipes

```bash
singularity build vep_99-GRCh38.simg docker://matmu/vep:99-GRCh38
```


## Convert/build with recipe file

```bash
Bootstrap: docker
From: matmu/vep:99-GRCh38

%help
VEP 99-GRCh38. Available VEP cache: merged (use --merged). See environment variables (env) for file locations.

%environment
    SINGULARITYENV_PREPEND_PATH=/opt/vep/src/ensembl-vep/
    CACHE=/opt/vep/.vep
    export REF SINGULARITYENV_PREPEND_PATH
    
%apprun default
    exec vep 
    
%apphelp default
    Args: input.vcf[.gz] outout.txt reference.fasta
    Command executed (For parameter explanations see http://uswest.ensembl.org/info/docs/tools/vep/script/vep_options.html) :
    vep --dir $CACHE
        --input_file INPUT_FILE 
        --output_file OUTPUT_FILE 
        --fasta REFERENCE_FILE 
        --species homo_sapiens 
        --assembly GRCh38 
        --tab 
        --merged
        --cache
        --offline
        --verbose
        --force_overwrite
        --sift b
        --polyphen b
        --ccds
        --uniprot
        --hgvs
        --symbol
        --numbers
        --domains
        --regulatory
        --canonical
        --protein
        --biotype
        --uniprot
        --tsl
        --appris
        --gene_phenotype
        --af
        --af_1kg
        --af_esp
        --af_gnomad
        --max_af
        --pubmed
        --variant_class 
        --total_length
        --check_existing
        --nearest symbol
        --total_length
```
