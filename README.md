# Recipes for computational biology
Here you can find installation commands, scripts and configurations to run genetic software. This is in order to keep everything tidy. 


## Starting to use Git/GitHub
Some useful commands are:

Command | Description | Usage
--------|-------------|-------
git clone | This command clones repositories from github | `git clone https://github.com/gaow/lab-wiki` |
git status | This command displays the state of your working directory and the staging area| `git status`|
git diff | Let you see differences between the working tree and the index or a tree| |
git add | This serves to add the file to the repo. Every time you change a file add it to the repo using this command and commiting it | `git add filename.md` |
git commit | Commit refers to saved changes. It will save the changes to file and add the description. The -m command creates a description of the file to help you and others understand the change in the code | `git commit -am "Add example"`|
git push | To upload your local changes into your online repository | `git push origin master` |
git pull | If you have cloned a previous copy to your computer before editing **update to the current version** that might have been changed by other since you last cloned| `git pull`|
git checkout | | `git checkout <filename>` |
git reset | | `git reset --hard` |
git log| Keeps track of any change to any file| `git log` |
git blame | Keeps track of changes more specifically. It allows you to easily see the history of modifications of a file, or restore the file to any time point in the history | `git blame` |
git branch BRANCHNAME | to create a branch in someone else's repository | `git branch BRANCHNAME`
git checkout -b BRANCHNAME | another way of creating branches | `git checkout -b ld-module`

### First GitHub project
1. Create a GitHub repository online going to `https://github.com/dianacornejo'`
1. Under `repositories` click `new repository` and call it *compbio-recipes*. When creating the repository select "Include a **README.md** file".
1. Download or "clone" the repo to your local computer
  `$ git clone https://github.com/dianacornejo/compbio-recipes`
1. **Add files to the repository**:  
`$ echo "# Recipes for software configurations" > software-config.md
 $ git add software-config.md
 $ git commit -am "Add a software configuration file with markdown`
1. Push your change to repo into GitHub. This is an important step to upload your changes to the online repository
`git push`
1. Update the local clone of repositories before editing and commiting new information
`git pull`


  
## Iphyton notebook and Jupyterlab

This is to document clearly what you do in research

###**SoS installation** 

###**SoS workflow and sos-notebook installation with Conda**

* `conda install sos sos-pbs -c conda-forge`
* `conda install sos-notebook jupyterlab-sos sos-papermill -c conda-forge`
* `conda install sos-r sos-python sos-bash -c conda-forge`
* `conda install sos-r -c conda-forge`
* `conda install sos-bash -c conda-forge`

#### How to access you SoS notebook using docker
* `$ docker run -it mdabioinfo/sos:latest /bin/bash`
* `$ docker run -d -p 9999:8888 mdabioinfo/sos-notebook`
* `$ docker ps` Finds the of the instances (look at the last column of the output, for example mine is: *priceless_archimedes*)
* `$ docker logs priceless_archimedes`

Now enter the URL into a browser. Tip: change the text within ( ) to your ip 

<http://(10.124.143.97):8888/?token=2fcb6e87003ac99f26b7a1c021327d7dd31595e0e4b60173>

* `$ docker run -d -p 8888:8888 -v $HOME:/Users/dmcs2245/work  mdabioinfo/sos-notebook`

If you get a message stating that the port is already allocated use -p 9999:8888 instead

## Making the WGS example for SoS from Gao

1. Using docker access the SoS notebook 
2. Clone the repository to you local folder
  `git clone https://github.com/vatlab/sos-docs/`
3. Prepare the data as explained in Gao's tutorial
4. Run sos run WGS_Call.ipynb --samples k9-test/test_samples.manifest -j2

A directory containing the results will be created 



