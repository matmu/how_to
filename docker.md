# Docker

## Create Docker image of VEP + Cache

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


## Convert/build Singularity image

