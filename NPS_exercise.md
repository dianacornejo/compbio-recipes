# Install NSP: either clone it from github or download the latest version

Check this website for latest versions https://github.com/sgchun/nps/releases

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

OR

Just pull and run the docker image:

`statgen-setup login --tutorial nps --my-name [name]`. Take into account that this image already contains the data. 

## Installation of NPS

 **Install all the required libraries**

  * R v3.3 or later
  * OpenBLAS (linear algebra acceleration library): brew install openblas
  * Install R modules pROC and DescTools
    Rscript -e 'install.packages("pROC", repos="http://cran.r-project.org")'
    Rscript -e 'install.packages("DescTools", repos="http://cran.r-project.org")' 
    
  
  Note: to activate python2.7 type conda activate py2
## Running of NPS

### File preparation 

1.  **Prepare input files**

  * GWAS summary statistics: MINIMAL or PREFORMATTED (**Test1.summstats.txt**)
  * Training genotypes in the qctool dosage format the data should be splitted by chromosome and named          "chromN.DatasetTag.dosage.gz." (**chrom1.Test1.train.dosage.gz**)
  * Training sample IDs in the PLINK .fam format (**Test1.train.2.5K_2.5K.fam**)
  * Training phenotypes in the PLINK phenotype format (**Test1.train.2.5K_2.5K.phen**)
  * Validation genotypes in the qctool dosage format (**chrom1.Test1.val.dosage.gz**)
  * Validation sample IDs in the PLINK .fam format (**Test1.val.5K.fam**)
  * Validation phenotypes in the PLINK phenotype format (**Test1.val.5K.phen**)
   
2. **Unpack datafiles**
   
   Download and unpack the files in the following directory. Only if you didn't use the docker image
   
   ```
   cd nps-1.0.1/testdata/
   tar -zxvf NPS.Test1.tar.gz 
   ```
   
### Running on test set #1  

**Run NPS on test1 set without parallelization**

* Test set #1 is an unrealistic (small) dataset that can be tested in modest desktop computers without parallelization.

1. **Standardize the training genotypes** to the mean 0 and variance of 1 using `nps_stdgt.job`. The first parameter (`testdata/Test1`) is the location of training cohort data where NPS will find `chromN.DatasetTag.dosage.gz` files. The second parameter (`Test1.train`) is the DatasetID of the training cohort.
* Move to the `nps-1.1.0` folder and execute the following command.

```
cd nps-1.1.0/
./run_all_chroms.sh sge/nps_stdgt.job testdata/Test1 Test1.train
```   

### Note: 
* run_all_chroms.sh is a wapper script to perform the jobs sequentially, by processing one chromosome at a time
* nps_stdgt.job is a script to standardize the genotypes. Latest version of NPS (1.1.0) integrates compatible bash scripting for MacOS. 

* Verify that all jobs were successfully completed. If there is an error `FAIL` message will be printed.

```
./nps_check.sh stdgt testdata/Test1 Test1.train

#output
Verifying nps_stdgt:
Checking testdata/Test1/chrom1.Test1.train ...OK
Checking testdata/Test1/chrom2.Test1.train ...OK
Checking testdata/Test1/chrom3.Test1.train ...OK
Checking testdata/Test1/chrom4.Test1.train ...OK
Checking testdata/Test1/chrom5.Test1.train ...OK
Checking testdata/Test1/chrom6.Test1.train ...OK
Checking testdata/Test1/chrom7.Test1.train ...OK
Checking testdata/Test1/chrom8.Test1.train ...OK
Checking testdata/Test1/chrom9.Test1.train ...OK
Checking testdata/Test1/chrom10.Test1.train ...OK
Checking testdata/Test1/chrom11.Test1.train ...OK
Checking testdata/Test1/chrom12.Test1.train ...OK
Checking testdata/Test1/chrom13.Test1.train ...OK
Checking testdata/Test1/chrom14.Test1.train ...OK
Checking testdata/Test1/chrom15.Test1.train ...OK
Checking testdata/Test1/chrom16.Test1.train ...OK
Checking testdata/Test1/chrom17.Test1.train ...OK
Checking testdata/Test1/chrom18.Test1.train ...OK
Checking testdata/Test1/chrom19.Test1.train ...OK
Checking testdata/Test1/chrom20.Test1.train ...OK
Checking testdata/Test1/chrom21.Test1.train ...OK
Checking testdata/Test1/chrom22.Test1.train ...OK

```

