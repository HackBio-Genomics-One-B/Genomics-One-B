# [HackBio internship 2021](https://thehackbio.com/): Genomics-One-B

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)
![](https://files.slack.com/files-pri/T025KDN24L8-F02BXHY1JMP/1629208803397.jpg)

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


## Introduction


In this tutorial, we will cover:

1. Importing data
2. Checking data quality
3. Mapping reads to a reference
4. Postprocessing mapped reads
      - Merging BAM datasets
      - Removing duplicates
      - Left-aligning indels
      - Filtering reads
5. Calling non-diploid variants
6. Filtering variants
7. Examining the results
      - Visualising with VCF.IOBIO
      - Visualising with IGV
8.Comparing frequencies
