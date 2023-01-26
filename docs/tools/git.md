---
tags:
  - tools
  - git
---

# GIT

*Last update: 26 Jan 2023*

### Basic commands

Clone an existing repository

    git clone URL
    git clone git://...
    git clone https://

To create the initial repository:

    git init

Add a new file or update it in the stage
    
    git add

Commit (staged files)

    git commit -m "comment"

Push changes

    git push -v
    git push --tags

Check Status

    git status
    git status --cached

Rebase ignoring whitespace differences

    git rebase -v --ignore-whitespace


### Aliases

Some useful aliases

    alias gf='git fetch -v'
    alias gr='git rebase -v --ignore-whitespace'
    alias gs='git status'

### Configuration

Scopes:

    * system: all users all repos
    * global: current user
    * local:  current repo

Commands:

    git config --global merge.tool /c/APP/meld/meld/meld.exe
    git config --list
    git config user.name  "John Doe"
    git config user.email "j.doe@example.it"

    git config --global core.filemode false  ## Tell git to ignore file permission changes
    git config --global core.editor "vim"
    git config --global core.editor "'c:/program files (x86)/sublime text 3/sublimetext.exe' -w"

    git config --global alias.st status      ## Alias
    git config --global alias.ck checkout    ## Alias

    git config push.default upstream   ## Avoid push on all branches

The .gitignore file

    .gitignore            # for the current repo
    ~/.config/git/ignore  # for all repos


### Logging

Basic logging:

    git log --pretty=oneline -5

Logging configuration:

    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

Show what has been changed

    git log -p

Show last n commits

    git log -<n>
  
Show stats for each new/changed file

    git log --stat 

Compare two branches

    git log --left-right --graph --cherry-pick --oneline branch1...branch2


### Tags

Create a new tag

    git tag -a v1.4 -m 'my version 1.4'

Push tags

    git push --follow-tags -v

List tags ordered by creation date

    git for-each-ref --sort=creatordate --format '%(refname) %(creatordate)' refs/tags


### Branches

Link a local branch to a remote one

    git push --set-upstream origin <remote-branch>

    git branch --set-upstream-to=origin/remote-branch local-branch

Create a new local branch

    git checkout -b <branch>

Avoid pushing to all branches

    git config push.default upstream

Clean the branch:

    git clean -i .
    git clean -f .

Remove all local commits and changes and reset the local branch to the remote branch status

    git reset --hard @{u}


### Special commands

GIT internal DB maintenance:

    git gc --auto --prune
    git fsck --full
    git prune --expire now

Updates and cleanup remote branch list

    git remote update origin --prune

Patch

    git format-patch -1 <sha1> --stdout > myPatch.patch
    git log --pretty=oneline -7 | tac  | awk '{print "git format-patch -1 "  $1  " --stdout > "}' > 1
    git am --signoff -k < <patch-file>

Create a second remote repository

    git remote add backup  URL
    git remote set-url --add  --push backup  URL
    git push backup <branch-name>

