# Using Annovar for variant annotation

## Download Annovar

1. Download Annovar from the web site (http://annovar.openbioinformatics.org/en/latest/)
2. Unzip the file using `tar xvfz annovar.latest.tar.gz`
3. Add the program to your $PATH 
4. Create an executable version usin `chmod 777 annotate_variants.pl`
5. Run it using `./annotate_variants.pl`

## Download necessary databases

1. Download the most updated databases at the moment 

```
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar refGeneWithVer humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar knownGene humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar ensGene humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar dbnsfp35a humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar dbscsnv11 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar esp6500siv2_ea humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar esp6500siv2_aa humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar esp6500siv2_all humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar exac03 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gnomad211_exome humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gnomad211_genome humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar kaviar_20150923 humandb/ 
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar abraom humandb/ 
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar 1000g2015aug humandb/ 
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar avsnp150 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar clinvar_20190305 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar cadd13gt20 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar popfreq_max_20150413 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar regsnpintron humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gerp++gt2 humandb/

```
2. Add the humandb/ directory to the $PATH 

## Prepare the input file and run Annovar

Make sure you have the VCF files you would like to annotate in a separate directory 

Perform the annotation

* Gene-based
This annotation identifies which are the disrupted genes, the distance to the genes for intronic variants and for exonic variants the aminoacid change
There are multiple gene definitions: refGene, ensGene (Ensembl) and knownGene (UCSC) are some of them
* Filter-based

* Region-based

## Use commandline to annotate

sudo table_annovar.pl Batch1_combine.snpindel.vcf /usr/local/bin/humandb/ -buildver hg19 -out Batch1_combine.snpindel_ANN.vcf -remove -protocol refGeneWithVer,knownGene,ensGene,dbnsfp35a,dbscsnv11,exac03,gnomad211_exome,gnomad211_genome,kaviar_20150923,abraom,gme,avsnp150,clinvar_20190305,cadd13gt20,popfreq_max_20150413,regsnpintron,gerp++gt2 -operation gx,g,g,f,f,f,f,f,f,f,f,f,f,f,f,f,f  -argument '-splicing 12 -exonicsplicing','-splicing 12 -exonicsplicing','-splicing 40',,,,,,,,,,,,,, -nastring .  -xreffile LoFtool_scores.txt -intronhgvs 40 -vcfinput


### filter based on splice exon
awk 'FNR == 1 {print}/splic|exonic/{print}' 
Batch1_combine.snpindel_ANN.vcf.hg19_multianno.txt > Batch1_combine.snpindel_ANN.vcf.hg19_multianno_splice_exon.txt

### filter pathogenic
awk 'FNR == 1 {print}/pathogenic|Pathogenic/{print}'  FIN11.raw.snpindel.vcf.gz.hg19_multianno.txt> FIN11.raw.snpindel.vcf.gz.hg19_multianno_splice_exon.txt

### filter more based on frequency

annotate_variation.pl -filter -dbtype popfreq_max_20150413 -build hg19 -score_threshold 0.005 -out Batch1_combine.snpindel.hg19_multianno_splice_exon_popmax Batch1_combine.snpindel.hg19_multianno_splice_exon.txt /usr/local/bin/humandb/

**The filtering option of annovar accepts .avinput and .txt formats as input**

  
