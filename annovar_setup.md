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
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gme humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar avsnp150 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar clinvar_20190305 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar cadd13gt20 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar popfreq_max_20150413 humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar regsnpintron humandb/
  $ annotate_variation.pl -buildver hg19 -downdb -webfrom annovar gerp++gt2 humandb/

  
  
