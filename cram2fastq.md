# CRAM 2 FASTQ conversion

Since the format only the difference between the reference and the aligned read, the reference genome is needed for the conversion to fastq. Usually, the information about the reference genome is requested from an **EBA** webservice during conversion. However, the webservice doesn't always respond and therefore produces empty fastq files. Hence, it is recommended to create a local reference database with **SAMtools** prio to conversion.

## Workflow
1. Extract name of reference genome from cram header and download **.fasta** file
2. Create reference database with ```/path/to/samtools/misc/seq_cache_populate.pl```
3. Set appropriate environment variables
4. Use **SAMtools sort** and **fastq** to create valid fastq files

## Example bash script

```#! /bin/bash

path_tmp=...

seq_cache_populate.pl -root $path_tmp/cache /path/to/ref_fasta

export XDG_CACHE_HOME=$path_tmp
export REF_PATH=$path_tmp/cache/%2s/%2s/%s:http://www.ebi.ac.uk/ena/cram/md5/%s
export REF_CACHE=$path_tmp/cache/%2s/%2s/%s


# For paired end reads
samtools sort -n /path/to/cram | samtools fastq -1 output.R1.fq -2 output.R2.fq -
```
