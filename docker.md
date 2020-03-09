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

## Recipe file
Available in https://github.com/matmu/vep/

## Singularity Hub
Build from recipes on Github: https://singularityhub.github.io/singularityhub-docs/docs/getting-started/recipes


## Build Singularity image
* Binding paths (disabled on OMICS cluster, by default /scratch is bound): https://singularity.lbl.gov/docs-mount
* Singularity recipes: https://singularity.lbl.gov/docs-recipes

```bash
singularity build vep_99-GRCh38.simg shub://matmu/vep:99-grch38
```

or from Docker image (no apps available)
```bash
singularity build vep_99-GRCh38.simg docker://matmu/vep:99-GRCh38
```

## View help
```bash
singularity run-help vep.99-GRCh38.simg
```

For specific app
```bash
singularity run-help --app vep-tab vep.99-GRCh38.simg
```

##  Run singularity
```bash
singularity run --app vep-tab vep.99-GRCh38.simg
```
