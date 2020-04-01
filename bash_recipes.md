# Basic shell usage

This tutorial for simple linux commands can be found here: https://docs.ycrc.yale.edu/PIL/

**TIPS**

File system organization 
**/ root:** holds everything else
**bin:** where some built in programs are stored
**data:** miscellaneous data files
**home:** where user's personal directories are located
**tmp:** for temporary files.

Command | Description | Usage
--------|-------------|-------
whoami | ID of the current user | `whoami`
pwd | Where are you? print working directory | `pwd`
ls | List contents of the current directory ls=listing | `ls` or `ls --help` for a full list of options
ls . /directory | Lists contents in both directories | `ls . /directory`
lf -F | Which tells ls to add a trailing / to the names of directories | `ls -F`
ls -Fa | To display the parent directory `..` | `ls -Fa`
man <command> | For information of how to run other programs and commands within bash | `man ls`
cd <dir_name> | Change directory | `cd /home/diana`
cd | Returns you to the home directory | `cd`
cd .. | move up to the parent directory | `cd ..`
. | Current working directory | `ls .`|
~ | The `~` (tilde) at the start of a path means “the current user’s home directory” | `cd ~`, `cd ~/software` = `cd /home/dc2325/software`
cd - | Brings you back to the last directory you were in | `cd -`
mkdir <dir_name> | Make directories | `mkdir data`
nano example.txt | To create a file with a text editor you can also use Vim or Emacs. Ctrl+o to save and Ctrl+x to exit | `nano example.txt`
rm <file> | Remove files | `rm example.txt`
rm -r -i | Remove directories and files within | `rm -r -i software`
mv <old_file> <new_file> | Rename files using move use the -i (interactive) option if you want confirmation  |`mv software/old_file software/new_file`
mv <file1> . | Move a file to the current working directory | `mv software/file1 .`
cp <old_file> <software/new_file> | Copy a file to a diferent place | `cp <old_file> <software/new_file>`
wc <file> | Word count: counts the number of lines, words and characters in each file | `wc file.txt`
wc -l <file> | Counts the number of lines in the file | `wc -l file.txt`






## Configuring the PATH

`$ echo $PATH` to see the directories included in the PATH
`$ export PATH=$PATH:/path/to/my/program` To add the path to your program
`$ echo $PATH`

##Copying directories from one location to another

`$ cp -R path_to_source path_to_destination/`

## Editing the bash_profile

`$ vim ~/.bash_profile`

## Make an executable file

`$ chmod 755 YourScriptName.sh`

Execute the file using " ./ " e.g. ./annotate_variants.pl



