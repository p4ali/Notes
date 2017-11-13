# Show branch name as PS:
This is one of the functions [bash_it](https://github.com/Bash-it/bash-it) provide.

## `~/.gitconfig`
Pay attention to **[include]** and **[alias]** sections.

## alternatively, you can put this on your .bashrc file

```
# Git branch in prompt.

parse_git_branch() {

    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'

}

export PS1="\u@\h \w\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
```

# Staging Area

Working directory => staging/index area => git repo (local) => git repo (remote)

When commit to git repo local, all contents are still local. 
When push to remote, you make your commits public.

| Command         |      desccription    |
|-----------------|:--------------------:|
| git add .       | Add current workdirectory to the Object store (repo) and create a index for that (git reset)  |
| git reset --    | (the oposite of git add) reset the index entries for all path to their state at tree-ish.|
|git clean -df && git checkout -- . |`git clean` removes all untracked files and `git checkout` clears all unstaged changes.|
|git checkout-index -a -f| checks out all files from the index, overwriting working tree files.|

# File Status Lifecycle
Here we talk about file states in the working directory:
* Tracked: Files that were in the last snapshot
* Unmodified: Tracked files which was NOT changed since your last commit
* Modified: Tracked files which was changed since your last commit
* Staged: Files which was added to the staging area
* Untracked: anything else
When you first clone a repository, all of your files will be tracked and unmodfied. 


|                 | Untracked       | Unmodified           |  Modified           | Staged                |
|:----------------|:----------------|:---------------------|:--------------------|:---------------------:|
| **Untracked**   |                 |                      |                     | git add               |
| **Unmodified**  |                 |                      |                     |                       |
| **Modified**    |                 | git checkout         |                     | git add               |
| **Staged**      |                 |                      |                     |                       |

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
$ git clone --recurse-submodules ssh://git@pbitbucket.com:7999/~ali/parent_project.git # many commands support recurse-submoduels option. It will clone submodules.
$ git submodule update --init # populate the submodule, incase you did not use the recurse-submodules to clone
$ git submodule rm lib/p4java # remove submodule
```

### Do not forget to update submodule before you check in.

```
$ git ls-tree parent_commit # this will show the sha of submodule in the parent_commit
$ git diff # this will show if there is any changes in the submodule, or submodule sha changes (e.g., you switch branch in submodule)
$ git submodule update --init # this will get the submodule commit by parent project.
$ cd lib/p4java
$ git co master # this will switch to master branch, which may cause changes in parent project
$ vi source_file
$ git commit # this will change the submodule
$ cd ../..
## Develop my code, and testing
$ git status
$ git add . # do this iff you want to change the submodule reference of the parent project
$ git commit -m"Check in my change to project, and also update the reference to the most recent p4java"
```

### Recursively update submodule

```
$ git submodule update --init --recursive # pupulate submodules recursively
$ git submodule foreach --recursive git checkout master # run git checkout for each submodules
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

# delete a local branch
$ git branch -D my_branch

# delete a remote branch
$ git push origin --delete my_branch

# You may need prune(remove) any remote-tracking references that no longer exist on the remote, incase you got "remote refs do not exist" error
$ git fetch --prune origin

# delete a remote branch 
$ git push origin :my_branch
Remeber `git push [remotename] [localbranch]:[remotebranch]`. If you leave off the [localbranch] portion, then you’re basically saying, “Take nothing on my side and make it be [remotebranch].”

# List remote branch on origin
git for-each-ref --sort=-committerdate refs/remotes/origin/ --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset) - %(objectname:short) - %(color:red)%(authorname)%(color:reset) (%(color:green)%(committerdate:relative)%(color:reset))'

# list branch creator
git for-each-ref --format='%(committerdate) %09 %(authorname) %09 %(refname)' | sort -k5n -k2M -k3n -k4n

```

## [Rebase](http://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

Rebase will replay local commits between upstream and branch to the newbase. For more example, see [`git reset` document](http://git-scm.com/docs/git-reset)

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

# squash last 2 commit into one
$ git rebase -i HEAD~2 
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

A simiple flow
```
# update master
git co master
git pull
# rebase branch
git co max_projects_count_20150128_WIP
git rebase master
# merge back to master
git co master
git merge max_projects_count_20150128_WIP
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

