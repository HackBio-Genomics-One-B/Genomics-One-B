
# [HackBio Internship 2021](https://thehackbio.com/): Genomics-One-B Project

## ⛳️ Table of Contents
<!-- markdownlint-disable -->
  - [🧬 Stage 2 Task](#-stage-2-task-calling-variants-in-non-diploid-systems)
  - [🚀 Project Workflow](#-project-workflow)
  - [📙 Instructions](#-instructions)
  - [1️⃣ Step 1 - IMPORTING DATASET](#1%EF%B8%8F%E2%83%A3-step-1---IMPORTING-DATASET) 
  - [2️⃣ Step 2 - QUALITY CHECK OF DATASET](#2%EF%B8%8F%E2%83%A3-step-2---QUALITY-CHECK-OF-DATASET) 
  - [3️⃣ Step 3 - MAPPING THE READS USING BWA MEM](#3%EF%B8%8F%E2%83%A3-step-3---MAPPING-THE-READS-USING-BWA-MEM )
  - [4️⃣ Step 4 - POST-PROCESSING MAPPED READ](#4%EF%B8%8F%E2%83%A3-step-4---POST-PROCESSING-MAPPED-READ) 
  - - [➡️ 4.1 Merging BAM Datasets](#%EF%B8%8F-41-merging-bam-datasets)
  - - [➡️ 4.2 Removing Duplicates](#%EF%B8%8F-42-removing-duplicates)
  - - [➡️ 4.3 Left-Aligning Indels](#%EF%B8%8F-43-left-aligning-indels)
  - - [➡️ 4.4 Filtering Reads](#%EF%B8%8F-44-filtering-reads)
  - [5️⃣ Step 5 - CALLING NON-DIPLOID VARIANTS USING FREEBAYES](#5%EF%B8%8F%E2%83%A3-step-5---CALLING-NON-DIPLOID-VARIANTS-USING-FREEBAYES)
  - [6️⃣ Step 6 - FILTERING VARIANTS USING VCF](#6%EF%B8%8F%E2%83%A3-step-6---FILTERING-VARIANTS-USING-VCF)
  - [7️⃣ Step 7 - VISUALIZATION](#7%EF%B8%8F%E2%83%A3-step-7---VISUALIZATION)
  - - [➡️ 7.1 Visualising with VCF.IOBIO](#%EF%B8%8F-71-visualising-with-vcfiobio)
  - - [➡️ 7.2: Visualising with IGV](#%EF%B8%8F-72-visualising-with-igv)
  - [8️⃣ Step 8 - COMPARING FREQUENCIES](#8%EF%B8%8F%E2%83%A3-step-8---COMPARING-FREQUENCIES)
  - - [➡️ 8.1 Convert VCF to tab-delimited data](#%EF%B8%8F-81-convert-vcf-to-tab-delimited-data)
  - - [➡️ 8.2: Cut columns from a file](#%EF%B8%8F-82-cut-columns-from-a-file)
  - [🔮 Interpretation of Results]()
  - [🏁 Team Results](#-team-results)
  - [🔥 Contributors](#-contributors)

# 🧬 Stage 2 Task: Calling Variants in Non-diploid Systems

# 🚀 Project Workflow

![image](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/mike/PROJECT_DESIGN_(GENOMICS_1B).png)

# 📙 Instructions  
## Calling variants in non-diploid systems 
A handful of life ranging from prokaryotes, down to viruses and a few extension operate on non-diploid mechanism.
In this tutorial Team Genomics_One_B will be recreating the above project which involves working on four datasets, gotten from human genomic DNA sequencing. The aim of this is to identify heteroplasmies variant within the mitochondria DNA using Galaxy packages.

By: Anton Nekrutenko and Alex Ostrovsky

This tutorial is implemented in [galaxy](https://usegalaxy.org) via https://usegalaxy.org <br/>

Always use the search button to navigate the respective tools. 

[List of contributors](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/main/List_of_contributors.md)  

[The galaxy tutorial we followed](https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/non-dip/tutorial.html#checking-data-quality)  

# 1️⃣ STEP 1 - IMPORTING DATASET

- Download datasets from resource page
- Click upload data on Galaxy web page
- Galaxy will prompt to ask if it is from the local files or web (it depends on where you saved the dataset)
- After uploading, click start. Once import is completed, the dataset highlight turns green as seen on the picture below.

![gd](https://user-images.githubusercontent.com/77963733/130158462-53243352-6693-4370-b0aa-2223834cb571.jpg)

Import raw reads from [here](https://zenodo.org/record/1251112)

```
https://zenodo.org/record/1251112/files/raw_child-ds-1.fq
https://zenodo.org/record/1251112/files/raw_child-ds-2.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-1.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-2.fq
```
<br/>

# 2️⃣ STEP 2 - QUALITY CHECK OF DATASET
### 🛠️ Tool: ``FastQC``<br/>
It is important to check the quality of the data to be used before proceeding with the analysis. This is done to determine if there is a problem with the dataset. Click on  FASTA/Fastq on the left hand side, select 'FastQC Read Quality Check' and execute. It will run a check on the data.
### 🎯 Parameters: <br/>
*Short read data from your current history:* `all 4 FASTQ datasets selected with Multiple datasets`<br/>

### 💡 Tips: <br/>
To select multiple datasets, <br/>
click on the Multiple datasets icon<br/>
select several files by keeping the ``Ctrl`` (or ``COMMAND``) key pressed and clicking on all 4 FASTQ files
<br/>
![gc](https://user-images.githubusercontent.com/77963733/130158592-ceedd90b-8761-4289-8284-504bf35ae368.jpg)
<br/>
![gd](https://user-images.githubusercontent.com/77963733/130158596-09ad1ed9-390e-44a3-a3c5-64cb2e0d0100.png)
<br/>
<br/>
# 3️⃣ STEP 3 - MAPPING THE READS USING BWA MEM 
### Tool: ``BWA-mem``
Human genome, ‘hg38’ was used as the reference genome.Using the Paired end sequencing, the datasets has to be uploaded by selecting multiple datasets as follows:

### 🎯 Parameters: <br/>
- *select a reference genome from your history or use a built-in index?:* ``Use a built-in genome index``<br/>
- *Using reference genome:* ``Human: hg38``<br/>
- *Single or Paired-end reads:* ``Paired``<br/>
- *Select first set of reads:* ``select both -1 datasets selected with Multiple datasets``<br/>
- `raw_child-ds-1.fq` & `raw_mother-ds-1.fq `
- *Select second set of reads:* ``select both -2 datasets selected with Multiple datasets``<br/>
- `raw_child-ds-2.fq` & `raw_mother-ds-2.fq`
- *Set read groups information?:* ``Set read groups (SAM/BAM specification)``<br/>
- *Auto-assign:* ``Yes``<br/>
- *Auto-assign:* ``Yes``<br/>
- *Platform/technology used to produce the reads (PL):* ``ILLUMINA``<br/>
- *Auto-assign:* ``Yes``<br/>
- *Execute<br/>* <br/>

![img_20210820_131657](https://user-images.githubusercontent.com/77963733/130246896-a965760e-b51a-49b4-bb65-1dab11dbccfb.jpg)

![img_20210820_131439](https://user-images.githubusercontent.com/77963733/130246959-8e69f0ab-3385-4daa-8d69-2b0f60913e41.jpg)
<br/>
<br/>
# 4️⃣ STEP 4 - POST-PROCESSING MAPPED READ

## ➡️ 4.1 Merging `BAM` Datasets

### 🛠️ Tool: ``Picard's MergeSAMFiles``<br/>
### 🎯 Parameters:<br/>
- *Select SAM/BAM dataset or dataset collection obtained from Step 3 into the dataset collection:* `Both BAM datasets produced by BWA-MEM tool`<br/>
- *Select validation stringency:* `Lenient`<br/>  
- Execute.

![41](https://user-images.githubusercontent.com/77963733/130158840-9923d314-6841-44cd-a4cb-33d738e1a208.png)

## ➡️ 4.2: Removing duplicates using MarkDuplicates

### 🛠️ Tool: ``Picard's MarkDuplicates``<br/>
### 🎯 Parameters:<br/>
-   or dataset collection box. 
- *Click on `MarkDuplicates` in the `SAM/BAM` dataset  or dataset collection:* ``The merged BAM dataset produced by MergeSAMFiles tool from step 4.1``<br/>
- *The scoring strategy for choosing the non-duplicate among candidates:* ``SUM_OF_BASE_QUALITIES``<br/>
- *The maximum offset between two duplicate clusters in order to consider them optical duplicates:* ``100``<br/>
- *Select validation stringency:* ``Lenient``<br/>
- Execute

![42](https://user-images.githubusercontent.com/77963733/130158847-44d2da3e-d50e-46f8-a2ae-c8f7a2d676cc.png)

## ➡️ 4.3: Left-Aligning Indels using BamLeftAlign Tool

### 🛠️ Tool: ``BamLeftAlign``<br/>

Left aligning of indels is important for obtaining accurate variant calls (The BAM dataset generated by MarkDuplicates will be used to run this step)

### ⚠️ Required step before executing Step 4.3:<br/>
Click on the ``Pencil`` icon of the BAM dataset produced in **Step 4.2** to edit ``attributes`` <br/>
Select `hg38` under the `Database/Build` option and save.

### 🎯 Parameters:<br/>
- *Choose the source for the reference genome:* ``Locally cached``<br/>
- *Select alignment file in BAM format:* ``The BAM dataset produced by MarkDuplicates tool``<br/>
- *Using reference genome:* ``hg38``<br/>
- *Maximum number of iterations:* ``5``<br/>
- Execute

![43](https://user-images.githubusercontent.com/77963733/130158853-bc589b3e-3841-44cc-b5fa-4951f483459e.png)


## ➡️ 4.4: Filtering Reads

### 🛠️ Tool: ``BAMTools Filter``<br/>
### 🎯 Parameters:<br/>
- *BAM dataset(s) to filter:* ``Select the BAM dataset produced by BamLeftAlign tool``<br/>

 - Under ``Condition`` > ``1: Condition`` > ``Filter``:<br/>
  - **In ``1: Filter``:**<br/>
    - *Select BAM property to filter on:* ``mapQuality``<br/>
    - *Filter on read mapping quality (phred scale):* ``>=20``<br/>

    - Click on ``Insert Filter``<br/>

  - **In ``2: Filter``:**<br/>
    - *Select BAM property to filter on:* ``isPaired``<br/>
    - *Selected mapped reads:* ``Yes``<br/>

    - Click on ``Insert Filter``<br/>

  - **In ``3: Filter``:**<br/>
    - *Select BAM property to filter on:* ``isProperPair``<br/>
    - *Select reads with mapped mate:* ``Yes``<br/>

    - Click on ``Insert Filter``<br/>

  - **In ``4: Filter``:**<br/>
    - *Select BAM property to filter on:* ``reference``<br/>
    - *Select reads with mapped mate:* ``chrM``<br/>
<br/>

- Execute  (**NB**: the parameter, 'would you like to set rules' should be set to NO)

![44](https://user-images.githubusercontent.com/77963733/130158864-3000b582-0429-48cb-9655-f5b4b1ec683a.png)

# 5️⃣ STEP 5 - CALLING NON-DIPLOID VARIANTS USING FREEBAYES

### 🛠️ Tool: ``FreeBayes``<br/>
You can navigate to the tool (FreeBayes) using the search button in Galaxy. Select the reference genome, mode of run and the BAM file input. Set the parameters for the following options (population mode, allelic scope, input filter) as seen in the images below.

### 🎯 Parameters:<br/>
- *Choose the source for the reference genome:* ``Locally cached``<br/>
- *Run in batch mode?:* ``Run individually``<br/>
- *BAM dataset:* ``select the BAM dataset produced by last Filter tool step``<br/>
- *Using reference genome:* ``hg38``<br/>
- *Limit variant calling to a set of regions?:* ``Limit to region``<br/>
- *Region Chromosome:* ``chrM``<br/>
- *Region Start:* ``1``<br/>
- *Region End:* ``16000``<br/>
![51](https://user-images.githubusercontent.com/77963733/130159354-9c941c1e-52a2-4345-8245-6fbb3c776318.jpg)

- *Choose parameter selection level:* ``5: Full list of options``<br/>
- *Population model options:* ``Set population model options``<br/>
- *The expected mutation rate or pairwise nucleotide diversity among the population under analysis:* ``0.001``<br/>
- *Set ploidy for the analysis:* ``1``<br/>
- *Assume that samples result from pooled sequencing:* ``Yes``<br/>
- *Output all alleles which pass input filters, regardless of genotyping outcome or model:* ``Yes``<br/>
![52](https://user-images.githubusercontent.com/77963733/130159365-17e62592-2693-4b66-9c2f-f79d406e9ec3.jpg)

- *Allelic scope options:* ``Set allelic scope options``<br/>
- *Ignore SNP alleles:* ``No``<br/>
- *Ignore indels alleles:* ``No``<br/>
- *Ignore multi-nucleotide polymorphisms, MNPs:* ``Yes``<br/>
- *Ignore complex events (composites of other classes):* ``Yes``<br/>
![53](https://user-images.githubusercontent.com/77963733/130159377-fa30dab0-0547-4e3d-8047-40ababc86442.jpg)

- *Input filters:* ``Set input filters``<br/>
- *Exclude alignments from analysis if they have a mapping quality less than:* ``20``<br/>
- *Exclude alleles from analysis if their supporting base quality less than:* ``30``<br/>  
- Execute <br/>
![54](https://user-images.githubusercontent.com/77963733/130159384-10e83cd7-b90d-4c36-82e3-205fe3999671.jpg)
<br/>
<br/>
# 6️⃣ STEP 6 - FILTERING VARIANTS USING VCF

### 🛠️ Tool: ``VCFfilter``<br/>

### 🎯 Parameters:<br/>

*VCF dataset to filter*: ``select the VCF dataset  obtained from variant call (step 5) produced by FreeBayes tool``<br/>

In ``more filters``:<br/>
 - **In ``1: more filters``:**<br/>
   - *Select the filter type*:* ``Info filter (-f)``<br/>
   - *Specify filtering value*: ``SRP > 20``<br/>
  
   - Click on ``Insert more filters``<br/>

 - **In ``2: more filters``:**<br/>
   - *Select the filter type*: ``Info filter (-f)``<br/>
   - *Specify filtering value*: ``SAP > 20``<br/>

   - Click on ``Insert more filters``<br/>

 - **In ``3: more filters``:**<br/>
   - *Select the filter type*: ``Info filter (-f)``<br/>
   - *Specify filtering value*: ``EPP > 20``<br/>

   - Click on ``Insert more filters``<br/>

 - **In ``4: more filters``:**<br/>
   - *Select the filter type*: ``Info filter (-f)``<br/>
   - *Specify filtering value*: ``QUAL > 20``<br/>
![61](https://user-images.githubusercontent.com/77963733/130159393-bf828636-a389-4580-a54e-3f8e20702da6.jpg)

Click on ``Insert more filters``<br/>

 - **In ``5: more filters``:**<br/>
   - *Select the filter type*: ``Info filter (-f)``<br/>
   - *Specify filtering value*: ``DP > 20``<br/>
- Execute<br/>
<br/>

![62](https://user-images.githubusercontent.com/77963733/130159398-e5ae3a89-0118-4fa6-ac52-5135899eaf76.jpg)
<br/>
<br/>
# 7️⃣ STEP 7 - VISUALIZATION
Processed data in VCF format imported onto the galaxy platform and change the reference genome to Human hg38 for comparison<br/>

![igvimage](https://user-images.githubusercontent.com/77963733/130161840-b9f7bc0c-9b1f-4b9b-9983-78e467fc78a1.jpg)<br/>

## ➡️ 7.1 Visualising with VCF.IOBIO
VCF.IOBIO link is also generated when the VCF dataset is uploaded that will give us the data on Varient types, Allele Frequency Spectrum and Base Changes.

### 🛠️ Tool: ``VCF.IOBIO``<br/>
### 🎯 Process:<br/>
- Click on processed VCF datasets, it will expand to show link.<br/>
- Click on ``display at vcf.iobio`` at the bottom<br/>
- Use the reference genome, Human hg38 for comparison<br/>
- VCF datasets will be index to display them<br/>
<br/>

![vcfiobioimage](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/main/7_Visualize/Visualising_with_VCF_IOBIO.jpg)<br/>

## ➡️ 7.2: Visualising with IGV
The data can be visualised via IGV locally and focus on varient at position 3243 and the Genotypic information can also be obtained.
### 🛠️ Tool: ``IGV``<br/>
### 🎯 Process:<br/>
- Click on processed VCF datasets, it will expand to show link.<br/>
- Click on ``display with IGV local`` at the bottom<br/>
- Use the reference genome, Human hg38 for comparison<br/>
- VCF datasets will be index to display them<br/>
<br/>

![igvimage](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/main/7_Visualize/Visualization%20using%20IGV.jpeg)<br/>
<br/>
<br/>
# 8️⃣ STEP 8 - COMPARING FREQUENCIES
Though visualizing VCF datasets is a good way to get an overall idea, it does not explain many details. To play a little more with data, 
## ➡️ 8.1 Convert VCF to tab-delimited data
Convert VCF dataset into a tab-delimited representation using VCFtoTab-delimited
### 🛠️ Tool: ``VCFtoTab-delimited``<br/>
### 🎯 Parameters:<br/>
- *Select VCF dataset to convert*: ``select the VCF dataset produced by VCFfilter tool``<br/>
- *Report data per sample*: ``Yes``<br/>
- *Fill empty fields with*: ``Nothing``<br/>

![IMG-20210820-WA0017](https://user-images.githubusercontent.com/77963733/130201353-bd30b6ca-2aac-45f2-bc6c-b4038cf6d326.jpg)

## ➡️ 8.2: Cut columns from a file
As we opted for “Report data per sample”(four), this will produce a dataset with many columns (In this tutorial, 62 columns were produced out of which only six are necessary)
### 🛠️ Tool: ``Cut columns from a table``<br/>
### 🎯 Parameters:<br/>
- *Cut columns*: ``c2,c4,c5,c52,c54,c55``<br/>
- *Delimited by*: ``Tab``<br/>
- *From*: ``select the tabular dataset produced by VCFtoTab-delimited``<br/>

![IMG-20210820-WA0018](https://user-images.githubusercontent.com/77963733/130201363-eae11cda-b070-4fb7-8583-315901440411.jpg)

Then proceed to cut these columns out (refer to image below)

![IMG-20210820-WA0019](https://user-images.githubusercontent.com/77963733/130201371-2038a041-6198-4657-9493-50a995f4e77e.jpg)
<br/>
<br/>

# 🔮 Interpretation of Results

![IMG-20210820-WA0020](https://user-images.githubusercontent.com/77963733/130201389-a7c1cae4-d4ce-46ae-9547-65a1da69ed08.jpg)

At position 3243, the mother sample has 671 G’s (‘G’ – an alternative allele) and depth of coverage is 2057 so, 2057-671 = 1386 A’s. At the same position, the child sample has 694 G’s and 1035-694 = 341 A’s. 


| Allele | A | G   |
| :----------- | :---------: | -----------: |
| Mother         | 1386            | 671   |
| Child          | 341             | 694   |

We noticed a remarkable frequency change i.e., the major allele in the mother ‘A’ becomes the minor allele in the child.
<br/>
<br/>
# 🏁 Team Results

To access our data and results on the drive click   <a href="https://drive.google.com/drive/folders/1oafSVbpr8Nz0YXFxoqBtIIDefZ8NcWbd" target="blank"><img align="center" src="https://user-images.githubusercontent.com/77963733/130207235-ab81908f-62cd-45c7-9e0c-408831a6f164.png" height="20" width="20" /></a> </p>
<br/>
<br/>
# 🔥 Contributors
        
| Name | Activity | 
| --- | ------------ |
| `Gautami` | **Team Leader**, Visualization using IGV and VCF.IOBIO; and Comparing of frequencies using VCFtoTab-delimited |  
| `Temmykeji` |  Graphic design of workflow, Dataset and FastQC |  
| `Solomon` |  Dataset, FastQC and Github Markdown |  
| `Rajeshcha44` |  Mapping of read using BWA-MEM |  
| `Nitigya-M` | Mapping of read using BWA-MEM  |  
| `abdnahid_` | Merging BAM datasets with MergeSAMFiles, Removing duplicates with MarkDuplicates, Left-aligning indels with BamLeftAlign, Filtering reads with BAMTools filter and GitHub Markdown |  
| `mike` | Merging BAM datasets with MergeSAMFiles, Removing duplicates with MarkDuplicates, Left-aligning indels with BamLeftAlign, Filtering reads with BAMTools filter and GitHub Markdown |
| `Karteek` | Merging BAM datasets with MergeSAMFiles, Removing duplicates with MarkDuplicates, Left-aligning indels with BamLeftAlign, Filtering reads with BAMTools filter |  
| `Priyacomp` |  Variant calling of dataset |  
| `MANGAIYARKARASI` | Variant calling of dataset |  
| `Pragna_lakshmi` |  Variant calling of dataset using FreeBayes and Comparing of frequencies using VCFtoTab-delimited |  
| `Naomi` | Mapping of read using BWA-MEM |  
| `Galaxy` |  Filtering of variant call dataset using FreeBayes |  
| `Aarathi04` | Filtering of variant call dataset using VCFfilter |  
| `ZubairAlam` | Visualization using IGV and VCF.IOBIO |  
| `Shreyashi` |  Visualization using IGV and VCF.IOBIO |  
| `omimiIII` | Comparing of frequencies using VCFtoTab-delimited and Github Markdown |  



