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

