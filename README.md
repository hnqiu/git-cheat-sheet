# Using Git
Git is a distributed version control system for tracking code histories. If you are using MacOS or Ubuntu, Git should have been installed by default, otherwise follow the instructions on [git-scm](https://git-scm.com/downloads) to install it.  

For complete beginners I would recommend [a Git tutorial on Corey Schafer's Youtube channel](https://www.youtube.com/watch?v=HVsySz-h9r4). That's how I first learnt Git. 

## Common Git Commands

### First-Time Setup
Once git is installed, we need to set up the user name and email address because these user identity variables are required for every git commit. 
We can use 
```sh
git config --[option] <variable> "value"
``` 
to configure these variables, with `[option]` indicating the level of the git config file.

Usually we specify the `--global` option to set up the configuration to be used across all repositories we are working with, in which case the configuration will be stored in the `~/.gitconfig` file (this configuration only needs to be done once). We can also specify the `--local` option so that it can only affect the single current repository. In this case it will be stored in the `${git-repo-dir}/.git/config`.

Command | Description 
------- | :-----------
`git config --global user.name "<name>"`    | set up user name to be used across all repos
`git config --global user.email "<email>"`  | set up email address to be used across all repos
`git config --list`     | list git configuration
`git config --global -e`| open config file
`git config --global core.editor "vim"`     | set git editor to vim
`git config --global push.default simple`   | set push default behaviour 

`push.default` can be set as `matching` or `simple`. `matching` means git will push all local branches to the remote with the matching name. `simple` means only the current branch -- this is the default setting from Git 2.0.

### Initialization
When initializing a git repository, we either create a new one or clone one from remote.

Command             | Description 
-------             | :-----------
`git init`          | initialize an existing dir as a git repo
`git clone <url>`   | clone a repo
`git help [command]` or <br> `git [command] --help`    | get help info

### Adding & Modifying Files
Command         | Description
-------         | :-----------
`git status`    | check status
`git add [file]`| add a file to the staging area
`git add -A`    | add all files in the directory to the staging area
`git reset [file]`  | remove a file from the staging area
`git reset`         | remove all from the staging area
`git commit -m "[message]"` | commit staged contents
`git commit -am "[message]"`| stage all files that have been modified or added and commit
`git add --patch [file]` or <br> `git add -p [file]`    | stage parts/hunks of a file, prompted with options, e.g., <br> `y`: stage this hunk <br> `n`: do not stage this hunk <br> `s`: split the current hunk into smaller hunks <br> `q`: quit <br> `?` help

### Pull Push
To avoid consistently entering username and password when pushing repositories to GitHub, it is better to establish a secure connection by setting up an SSH key in our GitHub account. 
First generate a pair of SSH keys following [my SSH tutorial](https://hnqiu.github.io/2019/04/30/using-ssh.html#more-secure-authentication) and add the public key to the account following [this help document](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/), beginning from **step 2**.
Finally, use `set-url` to change the remote url to SSH.  
:exclamation: Do **NOT** share the private key `id_rsa` with anyone.

Command | Description
------- | :-----------
`git push` or <br> `git push origin [branchname]`  | push changes to the remote repo <br> `origin` is an alias on the local system for the remote repo
`git push -u origin [branchname]`               | push changes to the remote repo and associate local branch with the remote repo
`git push origin --delete [branch]` or <br> `git push origin :[branch]`| delete a remote branch
`git pull origin [branchname]`                  | pull changes from the remote branch
`git remote -v` | list remote repos
`git remote add origin https://github.com/<user>/[reponame].git`    | connect remote server to local
`git remote add origin git@github.com:<user>/[reponame].git`        | or use SSH if it is set
`git remote get-url --all origin`       | get remote's url
`git remote set-url origin git@github.com:<user>/[reponame].git`    | set remote's url with ssh
`git remote rm origin`      | remove the remote repo `origin`
`git stash`                 | save the current changes on a stack
`git stash list`            | list stored stashes
`git stash apply <stash@>`  | reapply the stashed changes

### Branch & Merge
Command | Description 
------- | :-----------
`git checkout [branchname]` | switch to branch
`git checkout -b [branch]`  | create a branch and switch to it
`git branch`                | list local branches (the asterisk mark `*` denotes the current branch)
`git branch -a`             | list all branches
`git branch [branchname]`   | create a branch
`git branch -d [branch]`    | delete a local branch
`git branch -D [branch]`    | force delete a branch even it is not fully merged
`git merge [branchname]`    | merge the branch into the current working branch
`git branch --merged`       | show branches that have been merged into the current branch
`git cherry-pick <commitID>`| merge changes from a commit

## Use `hub` from Command-Line

### Installation
`hub` is a [GitHub extension](https://hub.github.com/) to make GitHub tasks easier. Download [`hub` release](https://github.com/github/hub/releases), extract and install it. For instance, 

- Ubuntu  
  Extract the release to `./hub/` and 
  ```
  cd hub
  sudo ./install
  ```
  
- Windows  
  Extract the release and run the `install.bat` file. Use `hub` in `git-bash.exe` instead of in `cmd.exe`, otherwise you would need to add `C:\Program Files\Git\bin` to **System Path**.

### Commands
Command | Description 
------- | :-----------
`hub create`            | create a repo with default settings
`hub create -p`         | create a private repo
`hub create -d [desc]`  | create a repo with description

For full documentation visit the [`hub` manual](https://hub.github.com/hub.1.html). 

## A Simple Workflow
In this example, we are going to create a repository and connect it to GitHub. We are going to create a local `master` branch first and connect it remotely. Then we will also create a daily working branch, and push it to GitHub for our everyday work. Once we have completed a major section, we merge it into the `master` and push the `master` to remote.  
- For first-time users, set up git configuration and SSH keys 
  ```sh
  # git config
  git config --global user.name "<name>"
  git config --global user.email "<email addr>"

  # generate ssh keys
  # and add the public key on github
  ```
  :exclamation: `id_rsa.pub` is the public key. Do **NOT** paste the `id_rsa`.

- Either clone a repository from remote or create a local repository and push it to github:
  ```sh
  # clone a remote repo
  cd [workspace]
  git clone <url>
  git branch -a # to list local and remote branches
  ```
  
  ```sh
  # create a local git repo and push to remote
  # in this case, we also need to create a repo on github
  # the simple way is to create one on the website
  # or you can use `hub` command 
  cd [repodir]
  git init
  touch [newfile] # and add some contents
  git add [file] # use `git status` to see if it's staged
  git commit -m "msg" # or use `git commit -am "msg"` to replace the last two commands

  # create a repo on github then use the following `git remote` to connect them
  git remote add origin git@github.com:<user>/[reponame].git
  # or use `hub` command
  hub create

  # then
  git push origin master
  git branch -a # will list a local and remote `master` branch
  ```

- Edit files and update the repository in a local working branch
  ```sh
  # create a working branch and do everyday tasks in it
  git checkout -b [branch]
  touch [file] # and modify
  git add [file]
  git commit -m "msg"
  # then push to remote
  git push -u origin [branch]
  ```

- Merge the working branch into master
  ```sh
  # merge into master
  git checkout master
  git pull origin master
  git merge [branch] # use `git branch --merged` to show merged branches
  git push origin master
  ```

## More Advanced Commands

### Version Control 
Syntax | Description 
------- | :-----------
`<rev>^1` or `<rev>^`                   | the first parent of the specified commit object
`<rev>^n`                               | the `<n>`th parent of that commit
`<rev>^^` or `<rev>~2`                  | the first grand-parent of the specified commit object following the first parent

Command | Description 
------- | :-----------
`git log`                               | show git log
`git log --abbrev-commit`               | show git log with abbreviated commit IDs
`git show <commitID>`                   | show changes in that commit
`git diff`                              | show unstaged changes
`git diff --staged`                     | show changes between the staging area and last commit
`git diff <commitID>`                   | compare with the specified commit
`git diff <oldCommit>..<newCommit>`     | compare between two commits
`git commit --amend`                    | open editor to change the most recent commit message (staged changes will be commited as well)
`git commit --amend -m "new msg"`       | change the most recent commit message to "new msg"
`git reset HEAD~`                       | undo the last commit and leave changes unstaged (`HEAD` will be copied to `.git/ORIG_HEAD`)
`git reset --soft HEAD~`                | undo the last commit and leave changes staged
`git reset --hard <commitID>`           | roll back local repo to the specified version and remove previous commits
`git push origin [branchname] --force`  | force push local repo to remote
`git revert <commitID>`                 | roll back files and create commit to record rollback history
`git checkout <commitID> -- [file]`     | revert a specified file to the commited reversion and stage changes (not commited)
`git checkout <commitID>~1 -- [file]`   | revert a file to the first parent of the specified commit and stage changes

### Submodule 
Command | Description 
------- | :-----------
`git clone --recursive <url>`   | clone a repository including its submodules
`git pull --recurse-submodules` | pull changes in the repository including submodules
`git submodule add <url>`       | add an existing repository as a submodule under current directory
`git submodule init`            | initialize submodule configuration
`git submodule update --remote` | fetch and update all submodules


## Examples
- [Amending the most recent commit message](https://stackoverflow.com/questions/179123/how-to-modify-existing-unpushed-commits)
  ```sh
  git reset # unstaged changes since last commit
  git commit --amend -m "new msg" # or git commit --amend
  git push origin [branch] -f # force push commit if that commit was pushed before
  ```
- [Undo the most recent commit](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git)
  ```sh
  git reset HEAD~ # undo commit and leave changes unstaged
  # or `git reset --soft HEAD~` to leave changes staged
  # edit files and
  git add [file]
  git commit -c ORIG_HEAD # open editor with previous change commit message
  git push origin [branch] -f # force push commit if that commit was pushed before
  ```
- Add an existing repository as a submodule
  ```sh
  # cd to the directory that keeps the submodule and
  git submodule add <url>
  git status # will show that the submodule folder is tracked as "new file"
  git submodule init
  # fetch and update the submodule 
  git submodule update --remote [sub-repo]

  cd <sub-repo>
  git branch -a # should show it's in “detached HEAD” state
  git checkout master # [or] -b [newbranch]
  # make changes and commit, and
  git push origin [branch]

  cd <main-repo>
  git status # will show the submodule has "new commits"
  git add -A
  git commit -m "msg"
  git push # push main-repo to remote
  ```

## `.gitignore` Template
There is a [`.gitignore` template repository](https://github.com/github/gitignore) available. For `.gitignore` syntax please visit the [git document](https://git-scm.com/docs/gitignore).

