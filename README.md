# [HackBio Internship 2021](https://thehackbio.com/): Genomics-One-B Project

# Calling Variants in Non-diploid Systems

# PROJECT WORKFLOW

![image](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/mike/PROJECT_DESIGN_(GENOMICS_1B).png)


## ⛳️ Table of Contents
<!-- markdownlint-disable -->
  - [🧬 Instructions](#-hackbio-2021)
  - [🎮 The Stage 0 Task](#-#the-stage-0-task)
  - [🛡 Dependencies](#-dependencies)
  - [💡 Installation](#-Installation)
  - [📙 Information Template](#-information-template)
  - [🚀 Get Started](#-get-started)
  - [✅ Code Submitted](#-code-submitted)
  - [💻 Programming Languages](#-programming-languages)
  - [🔥 Contributors](#-contributors)

## Instructions  
This tutorial is implemented in galaxy (https://usegalaxy.org) <br/>
Always use the search button to navigate the respective tools. 

[List of contributors](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/mike/List_of_contributors.md)  
[The galaxy tutorial we followed](https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/non-dip/tutorial.html#checking-data-quality)  

# 1️⃣ STEP 1 - IMPORTING DATA
Import raw reads from [here](https://zenodo.org/record/1251112)

```
https://zenodo.org/record/1251112/files/raw_child-ds-1.fq
https://zenodo.org/record/1251112/files/raw_child-ds-2.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-1.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-2.fq
```

# 2️⃣ STEP 2 - QUALITY CHECKING

### 🛠️ Tool: ``FastQC``<br/>
### 🎯 Parameters: <br/>
*Short read data from your current history:* `all 4 FASTQ datasets selected with Multiple datasets`<br/>
### 💡 Tips: <br/>
To select multiple datasets, <br/>
click on the Multiple datasets icon<br/>
select several files by keeping the ``Ctrl`` (or ``COMMAND``) key pressed and clicking on all 4 FASTQ files

# 3️⃣ STEP 3 - MAPPING READS TO REFERENCE GENOME 

### Tool: ``BWA-mem``
### 🎯 Parameters: <br/>
*Will you select a reference genome from your history or use a built-in index?:* ``Use a built-in genome index``<br/>
*Using reference genome:* ``Human: hg38``<br/>
*Single or Paired-end reads:* ``Paired``<br/>
*Select first set of reads:* ``select both -1 datasets selected with Multiple datasets``<br/>
*Select second set of reads:* ``select both -2 datasets selected with Multiple datasets``<br/>
*Set read groups information?:* ``Set read groups (SAM/BAM specification)``<br/>
*Auto-assign:* ``Yes``<br/>
*Auto-assign:* ``Yes``<br/>
*Platform/technology used to produce the reads (PL):* ``ILLUMINA``<br/>
*Auto-assign:* ``Yes``<br/>

# 4️⃣ STEP 4 - POSTPROCESSING MAPPED READS

## ➡️ 4.1 MERGING BAM DATASETS

### 🛠️ Tool: ``Picard's MergeSAMFiles``<br/>
### 🎯 Parameters:<br/>
*Select SAM/BAM dataset or dataset collection:* `Both BAM datasets produced by BWA-MEM tool`<br/>
*Select validation stringency:* `Lenient`<br/>  

## ➡️ 4.2: REMOVING DUPLICATES

### 🛠️ Tool: ``Picard's MarkDuplicates``<br/>
### 🎯 Parameters:<br/>
*Select SAM/BAM dataset or dataset collection:* ``The merged BAM dataset produced by MergeSAMFiles tool``<br/>
*The scoring strategy for choosing the non-duplicate among candidates:* ``SUM_OF_BASE_QUALITIES``<br/>
*The maximum offset between two duplicate clusters in order to consider them optical duplicates:* ``100``<br/>
*Select validation stringency:* ``Lenient``<br/>

## ➡️ 4.3: LEFT-ALIGNING INDELS

### ⚠️ Required step before executing Step 4.3:<br/>
Click on the ``Pencil`` icon of the BAM dataset produced in **Step 4.2** to edit ``attributes`` <br/>
Select `hg38` under the `Database/Build` option and save.

### 🛠️ Tool: ``BamLeftAlign``<br/>
### 🎯 Parameters:<br/>
*Choose the source for the reference genome:* ``Locally cached``<br/>
*Select alignment file in BAM format:* ``The BAM dataset produced by MarkDuplicates tool``<br/>
*Using reference genome:* ``hg38``<br/>
*Maximum number of iterations:* ``5``<br/>

## ➡️ 4.4: FILTERING READS

### 🛠️ Tool: ``BAMTools Filter``<br/>
### 🎯 Parameters:<br/>
*BAM dataset(s) to filter:* ``Select the BAM dataset produced by BamLeftAlign tool``<br/>
<br/>
Under ``Condition`` > ``1: Condition`` > ``Filter``:<br/>
**In ``1: Filter``:**<br/>
*Select BAM property to filter on:* ``mapQuality``<br/>
*Filter on read mapping quality (phred scale):* ``>=20``<br/>

Click on ``Insert Filter``<br/>

**In ``2: Filter``:**<br/>
*Select BAM property to filter on:* ``isPaired``<br/>
*Selected mapped reads:* ``Yes``<br/>

Click on ``Insert Filter``<br/>

**In ``3: Filter``:**<br/>
*Select BAM property to filter on:* ``isProperPair``<br/>
*Select reads with mapped mate:* ``Yes``<br/>

Click on ``Insert Filter``<br/>

**In ``4: Filter``:**<br/>
*Select BAM property to filter on:* ``reference``<br/>
*Select reads with mapped mate:* ``chrM``<br/>

# 5️⃣ STEP 5 - CALLING NON-DIPLOID VARIANTS

### 🛠️ Tool: ``FreeBayes``<br/>
### 🎯 Parameters:<br/>
*Choose the source for the reference genome:* ``Locally cached``<br/>
*Run in batch mode?:* ``Run individually``<br/>
*BAM dataset:* ``select the BAM dataset produced by last Filter tool step``<br/>
*Using reference genome:* ``hg38``<br/>
*Limit variant calling to a set of regions?:* ``Limit to region``<br/>
*Region Chromosome:* ``chrM``<br/>
*Region Start:* ``1``<br/>
*Region End:* ``16000``<br/>

*Choose parameter selection level:* ``5: Full list of options``<br/>
*Population model options:* ``Set population model options``<br/>
*The expected mutation rate or pairwise nucleotide diversity among the population under analysis:* ``0.001``<br/>
*Set ploidy for the analysis:* ``1``<br/>
*Assume that samples result from pooled sequencing:* ``Yes``<br/>
*Output all alleles which pass input filters, regardless of genotyping outcome or model:* ``Yes``<br/>

*Allelic scope options:* ``Set allelic scope options``<br/>
*Ignore SNP alleles:* ``No``<br/>
*Ignore indels alleles:* ``No``<br/>
*Ignore multi-nucleotide polymorphisms, MNPs:* ``Yes``<br/>
*Ignore complex events (composites of other classes):* ``Yes``<br/>

*Input filters:* ``Set input filters``<br/>
*Exclude alignments from analysis if they have a mapping quality less than:* ``20``<br/>
*Exclude alleles from analysis if their supporting base quality less than:* ``30``<br/>  

# 6️⃣ STEP 6 - FILTERING VARIANTS

### 🛠️ Tool: ``VCFfilter``<br/>
### 🎯 Parameters:<br/>

*VCF dataset to filter*: ``select the VCF dataset produced by FreeBayes tool``<br/>

In ``more filters``:<br/>
**In ``1: more filters``:**<br/>
*Select the filter type*:* ``Info filter (-f)``<br/>
*Specify filtering value*: ``SRP > 20``<br/>

Click on ``Insert more filters``<br/>

**In ``2: more filters``:**<br/>
*Select the filter type*: ``Info filter (-f)``<br/>
*Specify filtering value*: ``SAP > 20``<br/>

Click on ``Insert more filters``<br/>

**In ``3: more filters``:**<br/>
*Select the filter type*: ``Info filter (-f)``<br/>
*Specify filtering value*: ``EPP > 20``<br/>

Click on ``Insert more filters``<br/>

**In ``4: more filters``:**<br/>
*Select the filter type*: ``Info filter (-f)``<br/>
*Specify filtering value*: ``QUAL > 20``<br/>

Click on ``Insert more filters``<br/>

**In ``5: more filters``:**<br/>
*Select the filter type*: ``Info filter (-f)``<br/>
*Specify filtering value*: ``DP > 20``<br/>

# 7️⃣ STEP 7 - VISUALIZATION

## ➡️ 7.1 Visualising with VCF.IOBIO

### 🛠️ Tool: ``VCF.IOBIO``<br/>
### 🎯 Result:<br/>
![vcfiobioimage](https://raw.githubusercontent.com/HackBio-Genomics-One-B/Genomics-One-B/mike/7_Visualize/Visualising%20with%20VCF%20IOBIO.JPG)

## ➡️ 7.2: Visualising with IGV

### 🛠️ Tool: ``IGV``<br/>
### 🎯 Result:<br/>
![igvimage](https://raw.githubusercontent.com/HackBio-Genomics-One-B/Genomics-One-B/mike/7_Visualize/Visualization%20using%20IGV.jpeg)

# 8️⃣ STEP 8 - COMPARING FREQUENCIES

## ➡️ 8.1 Convert VCF to tab-delimited data

### 🛠️ Tool: ``VCFtoTab-delimited``<br/>
### 🎯 Parameters:<br/>
*Select VCF dataset to convert*: ``select the VCF dataset produced by VCFfilter tool``<br/>
*Report data per sample*: ``Yes``<br/>
*Fill empty fields with*: ``Nothing``<br/>

## ➡️ 8.2: Cut columns from a file

### 🛠️ Tool: ``Cut columns from a table``<br/>
### 🎯 Parameters:<br/>
*Cut columns*: ``c2,c4,c5,c52,c54,c55``<br/>
*Delimited by*: ``Tab``<br/>
*From*: ``select the tabular dataset produced by VCFtoTab-delimited``<br/>
<br/>
<br/>
# 🏁 RESULTS

Access our data and results on the drive here: <a href="https://drive.google.com/drive/folders/1oafSVbpr8Nz0YXFxoqBtIIDefZ8NcWbd" target="blank"><img align="center" src="https://user-images.githubusercontent.com/77963733/130207235-ab81908f-62cd-45c7-9e0c-408831a6f164.png" height="50" width="50" /></a> </p>

# ❤️ CONTRIBUTORS
        
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
| `omimill` | Comparing of frequencies using VCFtoTab-delimited and Github Markdown |  



