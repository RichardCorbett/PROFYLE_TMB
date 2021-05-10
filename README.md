# Deprecated and merged into https://github.com/bcgsc/WGS_TMB


# PROFYLE_TMB

This container will run the following tools in order to estimate the 
	TMB for the provided sample:
	Manta V1.6.0
	Strelka 2.9.2
	SnpEff 4.3t GRCh37.75
	msisensor 0.2
        
 
The steps of analysis are:
1. If necessary, count the non-N bases of the provided reference on the 1-22,X,Y chromosomes
1. Call SVs with Manta (recommended for Strelka 2)
1. Call SNVs and Indels with Strelka 2
1. Annotate with snpEff
1. Calculate MSI with msisensor

## Getting started

This container is designed to be run using Singularity. Testing was performed using Singularity Version 3.0.1-104.g4feafc42.dirty.el7.

To download the container: 

    singularity pull library://richardcorbett/profyle_tmb/profyle_tmb:*version*

for example:

    singularity pull library://richardcorbett/profyle_tmb/profyle_tmb:v0_0_2

To run you must provide the absolute paths to files that are required.
This can be done by using the --bind parameters to singularity like this:

    singularity run --bind /path/to/reference/fasta:/ref,/path/to/tumour_bam/folder/:/tumour_path,/path/to/normal_bam/folder/:/normal_path profyle_tmb_v0_0_1.sif -t /tumour_path/tumour_bam.bam -n  /normal_path/normal.bam -r /ref/hg19a.fa -o `pwd`

## Please Note   
* All steps will use 48 threads where possible
* The machine where analysis is performed should have at least 64Gb of RAM
* Due to the nature of the analysis applying the container to many samples (>20) at once may add heavy load to your filesystem.
* The raw VCF files for each type of analysis are compressed and kept.   This may result in up to 500Mb of files for each sample.


## Output
The output folder will contain a file named results.txt where the following will be reported:

Field | Comment
----- | -------
 Non-N bases in 1-22,X,Y |   Count of the bases used as the whole genome calculation denominator
 CDS bases in 1-22,X,Y |      Count of the unique CDS bases used in the coding calculation denominator
 Total genome SNVs |          Number of passed SNVs called by Strelka 2
 xTotal genome Indels |        Number of passed Indels called by Strelka 2
 Coding SNVs |                 Number of passed Coding SNVs called by Strelka 2
 Coding Indels |               Number of passed Coding Indels called by Strelka 2
 Genome SNV TMB |              total_SNVs * 1000000 / total_bases
 Genome Indel TMB |            total_Indels * 1000000 / total_bases
 Coding SNV TMB |              coding_SNVs * 1000000 / CDS_bases
 Coding Indel TMB |           coding_Indels * 1000000 / CDS_bases
 MSI score |                   Fraction of sites reported as MSI by MSIsensor


