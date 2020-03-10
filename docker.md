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
Dockerfile: https://github.com/matmu/vep
```bash
docker build - < Dockerfile
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
