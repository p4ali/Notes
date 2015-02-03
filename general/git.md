# Staging Area

Working directory => staging/index area => git repo (local) => git repo (remote)

When commit to git repo local, all contents are still local. 
When push to remote, you make your commits public.

| Command         |      desccription    |
|-----------------|:--------------------:|
| git add .       | Add current workdirectory to the Object store (repo) and create a index for that (git reset)  |
| git reset --    | (the oposite of git add) reset the index entries for all path to their state at tree-ish.

# Submodule

Submodules allow foreign repositories to be embedded (or linked, see gitlink) within a dedicated subdirectory 
of the source tree, always pointed at a particular commit.

It essentially allows you to attach an external repository inside another repository at a specific path.

Submodules are meant for different projects you would like to make part of your source tree, while the
history of the two projects still stays completely independent and you cannot modify the contents of the
submodule from within the main project

## Commands

```
$ git submodule add git@mygithost:project.git lib/p4java # create .gitmodules and add a submodule entry
$ git submodule update --init # populate the submodule recurisively
$ git submodule rm lib/p4java # remove submodule
```

### Do not forget to update submodule before you check in.

```
$ git submodule update --init # this will get the submodule commit by parent project.
$ cd lib/p4java
$ git pull # this will pull the most recent submodule
$ cd ../..
## Develop my code, and testing
$ git status
$ git add . # do this iff you want to change the submodule reference of the parent project
$ git commit -m"Check in my change to project, and also update the reference to the most recent p4java"
```

## Tig

```
$ tig staging/master # see the difference between master and stagging/master
```

## Branch

```bash
# create a branch
$ git branch my_branch
# or
$ git co -b my_branch

# in either case above, you did not set the upstream, unless you push -u later
$ git branch --set-upstream my_branch origin/my_branch # associate to remote branch
$ git push

# or you can 
$ git co -b my_branch
# push to remote
$ git push -u origin my_branch # this need only once

# or
$ git co -b my_branch orign/my_branch
# git push
```

## [Rebase](http://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

Rebase will replay local commits between upstream and branch to the newbase.

The following steps will be executed during rebase (assuming you are on **the current branch**):
* switch to branch if specified
  `git checkout <branch>`
* all changes made by commits in the current branch but that are not in the upstream are 
  saved to a temporary area. This is the same sets of commits shown by
  `git log <upstream>..HEAD`
* the current branch is reset to <upstream>, or <newbase> if specified. This is same as:
  `git reset --hard <upstream>`
* the commits that saved in temporary area are reapplied to **the current branch**, one
  by one, in order.

```bash
# full form rebase
$ git rebase [--onto <newbase>] [<upstream>] [<branch>]
```

Another way of thinking rebase is:
```bash
# full form
$ git rebase --onto <graft-point> <exclude-from> <include-from>
# shorter form
$ git rebase master
```

The shorter forms use defaults for things you don't specify:
* if you don't specify `--onto`, `<graft-point>` defaults to `<exclude-from>`
* if you don't specify an `<include-from>`, `<include-from>` defaults to the current branch
The commits will be apply:
```
git log <exclude-from>..<include-from>
```

### Cherry pick
For simple merge, you may not need rebase necessary. There is a more convenient way to do that:

```
dd2e86 - 946992 - 9143a9 - a6fd86 - 5a6057 [master]
           \
            76cada - 62ecb3 - b886a0 [feature]

# to commit 62ecb3 only to master
$ git checkout master
$ git cherry-pick 62ecb3

# to commits 76cada through 62ecb3 to master.
$ git checkout -b newbranch 62ecb3
$ git rebase --onto master 76cada^  # 76cada^ means previous commit, i.e., 946992, which is excluded

```
That’s all. 62ecb3 is now applied to the master branch and commited (as a new commit) in master (in first case). cherry-pick behaves just like merge. If git can’t apply the changes (e.g. you get merge conflicts), git leaves you to resolve the conflicts manually and make the commit yourself.

In second case, the result is that commits 76cada through 62ecb3 are applied to master.

## Revert previous commit

### Reset last commit and re-commit with local changes
```
add/edit
git reset --soft HEAD~1  
git commit
# or keep original commit message and time stamp
git commit -c ORIG_HEAD
```

### Temporarily switch to a different commit
If you want to temporarily go back and fool around
```
git co 0d123456c
# or
git co -b 0d123456c
```

### Hard delete unpublished commits
```
# reset current tip to 0d123456c, all local changes will be abandoned
git rest --hard 0d123456c

# alternatively, if you do want to keep local changes
git stash
git reset --hard 0d123456c
git stach pop
```

### Undo published commits with new commits
```
# This will create 3 separate revert commits, each for a change
git revert a867b4af 25eee4ca 0766c053

# or using ranges, this will revert the last two commits
git revert HEAD~2..HEAD

# reverting a merge commit
git revert -m 1 <merge_commit_sha>

# To get just one, you could use `rease-i` to squash them afterwards
# Or, do it manually (be sure to do this at top level of the repo)
# get the index and work tree into the desired state, without changing HEAD:
git co 0d123456c

# then commit
git commit
```

## Push

```bash
$ git --set-upstream <remote-branch> 
$ git push -u origin local-branch
```
