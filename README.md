# [HackBio internship 2021](https://thehackbio.com/): Genomics-One-B

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience

# PROJECT WORKFLOW & DESIGN
![hackbio ads](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/main/PROJECT%20DESIGN%20(GENOMICS%201B).png?raw=true)

## Step-by-Step 

### Task distribution:

|STEP|TASK|COLLABORATORS' SLACK USERNAME |PROFILE PICTURE|
|----|--------------|--------------------|---------------|
|1|Importing data (From Galaxy Library) |@Solomon @Temmykeji|******|
|2|Checking data quality using FastQC|@Solomon @Temmykeji|******|
|3|Mapping reads to a reference using BWA-MEM|@Rajeshcha44 @Nitigya-M|******|
|4|**Post-processing mapped reads** _*Merge datasets using SAMFiles_ _*Remove duplicates using MarkDuplicates_  _*Left aligning indels using BAM left align_ _*Filtering reads_|@abdnahid_ @Mike @Karteek|******|      
|5|Calling non-diploid variants using FreeBayes|@Priyacomp @MANGAIYARKARASI @pragna_lakshmi|******|
|6|Filtering variants using VCFfilte|@Naomi @Galaxy @Aarathi04|******|
|7|Visualization using IGV|@Gautami @Shreyashi @ZubairAlam|******|
|8|Compare frequencies|@omimiIII @Temmykeji @pragna_lakshmi @Gautami|******|



# Calling variants in non-diploid systems
#### _By: AvatarAnton Nekrutenko AvatarAlex Ostrovsky_

## Questions
How does frequency of mitochondrial polymorphisms change from mother to child?

## Objectives
Using Galaxy’s main site we will see how to call variants in bacteria, viruses, and organelles.

## Requirements
1. Introduction to Galaxy Analyses
2. Sequence analysis
      - Quality Control
      - Mapping

## Introduction
The majority of life on Earth is non-diploid and represented by prokaryotes, viruses, and their derivatives, such as our own mitochondria or plant’s chloroplasts. In non-diploid systems, allele frequencies can range anywhere between 0 and 100% and there could be multiple (not just two) alleles per locus. The main challenge associated with non-diploid variant calling is the difficulty in distinguishing between the sequencing noise (abundant in all NGS platforms) and true low frequency variants. Some of the early attempts to do this well have been accomplished on human mitochondrial DNA although the same approaches will work equally good on viral and bacterial genomes (Rebolledo-Jaramillo et al. 2014, Li et al. 2015).

As an example of non-diploid systems, we will be using human mitochondrial genome. However, this approach will also work for most bacterial and viral genomes.

There are two ways one can call variants:
1. By comparing reads against an existing genome assembly
2. By assembling a genome first and then mapping against that assembly



