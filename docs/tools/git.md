# GIT

Useful GIT commands

### Initialization

To create the initial repository:

    git init


### Aliases

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


## Aggiunge un file nuovo o lo mette in stage o riaggiorna lo stage
git add

## Commit (da stage)
git commit -m "commento"

## Riallinea il remoto (origin) con il locale (master)
git push origin master
git push --tags

## Clone
git clone URL
git clone git://...
git clone http://
git clone https://
git clone --branch v2.2.0 git@bitbucket.org:factor-y/nlmachine.git /data/nlmachine/target/checkout


## File ignore
.gitignore            # nel progetto
~/.config/git/ignore  # valido per tutti i progetti



## Status
git status
git status --cached

## Logging

    git log --pretty=oneline -5

    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

    # mostra i cambiamenti
    git log -p

  # mostra gli ultimi <numero> commit
  git log -<numero> mostra gli ultimi <numero> commit
  
  # mostra le statisiche per ogni file toccato dal commit
  git log --stat 

  # Mostra le differenze tra un branch e l'altro
  git log --left-right --graph --cherry-pick --oneline develop...prod


## TAGS

    ## Creazione tag
    git tag -a v1.4 -m 'my version 1.4'

    ## Push dei tag
    git push --follow-tags -v

    ## Elenco tag ordinati per data
    git for-each-ref --sort=creatordate --format '%(refname) %(creatordate)' refs/tags


## STASH
(nel branch) git stash
git checkout master
....
git checkout <branch>
git stash apply

## Aggancia un branch locale ad uno remoto
git push --set-upstream origin <branch-remoto>
##git branch --set-upstream-to=origin/nome-branch-remoto nome-branch-locale


## Scarica un branch da remoto
git fetch
git checkout <branch>
( equivalente a git checkout -b <branch> origin/<branch> )

## Crea un nuovo branch (prima in locale e poi su origin)
git checkout -b <branch>
git push -u origin <branch>

## Evitare che PUSH operi su tutti
git config push.default upstream

## Per cancellare i file untracked
git clean -i .
git clean -f .

## Rimuovere commit locali e riallinearsi con il branch remote di upstream
git reset --hard @{u}

## Rebase senza tenere conto delle differenze sui whitespace
git rebase -v --ignore-whitespace


## Manutenzione
git gc --auto --prune
git fsck --full
git prune --expire now

## Aggiorna l'elenco locale dei branch
git remote update origin --prune

## Patch
git format-patch -1 <sha1> --stdout > myPatch.patch
git log --pretty=oneline -7 | tac  | awk '{print "git format-patch -1 "  $1  " --stdout > "}' > 1
git am --signoff -k < <patch-file>

git log --pretty=oneline -7 | tac  | awk '{print "git diff-tree --no-commit-id --name-only -r " $1}' > 2

## Creazione di un secondo remote
git remote add backup  git@bitbucket.org:factor-y/backup-portletpiastra.git
git remote set-url --add  --push backup  git@bitbucket.org:factor-y/backup-portletpiastra.git
git push backup <nome-branch>