2. **Configure a NPS run:** Next, run `npsR/nps_init.R` to configure an NPS run. 

```
Rscript npsR/nps_init.R testdata/Test1/Test1.summstats.txt testdata/Test1 testdata/Test1/Test1.train.2.5K_2.5K.fam testdata/Test1/Test1.train.2.5K_2.5K.phen Test1.train 80 testdata/Test1/npsdat
```

The command arguments are:

* GWAS summary statistics file in the PREFORMATTED format: `testdata/Test1/Test1.summstats.txt`
* Directory containing training genotypes: `testdata/Test1`
* Sample information of training samples: `testdata/Test1/Test1.train.2.5K_2.5K.fam`
* Phenotypes of training samples: `testdata/Test1/Test1.train.2.5K_2.5K.phen`
* DatasetID of training cohort genotypes: `Test1.train`
* Analysis window size: `80` SNPs. The window size of 80 SNPs for ~100,000 genome-wide SNPs is comparable to 4,000 SNPs for ~5,000,000 genome-wide SNPs.
* Directory to store intermediate data: `testdata/Test1/npsdat`. The NPS configuration and all intermediate data will be stored here.

To check the setup run  `./nps_check.sh init testdata/Test1/npsdat/`. If there is an error `FAIL` message will be printed. 

```
Checking testdata/Test1/chrom1.Test1.train ...OK
Checking testdata/Test1/chrom2.Test1.train ...OK
Checking testdata/Test1/chrom3.Test1.train ...OK
Checking testdata/Test1/chrom4.Test1.train ...OK
Checking testdata/Test1/chrom5.Test1.train ...OK
Checking testdata/Test1/chrom6.Test1.train ...OK
Checking testdata/Test1/chrom7.Test1.train ...OK
Checking testdata/Test1/chrom8.Test1.train ...OK
Checking testdata/Test1/chrom9.Test1.train ...OK
Checking testdata/Test1/chrom10.Test1.train ...OK
Checking testdata/Test1/chrom11.Test1.train ...OK
Checking testdata/Test1/chrom12.Test1.train ...OK
Checking testdata/Test1/chrom13.Test1.train ...OK
Checking testdata/Test1/chrom14.Test1.train ...OK
Checking testdata/Test1/chrom15.Test1.train ...OK
Checking testdata/Test1/chrom16.Test1.train ...OK
Checking testdata/Test1/chrom17.Test1.train ...OK
Checking testdata/Test1/chrom18.Test1.train ...OK
Checking testdata/Test1/chrom19.Test1.train ...OK
Checking testdata/Test1/chrom20.Test1.train ...OK
Checking testdata/Test1/chrom21.Test1.train ...OK
Checking testdata/Test1/chrom22.Test1.train ...OK
```

3. **Separate out the GWAS-significant peaks as a separate partition**

```
 ./run_all_chroms.sh sge/nps_gwassig.job testdata/Test1/npsdat/
 
 # Check the result of the last step
 
 ./nps_check.sh testdata/Test1/npsdat
 
 #Output
 
NPS data directory: testdata/Test1/npsdat
Verifying nps_gwassig:
Checking testdata/Test1/npsdat/log/nps_gwassig.Rout.1 ...OK
Checking testdata/Test1/npsdat/log/nps_gwassig.Rout.2 ...OK
...
 ```
 
4. **Set up the decorrelated "eigenlocus" space**. This step sets up the decorrelated eigenlocus space by projecting the data into the decorrelated domain and pruning residual correlations bretween windows. This is one of the most time-consuming steps of NPS. The first argument to `nps_decor_prune.job` is the NPS data directory, in this case, `testdata/Test1/npsdat/`. The second argument is the window shift. We recommend running NPS four times on shifted windows and merging the results in the last step. Specifically, we recommend shifting analysis windows by 0, WINSZ * 1/4, WINSZ * 2/4 and WINSZ * 3/4 SNPs, where WINSZ is the size of analysis window. For test set #1, we use the WINSZ of 80, thus the recommended window shifts should be `0`, `20`, `40` and `60`.

