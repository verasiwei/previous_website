---
title: deign_analysis_interpretation_GWAS
date: 2018-11-19 12:46:59
tags:
categories:
- Bioinformatics
---
Notes for the book: Design, Analysis, and Interpretation of Genome-Wide Association Scans

## Introduction:

### Charles Darwin's work 
Led most directly to the field of genetics, which is the theory of evolution: 1) More individuals are produced each generation that survive. 2) Phenotypic variation exists among individuals and the variation is heritable. 3) Those individuals with heriatble traits better suited to the environment will survive. 4) When reproductive isolation occurs new species will form [Wiki]

### Fundamental to the emergence of modern genetics was the discovery by Gregor Mendel: 
1) Law of Segregation: When sperm and egg unite at fertilization, each contributes its allele, restoring the paired condition in the offspring. 2) Law of Independent Assortment: Each pair of alleles segregates independently of the other pairs of alleles during gamete formation. 3) Law of Dominance: If the two alleles of an inherited pair differ, then one determines the organism's appearance and is called the dominant allele. [Wiki]

### Organization of Chromosomes:
22 autosomes and 2 sex chromosomes. The Centromere (which I circled in the example picture[Wiki]) divides the chromosome into two arms, the shorter arm is the p arm and the longer is the q arm. Example: 1q22 is the 2th band of the 2nd region of the long arm of chromosome 1.

![](/images/chr1.jpg)

### Organization of DNA:
Each chromosome is made up of two strands of DNA, each strand has a deoxyribose backbone, each sugar having a 3' carbon linked by a phosphate group to the 5' carbon. The strand issue is the problem when compare SNP data from different genotyping platforms. If one platform uses probes for one strand and another platform uses the reverse probse, it will be read differently. So define "plus" or "forward" strand for a given chromosome as the strand of DNA for which the 5' end is the nearest to the centromere, another definition is moving from the 5' to the 3' end moves in increasing base position order. The "minor" or "reverse" strand is the complementary strand to plus strand. Example in the picture:

![](/images/strand.jpg)

### Types of Genetic Variation

1) An SNV consists of a single base variation at a specific position on a given strand of DNA with the change measured relative to some existing consensus sequence. If an SNV presents at least 1%, it will be denoted as SNP. 
2) Insertions/Deletions
3) Larger Structured Variants: including deletions, insertions, and duplications of segments of DNA that are 1kb or greater in extent are termed copy number variants or CNVs. Other types of structural variants include inversions and translocations. 
4) Exonic Variation: Mendelian traits are most often due to protein changes in specific genes. 
5) Non-exonic SNPs and Disease
6) SNP Haplotypes: a combination of from several to many SNPs on the same chromosome segment of DNA.

### Basic Genomics:
dbSNP: SNPs and other variants are reported.
UCSC Genome Browser: tools and datasets compiling sequence information for many genome. The blat tool maps sequences to genome location and strand is also available?
1000 Genomes Project website: Results of next generation sequencing of 1092 individuals from a variety of racial ethnic groups of similar diversity as HapMap phase 3. I use this datasets as a reference panel for imputation in GWAS study.

### My understanding 
Human is diploid, 23 pairs of chromosomes, if both alleles of a human are the same, he is homozygous at that locus, otherwise it is heterozygous due to the existence of SNP. Assume on one loci of a chromosome, the base is T, normally if there is no polymorphism, it should also be T on the alleles because one from mother, one from father, if there is no SNP in the population, they should be same, but when there exists SNP, the genotype might be TC,TA,TG,CC,AA,GG.

### Quality Control
1. Sample Level
From my own experience, it is very important to control the sample level, otherwise after you completing the whole GWAS, you will notice from the manhattan plot, some dots(variants) will suddenly jump to very abnormal level. 
1) IBD(Identity by Descent):










 