![](https://training.galaxyproject.org/training-material/topics/variant-analysis/images/ref_vs_assembly.jpg)

**Figure 1:** This figure from Olson et al. 2015 contrasts the two approaches.

In this tutorial we will take the first path, in which we map reads against an existing assembly. Later in the course, after we learn about assembly approaches, we will try the second approach as well.

The goal of this example is to detect heteroplasmies - variants within mitochondrial DNA. Mitochondria are transmitted maternally, and heteroplasmy frequencies may change dramatically and unpredictably during the transmission due to a germ-line bottleneck (Cree et al. 2008). As we mentioned above, the procedure for finding variants in bacterial or viral genomes will be essentially the same.

Datasets representing a child and a mother are available in Zenodo. These datasets were obtained by paired-end Illumina sequencing of human genomic DNA enriched for mitochondria. The enrichment was performed using long-range PCR with two primer pairs that amplify the entire mitochondrial genome. Samples will therefore still contain a lot of DNA from the nuclear genome, which, in this case, is a contaminant.

### Agenda
In this tutorial, we will cover:

1. Importing data
      - Checking data quality
2. Mapping reads to a reference
3. Postprocessing mapped reads
      - Merging BAM datasets
      - Removing duplicates
      - Left-aligning indels
      - Filtering reads
4. Calling non-diploid variants
      - Filtering variants
5. Examining the results
      - Visualising with VCF.IOBIO
      - Visualising with IGV
      - Comparing frequencies
6. Conclusion

## Importing data
For this tutorial we have prepared a subset of data previously by our group (Rebolledo-Jaramillo et al. 2014). Let’s import these data into Galaxy. They are available from this Galaxy Library or via Zenodo

### Hands-on: Get the data
1. Create a new history for this tutorial and give it a meaningful name
2. Import files from Zenodo:
[](https://zenodo.org/record/1251112/files/raw_child-ds-1.fq)
[](https://zenodo.org/record/1251112/files/raw_child-ds-2.fq)
[](https://zenodo.org/record/1251112/files/raw_mother-ds-1.fq)
[](https://zenodo.org/record/1251112/files/raw_mother-ds-2.fq)

3. Check that all newly created datasets in your history are assigned datatype fastqsanger, and fix any missing or wrong datatype assignment

## Checking data quality
Before proceeding with the analysis, we need to find out how good the data actually is. For this will use FastQC.

### Hands-on: Assess quality of data
1. Run [FastQC Tool:](toolshed.g2.bx.psu.edu/repos/devteam/fastqc/fastqc/0.72+galaxy1) on each of the four FASTQ datasets with the following parameters:
param-files 
      - “Short read data from your current history”: all 4 FASTQ datasets selected with Multiple datasets


Once the FastQC jobs runs, you will be able to look at the HTML reports generated by this tool. The data have generally high quality in this example:
![](https://drive.google.com/drive/folders/1oeJ6eVhpZ_Dj-hbW3l2F1kdL69TrlP-f)







**Figure 2:** FastQC plot for one of the mitochondrial datasets shows that qualities are acceptable for 250 bp reads (mostly in the green, which is at or above [Phred score](https://en.wikipedia.org/wiki/Phred_quality_score) of 30).



## Mapping reads to a reference
Our reads are long (250 bp) so we will use BWA-MEM (Li 2013) to align them against the reference genome as it has good mapping performance for longer reads (100bp and up).

### nHands-on: Map reads
Use [Map with BWA-MEM Tool:](toolshed.g2.bx.psu.edu/repos/devteam/bwa/bwa_mem/0.7.17.1) to map the reads to the reference genome with the following parameters:
- “Will you select a reference genome from your history or use a built-in index?”: Use a built-in genome index
      - “Using reference genome”: Human: hg38 (or a similarly named option)
- “Single or Paired-end reads”: Paired
      - “Select first set of reads”: select both -1 datasets selected with **Multiple datasets**
      - “Select second set of reads”: select both -2 datasets selected with **Multiple datasets**
- “Set read groups information?”: Set read groups (SAM/BAM specification)
      - “Auto-assign”: Yes
      - “Auto-assign”: Yes
      - “Platform/technology used to produce the reads (PL)”: ILLUMINA
      - “Auto-assign”: Yes
      - 
### More about selecting datasets
By selecting datasets 1 and 3 as **Select the first set of reads** and datasets 2 and 4 as **Select the second set of reads**, Galaxy will automatically launch two BWA-MEM jobs using datasets 1,2 and 3,4 generating two resulting BAM files. By setting **Set read groups information** to Set read groups (SAM/BAM specifications) and clicking **Auto-assign** we will ensure that the reads in the resulting BAM dataset are properly set.

## Postprocessing mapped reads
### Merging BAM datasets
Because we have set read groups, we can now merge the two BAM dataset into one. This is because read groups label each read as belonging to either mother or child.

### Hands-on: Merge multiple datasets into one
1. Use MergeSAMFiles Tool: toolshed.g2.bx.psu.edu/repos/devteam/picard/picard_MergeSamFiles/2.18.2.1to merge the BAM datasets with the following parameters:
      - “Select SAM/BAM dataset or dataset collection”: select both BAM datasets produced by **BWA-MEM**
      - “Select validation stringency”: Lenient
### Removing duplicates
Preparation of sequencing libraries (at least at the time of writing) for technologies such as Illumina (used in this example) involves PCR amplification. It is required to generate sufficient number of sequencing templates so that a reliable detection can be performed by base callers. PCR has its own biases which are especially profound in cases of multi-template PCR used for construction of sequencing libraries (Kanagawa 2003).

Duplicates can be identified based on their outer alignment coordinates or using sequence-based clustering. One of the common ways for identification of duplicate reads is the MarkDuplicates utility from Picard package which is designed to identify both PCR and optical duplicates.

### More about MarkDuplicates
### Hands-on: De-duplicate mapped data
1. Use [MarkDuplicates](toolshed.g2.bx.psu.edu/repos/devteam/picard/picard_MarkDuplicates/2.18.2.2)to de-duplicate the merged BAM datasets with the following parameters:
      - “Select SAM/BAM dataset or dataset collection”: select the merged BAM dataset produced by **MergeSAMFiles**
      - “The scoring strategy for choosing the non-duplicate among candidates”: SUM_OF_BASE_QUALITIES
      - “The maximum offset between two duplicate clusters in order to consider them optical duplicates”: 100
      - “Select validation stringency”: Lenient
      
MarkDuplicates produces a BAM dataset with duplicates removed and also a metrics file. Let’s take a look at the metrics data:
