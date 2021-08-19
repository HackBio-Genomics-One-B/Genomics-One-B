# Genomics-One-B
Step-by-Step 

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

# [HackBio internship 2021](https://thehackbio.com/):  Genomics-One-B
![hackbio ads](https://pbs.twimg.com/media/E5k_rKIWEAcaG_-.jpg)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience

# Instructions  
This tutorial is implemented in galaxy  
Always use the serch button to navigate the respective tools  

# Step 1: Importing Data
Import raw reads from [here](https://zenodo.org/record/1251112)

```
https://zenodo.org/record/1251112/files/raw_child-ds-1.fq
https://zenodo.org/record/1251112/files/raw_child-ds-2.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-1.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-2.fq
```

# Step 2: Quality Checking  
Perform quality control of the raw reads using [FASTQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)  
Apply **FastQC** tool on all the datasets to check data quality.  

# Step 3: Map reads to reference  
This step aligns the reads from **Step 1** to the reference genome **hg38**    
**Tool:** ``BWA-mem``
<br/>
**Parameters:**<br/>
*Select first set of reads:* `select both -1 datasets selected with Multiple datasets` <br/>
*Select second set of read:** `select both -2 datasets selected with Multiple datasets`<br/>
*Set read groups information?:* `Set read groups (SAM/BAM specification)`<br/>

# Step 4: Postprocessing mapped reads

## 4.1: Merging BAM datasets

**Tool:** ``Picard's MergeSAMFiles``<br/>
**Purpose:** To merge the BAM datasets received from **Step 3**<br/>
<br/>
**Parameters:**<br/>
*“Select SAM/BAM dataset or dataset collection”:* `Both BAM datasets produced by BWA-MEM tool`<br/>
*“Select validation stringency”:* `Lenient`<br/>  


## Step 4.2: Removing duplicates

**Tool:** ``Picard's MarkDuplicates``<br/>
**Purpose:** To de-duplicate the merged BAM from **Step 4.1**<br/>
<br/>
**Parameters:**<br/>
*“Select SAM/BAM dataset or dataset collection”:* ``The merged BAM dataset produced by MergeSAMFiles tool``<br/>
*“The scoring strategy for choosing the non-duplicate among candidates”:* ``SUM_OF_BASE_QUALITIES``<br/>
*“The maximum offset between two duplicate clusters in order to consider them optical duplicates”:* ``100``<br/>
*“Select validation stringency”:* ``Lenient``<br/>

## Step 4.3: Left-aligning indels

#### **Required step before executing this step:**<br/>
Click on the ``Pencil`` icon of the BAM dataset produced by MarkDuplicates tool to edit ``attributes`` <br/>
Select `hg38` under the `Database/Build` option and save

**Tool:** ``BamLeftAlign``<br/>
**Purpose:** To perform left alignment on the de-duplicated BAM from **Step 4.2**<br/>
<br/>
**Parameters:**<br/>
*"Choose the source for the reference genome”:* ``Locally cached``<br/>
*“Select alignment file in BAM format”:* ``The BAM dataset produced by MarkDuplicates tool``<br/>
*“Using reference genome”:* ``hg38``<br/>
*“Maximum number of iterations’:* ``5``<br/>


## Step 4.4: Filtering reads

**Tool:** BAMTools Filter<br/>
**Purpose:** To filter reads from the left-aligned BAM from Step 4.3<br/>
<br/>
**Parameters:**<br/>
*“BAM dataset(s) to filter”:* ``Select the BAM dataset produced by BamLeftAlign tool``<br/>
<br/>
*Under “Condition” > “1: Condition” > “Filter”:*<br/>
**In “1: Filter”:**<br/>
*“Select BAM property to filter on”:* ``mapQuality``<br/>
*“Filter on read mapping quality (phred scale)”:* ``>=20``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “2: Filter”:**<br/>
*“Select BAM property to filter on”:* ``isPaired``<br/>
*“Selected mapped reads”:* ``Yes``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “3: Filter”:**<br/>
*“Select BAM property to filter on”:* ``isProperPair``<br/>
*“Select reads with mapped mate”:* ``Yes``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “4: Filter”:**<br/>
*“Select BAM property to filter on”:* ``reference``<br/>
*“Select reads with mapped mate”:* ``chrM``<br/>


# Step 5: Calling non-diploid variants  
**Tool:** ``FreeBayes``  