```
./run_all_chroms.sh sge/nps_decor_prune.job testdata/Test1/npsdat/ 0
./run_all_chroms.sh sge/nps_decor_prune.job testdata/Test1/npsdat/ 20
./run_all_chroms.sh sge/nps_decor_prune.job testdata/Test1/npsdat/ 40
./run_all_chroms.sh sge/nps_decor_prune.job testdata/Test1/npsdat/ 60

# Check the results of last step
./nps_check.sh testdata/Test1/npsdat/

#Output
NPS data directory: testdata/Test1/npsdat/
Detecting window shifts...: 4 shifts detected (0 20 40 60)
Verifying nps_decor:
----- Shifted by 0 -----
Checking testdata/Test1/npsdat//log/nps_decor.Rout.0.1 ...OK
Checking testdata/Test1/npsdat//log/nps_decor.Rout.0.2 ...OK
...
```

5. **Partition the rest of data**. We define the partition scheme by running `npsR/nps_prep_part.R`. The first argument is the NPS data directory (`testdata/Test1/npsdat/`). The second and third arguments are the numbers of partitions. We recommend 10-by-10 double-partitioning on the intervals of eigenvalues of projection and estimated effect sizes in the eigenlocus space, thus last two arguments are `10` and `10`

`Rscript npsR/nps_prep_part.R testdata/Test1/npsdat/ 10 10 `

Then, partitioned genetic risk scores will be calculated using training samples with `nps_part.job`. The first argument is the NPS data directory (`testdata/Test1/npsdat/`) and the second argument is the window shift (0, 20, 40 or 60):

```
./run_all_chroms.sh sge/nps_part.job testdata/Test1/npsdat/ 0
./run_all_chroms.sh sge/nps_part.job testdata/Test1/npsdat/ 20
./run_all_chroms.sh sge/nps_part.job testdata/Test1/npsdat/ 40
./run_all_chroms.sh sge/nps_part.job testdata/Test1/npsdat/ 60

# Check the results of last step
./nps_check.sh testdata/Test1/npsdat/
```

6. **Estimate per-partition shrinkage weights**. We estimate the per-partition weights using `npsR/nps_reweight.R`. The argument is the NPS data directory (`testdata/Test1/npsdat/`):

`Rscript npsR/nps_reweight.R testdata/Test1/npsdat/ `

The re-weighted effect sizes should be converted back to the original per-SNP space from the eigenlocus space. This will store per-SNP *re-weighted* effect sizes in files of `testdata/Test1/npsdat/Test1.train.win_shift.adjbetahat.chromN.txt`. The order of re-weighted effect sizes in these files are the same as the order of SNPs in the summary statistics file

7. **Validate the accuracy of prediction model in a validation cohort**. Last, polygenic risk scores will be calculated for each chromosome and for each individual in the validation cohort using sge/nps_score.dosage.job as follows:

```
./run_all_chroms.sh sge/nps_score.dosage.job testdata/Test1/npsdat/ testdata/Test1/ Test1.val 0 
./run_all_chroms.sh sge/nps_score.dosage.job testdata/Test1/npsdat/ testdata/Test1/ Test1.val 20
./run_all_chroms.sh sge/nps_score.dosage.job testdata/Test1/npsdat/ testdata/Test1/ Test1.val 40
./run_all_chroms.sh sge/nps_score.dosage.job testdata/Test1/npsdat/ testdata/Test1/ Test1.val 60

# Check the results
./nps_check.sh testdata/Test1/npsdat/
```
Here, the first argument for sge/nps_score.dosage.job is the NPS data directory (`testdata/Test1/npsdat/`), the second argument is the directory containing validation cohort data (`testdata/Test1/`), and the third argument is the DatasetID for validation genotypes. Since the genotype files for validation cohorts are named as `chromN.Test1.val.dosage.gz`, DatasetID has to be `Test1.val`. The last argument is the window shift (0, 20, 40 or 60).

8. **Report polygenic risk score**. `npsR/nps_val.R` will combine polygenic risk scores across all shifted windows and report per-individual scores along with overall accuracy statistics:

`Rscript npsR/nps_val.R testdata/Test1/npsdat/ Test1.val testdata/Test1/Test1.val.5K.fam testdata/Test1/Test1.val.5K.phen`

The command arguments are:

* NPS data directory: `testdata/Test1/npsdat/`
* DatasetID for validation genotypes: `Test1.val`
* Sample IDs of validation cohort: `testdata/Test1/Test1.val.5K.fam`
* Phenotypes of validation samples: `testdata/Test1/Test1.val.5K.phen`

The polygenic risk score for each individuals in the cohort are stored in the file `testdata/Test1/Test1.val.5K.phen.nps_score`
