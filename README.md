# Genomics-One-B
Step-by-Step 

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

# [HackBio internship 2021](https://thehackbio.com/):  Genomics-One-B
![hackbio ads](https://pbs.twimg.com/media/E5k_rKIWEAcaG_-.jpg)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience
# Importing Data
Import raw reads from [here](https://zenodo.org/record/1251112)
# Step 1: Quality Checking  
Perform quality control of the raw reads using [FASTQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)  

# Step 2: Map to reference  
This step aligns the reads to the reference genome  
**Tool** [BWA-mem](http://bio-bwa.sourceforge.net/bwa.shtml)  
Use the in-built genome index of the human hg38 genome. 
Mark the reads as paired and set read groups in (SAM/BAM) specification.  

# Step 3:   

# Step 4: Postprocessing mapped reads

## 4.1: Merging BAM datasets

**Tool:** Picard's MergeSAMFiles<br/>
**Purpose:** To merge the BAM datasets received from **Step 3**<br/>
<br/>
**Parameters:**<br/>
**“Select SAM/BAM dataset or dataset collection”:** *Both BAM datasets produced by BWA-MEM tool*<br/>
**“Select validation stringency”:** *Lenient*<br/>
