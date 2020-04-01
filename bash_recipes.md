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
Ctrl+A | Move to the beginning of the line in the shell| |
Ctrl+E | Move to the end of the line in the shell| |
Ctrl+R | Search for previously typed commands | |
Crtl+C | Kill the process (STOP NOW)| |
Crtl+D | End of file (I'm all done)|
!! | Shortcut to run the last command used | `!!`|
!$ | Shortcut to retrieve the last word of the last command | `!$`|
whoami | ID of the current user | `whoami`|
pwd | Where are you? print working directory | `pwd`|
ls | List contents of the current directory ls=listing | `ls` or `ls --help` for a full list of options|
ls . /directory | Lists contents in both directories | `ls . /directory`|
lf -F | Which tells ls to add a trailing / to the names of directories | `ls -F`|
ls -Fa | To display the parent directory `..` | `ls -Fa`|
man <command> | For information of how to run other programs and commands within bash | `man ls`|
cd <dir_name> | Change directory | `cd /home/diana`|
cd | Returns you to the home directory | `cd` |
cd .. | move up to the parent directory | `cd ..` |
. | Current working directory | `ls .`|
~ | The `~` (tilde) at the start of a path means “the current user’s home directory” | `cd ~`, `cd ~/software` = `cd /home/dc2325/software`|
cd - | Brings you back to the last directory you were in | `cd -`|
mkdir <dir_name> | Make directories | `mkdir data` |
nano example.txt | To create a file with a text editor you can also use Vim or Emacs. Ctrl+o to save and Ctrl+x to exit | `nano example.txt` |
rm <file> | Remove files | `rm example.txt` |
rm -r -i | Remove directories and files within | `rm -r -i software` |
mv <old_file> <new_file> | Rename files using move use the -i (interactive) option if you want confirmation  |`mv software/old_file software/new_file` |
mv <file1> . | Move a file to the current working directory | `mv software/file1 .` |
cp <old_file> <software/new_file> | Copy a file to a diferent place | `cp <old_file> <software/new_file>` |
wc <file> | Word count: counts the number of lines, words and characters in each file | `wc file.txt` |
wc -l <file> | Counts the number of lines in the file, other options are -w (words) and -c (characters) | `wc -l file.txt` |
wc -l *.txt > file_name.txt | Redirects the output to a .txt file | `wc -l *.txt > lenghts.txt ` |
cat <file> | Prints the output of the file in the screen cat=concatenate | `cat lenghts.txt` |
less <file> | To display a screenful of the file and then stop | `less lengths.txt` |
sort -n <file> | It sorts the file based on numerical data (-n) and displays output (does not change the file). Other option  is -u that will retain unique values | `sort -n lenghts.txt ` |
head -n 1 <file> | To view the first line (-n 1) of the sorted file | `head -n 1 sorted_lenghts.txt` |
sort -n <file> \| head -n 1 | Here use sort and head together in a pipe | `sort -n lengths.txt \| head -n 1`|
tail -n 5 <file> | It displays the last 5 lines of the file | `tail -n 5 lenghts.txt`|
uniq <file> | Only removes adyacent duplicates within a file | `uniq file.txt` |
sort file.txt \| uniq | This pipe will remove all the duplicates in a file | `sort file.txt \| uniq`
cut -d , -f 2 <file> | The -d especifies delimiter in this case ',' and -f the column to retain | `cut -d , -f 2 <file>`
history \| tail -n 5 | The history command shows the last 5 commands that have been executed. You can re-execute by typing hictory and the command number |`history $123`|

  
## Usage of Loops

```
for filename in file1.dat file2.dat
> do
>    head -n 3 $filename
> done
```
Use ${filename} to explicitly indicate the variable name

```
for filename in *.dat
do
    echo $filename
    head -n 100 $filename | tail -n 2
done
```

```
for filename in *.dat
do
    cp $filename original-$filename
done
```

Another way of writing the loops in the shell

```
for datafile in *[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
```

### Nested loops

```
for sugar in fructose glucose sucrose
do
    for temperature in 25 30 37 40
    do
        mkdir $sugar-$temperature
    done
done
```

## Shell scripts

* To create a script you can use `nano script.sh`

* Then you add whatever the commands you want to run, for example, `head -n "$2" "$1" | tail -n "$3"`, where `$1` represents the first parameter on the command line, `$2` and `$3` represent the number of lines that we want to retreive for the `head` and `tail` commands

* Afterwards you can just run the script in all the data by using 
  * `bash script.sh`
  * `bash script.sh filename`
  * `bash script.sh filename 15 5`

* As a good practice always make sure your script is understandable to other and add comments to it
```
  # Select lines from the middle of a file.
  # Usage: bash middle.sh filename end_line num_lines
  ```
  
  The variable `"$@"` means all of the command-line parameteres to the shell script
  
  ### Debugging scripts
  
  Adding the -x option will help to debug scripts since the bash is going to show a trace of what it is doing.
  
  ```
  bash -x do-errors.sh *[AB].txt
  ```
## Finding things in the shell: grep, find 

Find the lines that contain the word not

`grep not file.txt`

To limit the search to word boundaries

`grep -w day file.txt`

To look for complete phrases use the `quotes`

`grep -w "is not" file.txt`

The flag `-n` outputs the lines where there is a match

`grep -n "it" file.txt`

The flag `-w` searches for the entire word, `-n` outputs the line where there is a match and `-i` makes the search case-insensitive

`grep -n -w -i "the" file.txt`

The flag `-v` inverts the search. In this case it looks for the lines that **do not** contain the word "the"

`grep -n -w -v "the" file.txt`

**Find**

Command | Description | Usage
--------|-------------|-------
find . | Outputs all the files within the working directory (wd) | `find .`
find . -type d | Outputs the directories within the wd | `find . -type d`
find . -type f | Outputs a list of files | `find .-type f`
find . -name '\*.txt' | Outputs the files that end in .txt | `find . -name '*.txt'`

**Subshells*

Integrating commands

`wc -l $(find . -name '*.txt')`
`grep "FE" $(find .. -name '*.pdb')` Find all of the iron atoms in the pdb files 





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



