# Staging Area

Working directory => staging/index area => git repo (local) => git repo (remote)

When commit to git repo local, all contents are still local. 
When push to remote, you make your commits public.

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

## Rebase

Rebase will replay local commits between upstreanm and branch to the newbase.

The following steps will be executed during rebase:
* switch to brand if specified
  `git checkout <branch>`
* all changes made by commits in the current branch but that are not in the upstream are 
  saved to a temporary area. This is the same sets of commits shown by
  `git log <upstream>..HEAD`
* the current branch is reset to <upstream>, or <newbase> if specified. This is same as:
  `git reset --hard <upstream>`
* the commits that saved in temporary area are reapplied to the current branch, one
  by one, in order.

```
git rebase [--onto <newbase>] [<upstream>] [<branch>]

# when <branch> is specified, git rebase will perform an automatic switch 
# to <branch> before doing anything else. Otherwise, it will remain on current branch
git checkout <branch>


```
