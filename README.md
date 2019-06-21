# ifpan-marpiech-wgs
Comparison of two wgs methods: illumina vs 10x genomics

## data 
one person's genome (stored in projects/ifpan-marpiech-genome)

# illumina:
1. convert 10x bcl to fastq wih bcl2fastq2 with [this code](https://gist.github.com/gosborcz/b31df08f6bb8b83c51f7a310f8f2bcc1)

2. fastqc with [this docker image](https://hub.docker.com/r/pegi3s/fastqc):
`docker run -d --rm -v $PWD:/data pegi3s/fastqc /data/<fastq_name>`
* fastqc reports [for R1](http://149.156.177.112/projects/ifpan-marpiech-wgs/illumina-fq/mp_S0_L002_R1_001_fastqc.html) and [for R2](http://149.156.177.112/projects/ifpan-marpiech-wgs/illumina-fq/mp_S0_L002_R2_001_fastqc.html)
* Warnings (in both cases): Per tile sequence quality and per sequence GC content

3. alignment bwa-mem do hg38 z broadinstitute to bam (https://storage.cloud.google.com/genomics-public-data/resources/broad/hg38/v0/Homo_sapiens_assembly38.fasta?_ga=2.192558178.-935441401.1560518376) + fai

4. gatk best practices variant calling


# 10x genomics
1. use longranger to convert bcl to fastq:
`longranger mkfastq --run . --csv SampleSheet.csv --output-dir longrangerfq`
This step can be alternatively done with Illuminas bcl2fastq with [this code](https://gist.github.com/gosborcz/bc6896406b776c41e83c37d7568cbe1a)

2. fastqc only on R2 file as I1 contains indexes and R1 barcodes, with [this docker image](https://hub.docker.com/r/pegi3s/fastqc):
`docker run -d --rm -v $PWD:/data pegi3s/fastqc /data/<fastq_name>`
* Warnings: Per base sequence quality, Per tile sequence quality, Per sequence quality scores, Per base sequence content, Per sequence GC content

3. continue with the [longranger pipelines](https://support.10xgenomics.com/genome-exome/software/pipelines/latest/what-is-long-ranger)
**longranger wgs with gatk** 
`gatk-package-4.0.3.0-local.jar` gatk

#software versions:
1. bcl2fastq v2.20.0.422
2. logranger 2.2.2
3. FastQC v0.11.7
4. gatk 4.03 *this is for longranger as it does not suppor newer versions for now*

#reference genome:
https://console.cloud.google.com/storage/browser/genomics-public-data/resources/broad/hg38/v0
