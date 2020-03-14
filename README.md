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

This container is designed to be run using Singularity. Version 3.0.1-104.g4feafc42.dirty.el7 was used during testing.

To download the container: TBD

To run you must provide the absolute paths to files that are required.
This can be done by using the --bind parameters to singularity like this:

    singularity run --bind /path/to/reference/fasta:/ref,/path/to/tumour_bam/folder/:/tumour_path,/path/to/normal_bam/folder/:/normal_path PROFYLE_TMB -t /tumour_path/tumour_bam.bam -n  /normal_path/normal.bam -r /ref/hg19a.fa -o `pwd`

## Please Note   
All steps will try to use 48 threads
Currently only works for hg19 genome bams without the 'chr' prefix.

## Output
The output folder will contain a file named results.txt where the following will be reported:
Non-N bases in 1-22,X,Y:    Count of the bases used as the whole genome calculation denominator
CDS bases in 1-22,X,Y:      Count of the unique CDS bases used in the coding calculation denominator
Total genome SNVs:          Number of SNVs called by Strelka 2
Total genome Indels:        Number of Indels called by Strelka 2
Coding SNVs:                Number of Coding SNVs called by Strelka 2
Coding Indels:              Number of COding Indels called by Strelka 2

Genome SNV TMB:             total_SNVs * 1000000 / total_bases
Genome Indel TMB:           total_Indels * 1000000 / total_bases
Coding SNV TMB:             coding_SNVs * 1000000 / CDS_bases
Coding Indel TMB:           coding_Indels * 1000000 / CDS_bases
MSI score:                  Fraction of sites reported as MSI by MSIsensor


