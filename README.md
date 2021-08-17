# [HackBio internship 2021](https://thehackbio.com/): Genomics-One-B
Step-by-Step 

![hackbio image](https://media-exp1.licdn.com/dms/image/C561BAQHKcVQGbcedOA/company-background_10000/0/1598491473588?e=2159024400&v=beta&t=rxECjvQ_YSc28Dn0n9YOtDoFFmvXjatRiqc__C2mpU0)

[HackBio](https://thehackbio.com/) is a virtually regimented research internship that is practice oriented and focused on equipping African scientists with advanced bioinformatics and computational biology skills. By the end of internship, successful interns should have:
- Honed their skills in a specific bioinformatics method
- Have at least a peer-reviewed article to show for the internship experience

#  WORKFLOW
![hackbio ads](https://files.slack.com/file-pri/T025KDN24L8-F02BU09HMJM/project_design__genomics_1b_.png)

![](https://files.slack.com/files-pri/T025KDN24L8-F02BU09HMJM/project_design__genomics_1b_.png0)






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
