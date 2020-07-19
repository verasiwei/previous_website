---
title: How to do Annotation
date: 2018-10-30 10:39:13
tags:
categories:
- Bioinformatics 
---

Use ANNOVAR to annotate the SNPs. 


1. Prepare the ANNOVAR input file: vcf format or .avinput format
Notice: In vcf file, the reference allele column and alternative allele column is actually the major allele and minor allele, but major allele are not always the reference allele. So use R package to figure out the correct reference allele in hg19 at first.

Example: 

```
#Do the annotation
source("https://bioconductor.org/biocLite.R")
#biocLite("GenomicRanges")
biocLite("BSgenome")
biocLite("BSgenome.Hsapiens.UCSC.hg19")
library(BSgenome.Hsapiens.UCSC.hg19)
#BSgenome.Hsapiens.UCSC.hg19
#overview
genome <- BSgenome.Hsapiens.UCSC.hg19
seqlengths(genome)
seqnames(genome)
Hsapiens$chr2

#read in significant snp information file
#library(pegas)
#vcf=read.vcf("sigsnpsvcf.vcf")
snpinfo=read.table("sigsnps.bim",header = FALSE,sep="\t")
colnames(snpinfo)=c("CHR","SNP","Unknown","BP","Minor","Major")

#to get the reference allele
chrs = seqnames(Hsapiens)[1:21]
sigsnpref=c()
k=1
for (i in 1:21) {
  for (j in k:(k+nrow(snpinfo[which(snpinfo$CHR==i),])-1) ) {
    sigsnpref = c(sigsnpref,as.character(getSeq(Hsapiens, names=chrs[i], start=snpinfo[j,"BP"], end=snpinfo[j,"BP"])))
  }
  k=k+nrow(snpinfo[which(snpinfo$CHR==i),])
}
sigsnpref.dat=data.frame(sigsnpref)
snpinfo$ref=sigsnpref.dat$sigsnpref
snpinfo$Minor=as.character(snpinfo$Minor)
snpinfo$Major=as.character(snpinfo$Major)
snpinfo$ref=as.character(snpinfo$ref)

#check unmatch
snpinfo[which((snpinfo$Major)!=(snpinfo$ref)),"Minor"]=snpinfo[which((snpinfo$Major)!=(snpinfo$ref)),"Major"]
snpinfo$Major=snpinfo$ref
snpinfo=snpinfo[,c(1:6)]
write.table(snpinfo,"sigsnps.bim",row.names = FALSE,col.names=FALSE,sep = "\t",quote = FALSE)

#prepare the .avinput file 
snpinfo$SNP=snpinfo$BP
snpinfo$Unknown=snpinfo$BP
snpinfo$Alt=snpinfo$Minor
snpinfo=snpinfo[,c(1,2,3,6,7)]
colnames(snpinfo)=c("CHR","Start","End","Ref","Alt")
write.table(snpinfo,"sigsnps.avinput",row.names = FALSE,col.names=FALSE,sep = "\t",quote = FALSE)

 
 #annotation=read.table("sigannotation5*10^-5.hg19_multianno.txt",header = TRUE,sep = "\t",
 #                      c("Chr","Start","End","Ref","Alt","Func.refGene","Gene.refGene","GeneDetail.refGene","ExonicFunc.refGene","AAChange.refGene"))
 annotation=read.csv("sigannotation2.hg19_multianno.csv",header = TRUE)
 annotation$BP=annotation$Start
 colnames(annotation)[1]="CHR"
 sigsnps=read.csv("unilog_4covs_ph.csv",header = TRUE)
 combine_annotation=merge(sigsnps,annotation,by.x=c("CHR","BP"),by.y=c("CHR","BP"),sort = FALSE)
 write.table(combine_annotation,"result_table.csv",sep = ",",row.names = FALSE,col.names = TRUE,quote = FALSE)
 
```

use plink `plink --bfile filename --recode vcf --out filename` to convert to vcf format, then download the database

```
annotate_variation.pl -buildver hg19 -downdb -webfrom annovar refGene humandb/

annotate_variation.pl -buildver hg19 -downdb cytoBand humandb/

annotate_variation.pl -buildver hg19 -downdb -webfrom annovar exac03 humandb/ 

annotate_variation.pl -buildver hg19 -downdb -webfrom annovar avsnp147 humandb/ 

annotate_variation.pl -buildver hg19 -downdb -webfrom annovar dbnsfp30a humandb/

```

Then using table_annovar.pl to annotate in one step:

```
table_annovar.pl vcffile --buildver hg19 -out outfilename -remove -protocol refGene,cytoBand,exac03,avsnp147,dbnsfp30a -operation g,r,f,f,f -nastring . -vcfinput

table_annovar.pl .avinputfile -buildver hg19 -out outfilename -remove -protocol refGene,cytoBand,exac03,avsnp147,dbnsfp30a -operation g,r,f,f,f -nastring . -csvout -polish -xref gene_reffile

```
If using annotate_variation.pl to apply only gene based annotation

```
perl annotate_variation.pl --geneanno -dbtype refGene -out outfilename -build hg19 avinputfile directory_refgene

```












