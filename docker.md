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
Help me. I'm in the container.

%environment
    REF=/data/lied_egypt_genome/reference/hg38/Homo_sapiens_assembly38.fasta
    SINGULARITYENV_PREPEND_PATH=/opt/vep/src/ensembl-vep/
    export REF SINGULARITYENV_PREPEND_PATH
    
%apprun default
    exec vep 
    
%apphelp default
    This is the help for foo.
```