### Soft reset (move HEAD only; neither staging nor working dir is changed)

```git reset --soft 073791e7dd71b90daa853b2c5acc2c925f02dbc6```

### Default: Mixed reset (move HEAD and change staging to match repo; does not affect working dir)

``` git reset --mixed 073791e7dd71b90daa853b2c5acc2c925f02dbc6 ```

### Hard reset (move HEAD and change staging dir and working dir to match repo)

```git reset --hard 073791e7dd71b90daa853b2c5acc2c925f02dbc6```


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
```bash
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

# revert a previoud commit and not commit
git revert -n <commit>
# fix conflict
git revert --continue
git add .
git commit
```

### Unmodify multiple files
```bash
git checkout -- . # discard changes in working directory
git checkout -- app/views/posts/index.html.erb # discard only one file
git checkout -- * # discard files in current dir
```

## undo a [rebase](http://stackoverflow.com/questions/17711146/how-to-open-link-in-new-tab-on-html)
Two ways
```bash
# reset to reflog just before the rebase
$ git reflog
a9cdf47 HEAD@{5}: rebase finished: returning to refs/heads/hc-2033
a9cdf47 HEAD@{6}: rebase: Refactor and add more tests.
dcd1693 HEAD@{7}: rebase: Add options for running the task in report mode, which will not remove the file, but just report orphan files.
2c2c448 HEAD@{8}: rebase: Add more debug info, and fix a small bug (use the ending_changelist as starting_changelist on reload.
c78d2e5 HEAD@{9}: rebase: [hc-2033] Add rake task which migrated from python scripts.
9896166 HEAD@{10}: rebase: Add a rake task - atlas:clean_failed_change [HC-2033] (WIP)
d91a7c2 HEAD@{11}: rebase: checkout master
a9aa270 HEAD@{12}: commit: Refactor and add more tests.
$ git reset --hard HEAD@{12}
$ git rebase -i --abort # get rid of “Interactive rebase already started”.

# Actually, rebase|reset|merge saves your starting point to ORIG_HEAD. 
# So if you only did a rebase, then you can reset to ORIG_HEAD.
$ git reset --hard ORIG_HEAD
```

## [ORIG_HEAD](http://stackoverflow.com/questions/964876/head-and-orig-head-in-git)
ORIG_HEAD is previous state of HEAD, set by commands that have possibly dangerous behavior, to be easy to revert them. It is less useful now that Git has reflog: HEAD@{1} is roughly equivalent to ORIG_HEAD except for multi-commit commands, e.g., rebase|merge|reset (HEAD@{1} is always last value of HEAD, ORIG_HEAD is last value of HEAD before dangerous operation).

* "pull" or "merge" always leaves the original tip of the current branch in ORIG_HEAD.
* Before any patches are applied, ORIG_HEAD is set to the tip of the current branch.

```bash
$ git commit -c ORIG_HEAD # commit with the previous head comment 
```

## Push

```bash
$ git --set-upstream <remote-branch> 
$ git push -u origin local-branch
# push only 2 oldest of 5 local changes in master to remote master
$ git push origin master~3:master
```

## tilde `~` and caret `^` in [git revisions](https://git-scm.com/docs/gitrevisions/2.5.2)

* `HEAD~n` specicify the *nth* ancestor commit, e.g., `A~2` means the `D`.
* `HEAD^n` allows you to select the *nth* parent of the commit, e.g., `A^2` means the `C`. `A~1^3` means `F`. `A^^` means `A`'s parent's parent, i.e., `D`.

A commit may have multiple parent. e.g., A has parents B and C. (following is an illustration, by Jon Loeliger.)
Both commit nodes B and C are parents of commit node A. Parent commits are ordered left-to-right.

