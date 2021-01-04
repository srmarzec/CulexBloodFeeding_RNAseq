# Workflow Ideas
This document contains notes and ideas about how to approached the RNAseq analysis for the Culex blood feeding project.

## Tuxedo Suite (TopHat and Cufflinks)
Based on [Trapnell et al. 2012](https://www.nature.com/articles/nprot.2012.016).

Requirements:
  - Reference genome -> Culex pipiens reference needed (Cx. quinquefasciatus has bad review)
  - Illumina or SOLiD sequencing

### Workflow
**TopHat** (or [TopHat2](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-4-r36#:~:text=TopHat2%20combines%20the%20ability%20to,.edu%2Fsoftware%2Ftophat.)) maps reads that are "initially unmapable" by Bowtie (those read segments that map far apart [b/w 100bp and >several hundred kb] and thus are inferred to span a slice site).

**Cufflinks** assembles individual transcripts from mapped reads

  - Does not deal well with pooled data, because computationally expensive and becomes too complex with potentially different splice isoforms. _Cuffmerge_, a "meta-assembler", can be used on individual samples after they are run though _Cufflinks_ and this will merge the resulting assemblies together.
  
  - _Cuffcompare_ will compare the assemblies to a reference annotation file to ep identify new genes
  
  - _Cuffdiff_ calculates expression in two or more samples and test for stat. significance in expression. _Cufflinks_ should be able to automatically model and substract a large amount of the bias due to library prep.
  
  - Output can be easily veiwed and illustrated with _CummeRbund_

## Alternatives
[*STAR*](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3530905/) as a mapper is very fast (but requires a lot of memory). Also maps reads independently of the alignments of other reads, which may mean lower sensitivity for spliced reads

[*BISR-RNAseq*](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-3251-1) is a potential alternative workflow to be explored. Ends in a way to visualize output data in Shiny web app.

## Studies
### [Kang et al 2016](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0154892)
Topic: Culex pipiens expression differences for diapausing and non-diapausing adults

Used: 
  - Cx. quinquefasciatus as reference genome but only ~55% of reads mapped (using TopHat)
  - Cufflinks to assemble transcripts and estimate FPKM
  - Cuffdiff for significant differences
  - GO analysis using [DAVID](https://david.ncifcrf.gov/) and the GO and KEG databases
  
### [Lv et al. 2016](https://link.springer.com/article/10.1007/s00438-015-1109-4)
Topic: analyses of deltamethrin-susceptible and -resistant Culex pipiens pallens by RNA-seq

Used:
  - SOAP as mapper to Cx. quinquefasciatus reference
  - Cufflinks 
  - RPKM

### [Honnen et al. 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4700752/)
Topic: Sex-specific gene expression in the mosquito Culex pipiens f. molestus in response to artificial light at night

Used:
  - Cx. quinquefasciatus reference (using RSEM and Bowtie)
  - DESeq2 in R
  - GO analysis using GOstats in R
  
## Other
[RNA-seq best practices](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8)

Would it be helpful to make our our transcriptome assembly guided based on Cx. quinquefasciatus? [Ungaro et al 2017](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0185020)
