# Git: Guide to your first pull request
## Table of contents
1. Getting started  
2. Git init, git add, git commit, git push  
3. How our graph is changing over commits
4. Remotes, branches
5. Fork, pull request

## Getting started
First thing first, you need git installed on your local machine. This can be simply 
done by  
[executing git install runfile on your local machine from the official website](https://git-scm.com/downloads),  
or you can just directly install the package using builtin package manager if you're running linux-based OS:
```bash
# Ubuntu distro
$ sudo apt-get install git 

# RPM distio(Fedora, CentOS ..)
$ sudo yum install git

# In MacOS, brew has to be installed first.
$ brew install git 
```
There are many different GUI-based client tools out there for committing and browsing git, but I strongly recommend beginners to start with 
the commnad-line interface that comes with git scm.  
Note that if you're using Windows OS, you'll already have **git bash** executable after the installation is done.  
(See [how-to-install-git-bash-on-windows](http://www.techoism.com/how-to-install-git-bash-on-windows/) for more details.)  
Lastly, ensure that the ```git``` command works fine on your local machine:
```bash
$ git --help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]
            ...
            ...
$ git --version
git version 2.17.0
```
Nice, now you're all set! 

## Git init, git add, git commit, git push
### Git init
1. First let's make an empty directory, `myfolder/`  
2. cd into `myfolder/`,  
3. then we'll initialize this empty directory as a git repository, by typing `git init` command.
Now we'll call this initialized directory as **local repository**

```bash
$ git init
Initialized empty Git repository in /Users/{your_path_to_myfolder}/myfolder/.git/

$ ls -al
total 0
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:22 .
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:04 ..
drwxr-xr-x  10 sangsulee  staff  320 Oct 16 10:22 .git
```
You'll find `.git/` directory created once you typed `git init`. Git tracks all version histories 
of the local repository such as file changes, all commits done, or other important user configurations
from the files reside in this`.git/` folder. So, make sure not to remove/modify them manually :).
### Git status
Now let's make some dummy files in our **local repository**.  
As a note, we should separate out the term **local repositoy** with **remote repository**. We'll cover this terms again.
```bash
# Create empty files
$ touch haha.txt hoho.txt 
```
Then we have directory tree looks like this:
```bash
# Directory tree
─── myfolder
    ├── .git/
    ├── haha.txt
    └── hoho.txt
```

