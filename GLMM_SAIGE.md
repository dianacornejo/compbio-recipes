## Usage of SAIGE to analyze binary phenotypes

## 1. Install SAIGE

The most convenient way of running the software is through a docker image

a. Pull the docker image
```
docker pull wzhou88/saige:0.36.2
docker run -it wzhou88/saige:0.36.2
```
The docker image is at https://github.com/weizhouUMICH/Docker/tree/master/SAIGE

Note: if you are compiling from the source code, take into account that the program is built for a Linux x86 machine

Try calling these functions to see if the program works

```
step1_fitNULLGLMM.R --help
step2_SPAtests.R --help
createSparseGRM.R --help
```

b. Run the docker image and share folders (container-data) with the host machine to upload input data

* `mkdir container-data` (make sure the host has read-write permission to this folder)
* `docker run --name test -dit -P -v  ~/container-data:/home wzhou88/saige:0.36.2` (*--name* followed by the name of the new container, -dit: *d* is detached mode, *it* ensures that bash or sh can be allocated to a pseudoterminal, *-P*: publishes the containers ports to the host, *-v* says that what follows is to be the volume, *wzhou88/saige:0.36.2* is the image to be used for the container). After using this command you will be given the container id, make sure you remember the first 4 characters.
* You can also use `docker ps -a` to see a list of all running containers and get the container ID
* Access the newly deployed container using `docker attach ID` (ID is the four first characters of the container ID. You should now find yourself at the bash prompt within the container. You can also use this command to start the container `docker start -i ID`

## 2. Prepare input data

a. Move data to be analyzed to ~/container-data folder which is shared between host and docker image.
b.**Plink files**: genotype file binary format (.bed, .bim, .fam) for constructing the Genetic Relationship Matrix (GRM)
  **Phenotype File**: contains the non-genetic covariantes (e.g. sex). File can be space or tab delimited with header (IID: individual ID, x1: covariate 1 (sex), x2: covariate 2 (race), y_binary: binary trait (obesity), y_quantitative: quantitative trait (BMI). Current version of SAIGE does not support categorical variables with more that 2 categories. 

## 3. Run step 1: fitting the null logistic/linear mixed model 

### For Binary traits

```
Rscript step1_fitNULLGLMM.R     \
        --plinkFile=/home/SAIGE_data/CEU_genotypes_hg19 \
        --phenoFile=/home/SAIGE_data/pheno_CEU_hg19_BMI_Obesity.txt \
        --phenoCol=y_binary \
        --covarColList=x1 \
        --sampleIDColinphenoFile=IID \
        --traitType=binary        \
        --outputPrefix=/home/SAIGE_data/example_binary_CEU \
        --nThreads=4 \
        --LOCO=FALSE
 ```
 
 ### For quantitative traits
 
```
Rscript step1_fitNULLGLMM.R     \
        --plinkFile=/home/SAIGE_data/CEU_genotypes_hg19 \
        --phenoFile=/home/SAIGE_data/pheno_CEU_hg19_BMI_Obesity.txt \
        --phenoCol=y_quantitative \
        --covarColList=x1 \
        --sampleIDColinphenoFile=IID \
        --traitType=quantitative       \
	      --invNormalize=TRUE \
        --outputPrefix=/home/SAIGE_data/example_quantitative \
        --nThreads=4 \
        --LOCO=FALSE \
	      --tauInit=1,0
```

## Output files

a. Model file 
    * Open R
    * load("./output/example_quantitative.rda")
      names(modglmm)
      modglmm$theta
      Result: 1.245839 (dispersion parameter estimate) 0.000000 (variance component parameter estimate)
 b. Association result file for the selected markers
 
 `more example_quantitative_30markers.SAIGE.results.txt`
 
 c. Variance ratio file
 
 `more example_quantitative.varianceRatio.txt`
 
 Result: 0.802671694014824

## 4. Run step 2: single variant association tests
* Saddle point approximation (SPA) to control for case-control inbalance
* You can use different file formats for dosage/genetic variant input: VCF, BGEN, SAV and plain text
* Use `bgzip CEU.exon.2010_03.genotypes.hg19.vcf` to compress the vcf file and then `tabix -p vcf CEU.exon.2010_03.genotypes.hg19.vcf.gz` to create the index file. 

a. Input files 
  * vcf file: genotypes or dosages --> .vcf.gz and .vcf.gz.tbi
  * Sample file: contains one column with the sample IDs corresponding to the order in the dosage file, **no header**
  * Model file from step 3: *example_binary_CEU.rda*
  * Variance ratio file from step 3: *example_binary_CEU.varianceRatio.txt*
  
```
#perform the single-variant association tests
#for binary traits, --IsOutputAFinCaseCtrl=TRUE can be specified to output allele frequencies in cases and controls
Rscript step2_SPAtests.R \
        --vcfFile=/home/SAIGE_data/CEU.exon.2010_03.genotypes.hg19.vcf.gz \
        --vcfFileIndex=/home/SAIGE_data/CEU.exon.2010_03.genotypes.hg19.vcf.gz.tbi \
        --vcfField=GT \
        --chrom=1 \
        --minMAF=0.0001 \
        --minMAC=1 \
        --sampleFile=/home/SAIGE_data/sampleID.txt \
        --GMMATmodelFile=/home/SAIGE_data/example_binary_CEU.rda \
        --varianceRatioFile=/home/SAIGE_data/example_binary_CEU.varianceRatio.txt \
        --SAIGEOutputFile=/home/SAIGE_data/example_binary_CEU.SAIGE.vcf.genotype.txt \
        --numLinesOutput=2 \
        --IsOutputAFinCaseCtrl=TRUE
 ```
        
  Note: to view a .gz file in MacOs use `gzcat CEU.exon.2010_03.genotypes.hg19.vcf.gz | less -S`