```
G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A

A =      = A^0
B = A^   = A^1     = A~1
C = A^2  = A^2
D = A^^  = A^1^1   = A~2
E = B^2  = A^^2
F = B^3  = A^^3
G = A^^^ = A^1^1^1 = A~3
H = D^2  = B^^2    = A^^^2  = A~2^2
I = F^   = B^3^    = A^^3^
J = F^2  = B^3^2   = A^^3^2
```

## [range with `..` and `...`](https://stackoverflow.com/questions/462974/what-are-the-differences-between-double-dot-and-triple-dot-in-git-com)
```bash
A <- B  <- E <- F <- master
      \ <- C <- D <- dev
      
# double dot shows range of commits reachable from one but not other tip
$ git log master..dev # commits reachable by dev, i.e., D,C, equal to git log ^master dev
$ git log dev..master # show F,E

# triple dot shows range of commits reachable by either of two refs, not both
$ git log --left-right master...dev 
< F
< E
> D
> C

# mutiple points
```bash
# to see all commits reachable from refA and refB, but not refC, use one of following: (^ means negative, i.e., --not)
$ git log refA refB ^refC
$ git log refA refB --not refC
```

## merge from branch and rebase master

```bash
4624  git show b847499
 4625  git jlfa
 4626  git reset --head HEAD^
 4627  git reset --hard HEAD^
 4628  git show dcfc5b5
 4629  git merge dcfc5b5
 4630  git co WIP_delete_frontend
 4631  git rebase master
 4632  git diff master..origin/master
 4633  git diff origin/master..master
 4634  git co master
 4635  git br -d WIP_delete_frontend
 4636  git push origin :WIP_delete_frontend
 4637  git jlga
```

## `git merge --squash dev_branch`
This will merge the changes(assuming you are in master) from `dev_branch` to master branch, but whithout commit. You need manually commit it (and also manually resolve the conflicts if has). This allow merge the dev_branch changes into one chunch.
```bash
git co dev_branch
... working on dev_branch and comit, push to origin/dev_branch
git co master
git merge --squash dev_branch
... resolve confilcts if necessary, and git add conflicts
git commit -m"merge changes from dev_branch"
git lg # this will show you a linear history
```

## search log
```
# search patches(commits) that introduced a change that add/removed line containaing regex
# -p print the patch file
git log -Gregex -p 
git log -g --grep="0052"
```

## delete files
```
git rm .project
git add -A
git commit -m "delete .project"
```

## Set `difftool`
```
## the first 2 commands set the difftoo and mergetool, you need also update ~/.gitconfig of cmd path
git config --global diff.tool tkdiff
git config --global merge.tool tkdiff

## this will disable the annoying prompt
git config --global --add difftool.prompt false
```

## [double dash for escaping path `--`](https://unix.stackexchange.com/questions/11376/what-does-double-dash-mean-also-known-as-bare-double-dash)

Double dash is used in shell to signify the end of parameter list (or command options). e.g, if you want grep a `-v` string, which normally a command option, you can do this: `grep -- -v file`

(from `git log help`) Paths may need to be prefixed with `--` to separate them from options or the revision range, when confusion arises.
```
# suppose you have a file named master
git checkout master # will checkout branch master
git checkout -- master # will checkout the file named master
```

## find commits delete files/lines and revert
The `--full-history` is very important, since [git may simplify the history during merge](https://stackoverflow.com/questions/6839398/find-when-a-file-was-deleted-in-git/34755406#34755406?newreg=5e9d8a8d262b4bf287e9997a4eefada6).
```
# if you know file path before it was deleted
git log --full-history -- file_path

# or 
git rev-list --full-history -n 1 HEAD -- file_path
git checkout deleting_commit^ -- file_path

#or in one command, assume $file == file_path
git checkout $(git rev-list -n 1 HEAD -- "$file")^ -- "$file"
```

# Term
* `upstream/downstream`: There is no absolute upstream/downstream. When you declared **otherRepo** as a remote one, then you are **pulling from upstream - otherRepo**, and you are **downstream for otherRepo**; you are **pushing to upstream - otherRepo**.

# Reference
* [Git workflow by GitLab](https://about.gitlab.com/2014/09/29/gitlab-flow/)
* [Another git ref](https://gist.github.com/hofmannsven/6814451)
