# Install NSP: either clone it from github or download the latest version

```
git clone https://github.com/sgchun/nps nps
cd nps/
git checkout tags/1.0.1
make
```
OR

```
tar -zxvf nps-1.0.1.tar.gz
cd nps-1.0.1/
make
```
1. **Install all the required libraries**

  * R v3.3 or later
  * OpenBLAS (linear algebra acceleration library): brew install openblas
  * Install R modules pROC and DescTools
    Rscript -e 'install.packages("pROC", repos="http://cran.r-project.org")'
    Rscript -e 'install.packages("DescTools", repos="http://cran.r-project.org")' 
    
  
  Note: to activate python2.7 type conda activate py2
  
2.  **Prepare input files**

   1. GWAS summary statistics: MINIMAL or PREFORMATTED (**Test1.summstats.txt**)
   2. Training genotypes in the qctool dosage format the data should be splitted by chromosome and named         "chromN.DatasetTag.dosage.gz." (**chrom1.Test1.train.dosage.gz**)
   3. Training sample IDs in the PLINK .fam format (**Test1.train.2.5K_2.5K.fam**)
   4. Training phenotypes in the PLINK phenotype format (**Test1.train.2.5K_2.5K.phen**)
   5. Validation genotypes in the qctool dosage format (**chrom1.Test1.val.dosage.gz**)
   6. Validation sample IDs in the PLINK .fam format (**Test1.val.5K.fam**)
   7. Validation phenotypes in the PLINK phenotype format (**Test1.val.5K.phen**)
   
3. **Unpack datafiles**
   
   Download and unpack the files in the following directory 
   
   ```
   cd nps-1.0.1/testdata/
   tar -zxvf NPS.Test1.tar.gz 
   ```
4. **Run NPS on test set without parallelization**

* Standardize genotypes to mean 0 and variance 1 using `nps_stdgt.job`. The first parameter (testdata/Test1) is the     location of training cohort data, where NPS will find `chromN.DatasetTag.dosage.gz` files. The second parameter (`Test1.train`) is the DatasetTag of training cohort.

```
cd nps-1.0.1/
./run_all_chroms.sh sge/nps_stdgt.job testdata/Test1 Test1.train
```   
### Note: 
* run_all_chroms.sh is a script to perform the jobs sequentially, by processing one chromosome at a time
* nps_stdgt.job is a script to standardize the genotypes, however, it has an error that prevents it from running. Here the fixed version (zcat to run in MacOS and the M variable placing the wc -l command at the end of the pipe)

```
dir=$1
datasetName=$2
chrom=$SGE_TASK_ID

###
# ADD CODES TO LOAD MODULES HERE
#
# If you loaded a GCC module to compile stdgt, you also need to load 
# the GCC module here.

# Broad institute: 
# source /broad/software/scripts/useuse
# use GCC-5.2
###

prefix="$dir/chrom${chrom}.${datasetName}"

echo $prefix

N=`zcat $prefix.dosage.gz | head -n 1 | tr " " "\n" | tail -n +7 | wc -l`
M=`zcat $prefix.dosage.gz | tail -n +2 | cut -d' ' -f1 | wc -l `

echo "N=$N M=$M"

zcat $prefix.dosage.gz | ./stdgt $N $M $prefix
    
gzip -f $prefix.stdgt

```
* Verify that all jobs were successfully completed. If there is an error `FAIL` message will be printed.

`./nps_check.sh stdgt testdata/Test1 Test1.train`

5. **Configure a NPS run**

`Rscript npsR/nps_init.R testdata/Test1/Test1.summstats.txt testdata/Test1 testdata/Test1/Test1.train.2.5K_2.5K.fam testdata/Test1/Test1.train.2.5K_2.5K.phen Test1.train 80 testdata/Test1/npsdat`

The command arguments are:

* GWAS summary statistics file in the PREFORMATTED format: testdata/Test1/Test1.summstats.txt
* directory containing training genotypes: testdata/Test1
* IDs of training samples: testdata/Test1/Test1.train.2.5K_2.5K.fam
* Phenotypes of training samples: testdata/Test1/Test1.train.2.5K_2.5K.phen
* DatasetTag of training cohort genotypes: Test1.train
* Analysis window size: 80 SNPs. The window size of 80 SNPs for ~100,000 genome-wide SNPs is comparable to 4,000 SNPs for ~5,000,000 genome-wide SNPs.
* Directory to store intermediate data: testdata/Test1/npsdat. The NPS configuration and all intermediate data will be stored here.

*Note: output from running `./nps_check.sh init testdata/Test1/npsdat/`

```
Error: unexpected end of input
Execution halted
Error: unexpected end of input
Execution halted
./nps_check.sh: line 36: [: -lt: unary operator expected
Verifying nps_init:
Checking testdata/Test1/npsdat//args.RDS ...Error: unexpected end of input
Execution halted
OK (version )
Checking testdata/Test1/npsdat//log ...OK
```

6. **Set up the decorrelated "eigenlocus" space**

```
./run_all_chroms.sh sge/nps_decor_prune_gwassig.job testdata/Test1/npsdat/ 0
./run_all_chroms.sh sge/nps_decor_prune_gwassig.job testdata/Test1/npsdat/ 20
./run_all_chroms.sh sge/nps_decor_prune_gwassig.job testdata/Test1/npsdat/ 40
./run_all_chroms.sh sge/nps_decor_prune_gwassig.job testdata/Test1/npsdat/ 60
```

