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

# 1️⃣ Step 1: Importing Data
Import raw reads from [here](https://zenodo.org/record/1251112)

```
https://zenodo.org/record/1251112/files/raw_child-ds-1.fq
https://zenodo.org/record/1251112/files/raw_child-ds-2.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-1.fq
https://zenodo.org/record/1251112/files/raw_mother-ds-2.fq
```

# 2️⃣ Step 2: Quality Checking  

### 🛠️ Tool: ``FastQC``<br/>
### 🎯 Parameters: <br/>
*Short read data from your current history:* `all 4 FASTQ datasets selected with Multiple datasets`<br/>
### 💡 Tips: <br/>
To select multiple datasets, <br/>
click on the Multiple datasets icon<br/>
select several files by keeping the ``Ctrl`` (or ``COMMAND``) key pressed and clicking on all 4 FASTQ files

# 3️⃣ Step 3: Mapping reads to reference  

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

# 4️⃣ Step 4: Postprocessing mapped reads

## ➡️ 4.1: Merging BAM datasets

### 🛠️ Tool: ``Picard's MergeSAMFiles``<br/>
### 🎯 Parameters:<br/>
*Select SAM/BAM dataset or dataset collection:* `Both BAM datasets produced by BWA-MEM tool`<br/>
*Select validation stringency:* `Lenient`<br/>  

## ➡️ Step 4.2: Removing duplicates

### 🛠️ Tool: ``Picard's MarkDuplicates``<br/>
### 🎯 Parameters:<br/>
*Select SAM/BAM dataset or dataset collection:* ``The merged BAM dataset produced by MergeSAMFiles tool``<br/>
*The scoring strategy for choosing the non-duplicate among candidates:* ``SUM_OF_BASE_QUALITIES``<br/>
*The maximum offset between two duplicate clusters in order to consider them optical duplicates:* ``100``<br/>
*Select validation stringency:* ``Lenient``<br/>

## ➡️ Step 4.3: Left-aligning indels

### ⚠️ Required step before executing Step 4.3:<br/>
Click on the ``Pencil`` icon of the BAM dataset produced in **Step 4.2** to edit ``attributes`` <br/>
Select `hg38` under the `Database/Build` option and save.

### 🛠️ Tool: ``BamLeftAlign``<br/>
### 🎯 Parameters:<br/>
*Choose the source for the reference genome:* ``Locally cached``<br/>
*Select alignment file in BAM format:* ``The BAM dataset produced by MarkDuplicates tool``<br/>
*Using reference genome:* ``hg38``<br/>
*Maximum number of iterations:* ``5``<br/>

## ➡️ Step 4.4: Filtering reads

### 🛠️ Tool: ``BAMTools Filter``<br/>
### 🎯 Parameters:<br/>
*BAM dataset(s) to filter:* ``Select the BAM dataset produced by BamLeftAlign tool``<br/>
<br/>
Under **Condition** > **1: Condition** > **Filter:**<br/>
**In “1: Filter”:**<br/>
*Select BAM property to filter on:* ``mapQuality``<br/>
*Filter on read mapping quality (phred scale):* ``>=20``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “2: Filter”:**<br/>
*Select BAM property to filter on:* ``isPaired``<br/>
*Selected mapped reads:* ``Yes``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “3: Filter”:**<br/>
*Select BAM property to filter on:* ``isProperPair``<br/>
*Select reads with mapped mate:* ``Yes``<br/>
Click on ``“Insert Filter”``<br/>
<br/>
**In “4: Filter”:**<br/>
*Select BAM property to filter on:* ``reference``<br/>
*Select reads with mapped mate:* ``chrM``<br/>


# 5️⃣ Step 5: Calling non-diploid variants

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

