# [HackBio internship 2021](https://thehackbio.com/): Genomics-One-B
Step-by-Step 

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience

#  WORKFLOW
![hackbio ads](https://github.com/HackBio-Genomics-One-B/Genomics-One-B/blob/main/PROJECT%20DESIGN%20(GENOMICS%201B).png?raw=true)






1. Importing data (From Galaxy Library)
2. Checking data quality using FastQC
3. Mapping reads to a reference using BWA-MEM
4. Post-processing mapped reads:
      - Merge datasets using SAMFiles
      - Remove duplicates using MarkDuplicates
      - Left aligning indels using BAM left align
      - Filtering reads 
5. Calling non-diploid variants using FreeBayes
6. Filtering variants using VCFfilter
7. Visualization using IGV
8. Compare frequencies



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

