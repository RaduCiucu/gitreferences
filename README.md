# Git References

Here is a short list of **GIT** snippets that I use daily while working on projects:
  - History
  - Branching
  - Stash
  - Committing
  - Bisect
  - filter-branch
  - Configuring
  - Cloning
  - Recovery

## Git Navigation History

Second Tab:

```sh
git log ; git lg
git checkout master
git checkout HEAD^2
git checkout “HEAD@{2 weeks ago}”
git checkout :/Commit message
```

## Branching

```sh
git checkout -b production # creates new branch prod and does checkout of production
git merge prototype/new_stuff # merges the branch "prototype/new_stuff" into the current branch
git branch -d prototype/unstable_stuff # deletes local branch "prototype/unstable_stuff"
git push origin :prototype/unstable_stuff # deletes remote branch "prototype/unstable_stuff" from the "origin" remote
```

## Stash

Retrieve a file from *stash*

```sh
git checkout stash@{0} -- _{{file_name}}_
```
Stash all tracked & untracked files

```sh
git stash -u
```

##  Committing

Commit files interactively by line
```sh
git add -p --
git add -p filename
```
Commit changes
```sh
git commit -m "commit message"
```

## Bisect

Trace nasty commits using git bisect
### Manually

```sh
git bisect reset # only needed after a bisect
git bisect start
git bisect good <revision>

git bisect bad <revision>

# git will checkout the next revision to check
git bisect (good | bad)
```

### Automated

```sh
git bisect start
git bisect good <revision>
git bisect bad <revision>
git bisect run/path/to/decision/script args...
```

## Branch Filtering

Author
```sh
git filter-branch -f --env-filter'

n=$GIT_AUTHOR_NAME
m=$GIT_AUTHOR_EMAIL

case ${GIT_AUTHOR_NAME} in
     "Radu Ciucu") n="Radu Ciucu" ; m="radu.ciucu@gmail.com" ;;
     "Radu Ciucu radu.ciucu@gmail.com") n="radu ciucu" ; m="radu.ciucu@gmail.com" ;;
esac

export GIT_AUTHOR_NAME="$n"
export GIT_AUTHOR_EMAIL="$m"
export GIT_COMMITTER_NAME="$n"
export GIT_COMMITTER_EMAIL="$m"
' HEAD
```

Remove filter-branch

```sh
git update-ref -d refs/original/refs/heads/master
```

## Pretty Git Config
This is a GIT configuration file that has "prettified" logs & authoring
```sh
[alias]
    bw = blame -w -M
    c = commit
    cc = commit --all --amend --no-edit
    ca = commit --all
    co = checkout
    cb = "!f() { git checkout `git log --until=\"$*\" -1 --format=%h`; } ; f"
    s = status --short
    d = diff
    dc = diff --cached --word-diff=color
    dw = diff --word-diff=color
    l = log
    a = add
    af = add -f
    p = push
    ss = show -1 --format="%B" --stat
    sw = show -1 --format="%B" --stat --word-diff=color
    whatis = show -s --pretty='tformat:%h (%s, %ad)' --date=short
    
    lg = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)%an%d%Creset %s [%N] %Cgreen(%ar)%Creset' --date=relative
    lgd = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)%an%d%Creset %s [%N] %Cgreen(%ar)%Creset' --date=default
    lgm = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)%an%d%Creset %s [%N] %Cgreen(%ar)%Creset' --date=relative --author="radu.ciucu@email.com"
    abbr = "!sh -c 'git rev-list --all | grep ^$1 | while read commit; do git --no-pager log -n1 --pretty=format:\"%H %ci %an %s%n\" $commit; done' -"

[web]
    browser = vivladi

[color]
    ui = always

[core]
    autocrlf = input
    pager = less -x1,5
    fileMode = false

[push]
    default = simple

[branch]
    autosetuprebase = remote
```

## Cloning

```sh
git clone git@github.com:raduciucu/gitreferences.git
```

Get all remote branches as local tracking branches except master and HEAD since you already got those with the clone command.

```sh
for branch in 'git branch -a | grep remotes/origin | grep -v HEAD | grep -v master'; do git branch --track ${branch##remotes/origin/} $branch; done
```

### Fetch and pull the remote tracking branches

 ```sh
git fetch --all
git pull --all
```

## Recovering the local branch from a force push
This will remove all commits previously on master and all commits done by yourself which are not yet pushed.

```sh
git fetch
git reset origin/master --hard
```

Preserve the changes and commit them again
This will move all changes to the working directory. All commits are done manually.

```sh
git fetch
git reset origin/master --soft
```

### Interactive Rebase. 
In this case you have to select which commits to keep.

```sh
git fetch
git rebase -i origin/master
```




