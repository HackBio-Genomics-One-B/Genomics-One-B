# Genomics-One-B
Step-by-Step 

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

# [HackBio internship 2021](https://thehackbio.com/):  Genomics-One-B
![hackbio ads](https://pbs.twimg.com/media/E5k_rKIWEAcaG_-.jpg)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience


# Step 4: Postprocessing mapped reads

## 4.1: Merging BAM datasets

**Tool:** Picard's MergeSAMFiles<br/>
**Purpose:** To merge the BAM datasets received from **Step 3**<br/>
<br/>
**Parameters:**<br/>
**“Select SAM/BAM dataset or dataset collection”:** *Both BAM datasets produced by BWA-MEM tool*<br/>
**“Select validation stringency”:** *Lenient*<br/>
