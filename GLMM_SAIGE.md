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
 
markerIndex	p.value	p.value.NA	var1	var2	Tv1	N	AC	AF
1314	0.544534701	0.587198464	0.535048354	0.666584321	-0.443248845	90	60	0.333333333
2964	0.736648221	0.76319071	0.459094047	0.571957407	-0.22786183	  90	67	0.372222222
1626	0.168242468	0.217030306	0.495915377	0.617830927	-0.970315597	90	49	0.272222222
2727	0.826541959	0.844351348	0.13372574	0.166600791	-0.080135824	90	75	0.416666667
1067	0.617538118	0.65460747	0.446474744	0.556235884	-0.333654496	90	56	0.311111111
1100	0.702216589	0.731946866	0.779194377	0.970750988	  0.337490309	90	20	0.111111111
2042	0.980333973	0.982380471	0.461925078	0.575484431	-0.016753529	90	41	0.227777778
155	  0.405193026	0.455819838	0.871477634	1.085721344	  0.77705384	90	36	0.2
3396	0.176368444	0.225779532	0.770215873	0.959565217	-1.186560328	90	25	0.138888889
1196	0.956666588	0.961172985	0.767357429	0.956004141	  0.047598727	90	21	0.116666667
1921	0.940505417	0.946687886	0.373202211	0.464950012	-0.045594505	90	68	0.377777778
179	  0.318890583	0.371857699	0.7791943	  0.970750988	-0.879842429	90	20	0.111111111
3131	0.101456635	0.1422765	  0.503586613	0.627388011	  1.162268799	90	48	0.266666667
649	  0.795755084	0.816612581	0.580985238	0.723814229	 -0.197297594	90	54	0.3
2241	0.337239738	0.389924495	0.22551065	0.280950028	  0.455710355	90	84	0.466666667
129	  0.253340201	0.306125691	0.313072937	0.390038637	-0.639135593	90	89	0.494444444
2130	0.976935498	0.979335495	0.384401517	0.47890252	  0.017924913	90	77	0.427777778
2273	0.522077331	0.566292832	0.490832198	0.611498089	  0.448482941	90	61	0.338888889
3245	0.722588495	0.750444939	0.289302881	0.360424901	-0.190944222	90	80	0.444444444
2551	0.090535103	0.129413974	0.568426859	0.708168643	  1.276106603	90	33	0.183333333
101	  0.609150111	0.646900731	0.534686653	0.666133669	-0.373864662	90	55	0.305555556
2046	0.472233399	0.519554911	0.405821288	0.505588115	  0.457936898	90	87	0.483333333
721	  0.687218886	0.718304324	0.435365937	0.54239601	-0.265665787	90	84	0.466666667
522 	0.26256583	0.315503519	0.449027435	0.55941604	  0.750739171	90	62	0.344444444
630	  0.072630569	0.107768279	0.386780961	0.481866947	-1.116431614	90	77	0.427777778
2886	0.416597799	0.466742572	0.309716916	0.385857515	-0.452084015	90	82	0.455555556
1368	0.856819781	0.87158518	0.459597898	0.572585119	-0.122315957	90	57	0.316666667
1089	0.127245261	0.171836372	0.201241928	0.250715103	-0.684139832	90	76	0.422222222
2794	0.48600056	0.532514126	0.479978241	0.597975805	  0.482665909	90	66	0.366666667
1318	0.476445716	0.523524307	0.547317885	0.681870189	-0.526767033	90	57	0.316666667
 
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
  * 
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
        
  Note: to view a .gz file use `gzcat CEU.exon.2010_03.genotypes.hg19.vcf.gz | less -S`
