# Git References

Here is a short list of **GIT** stuff that I use daily while working on projects:
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
git push origin :rototype/unstable_stuff # deletes remote branch "rototype/unstable_stuff" from the "origin" remote
```

## Stash
Retrieve a file from *stash*

```sh
git checkout stash@{0} -- _{{filename}}_
```
