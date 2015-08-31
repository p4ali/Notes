# Git data model
* blob: file content, identified by a hash
* tree: List of pointers to blob, or tree, identified by a hash
* commit: references the (root) tree + metadata, 0 to n parent commits, identified by a (sha-1) hash (author may not equal to commiter). DAG
* tag: name associated with a commit (+potential metadata).

# Git Workflow
This original from [atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/)

## Centralized workflow
Similiar to SVN, small groups of developers working on remote `master` branch with git.

## Feature branch wrokflow
Dedicate a branch for each feature development. 
* This can leverage `pull request`. Before merge into master, developer push their 
  branch to central server, and file a pull request asking. This gives other developers
  and opportunity to code review the changes before they become a part of the master.
* each branch can be pushed to remote, which can be used as local backup: 
```
git checkout -b new-feature master
# work hard...
git push -u origin new-feature
# file pull-request

# colleague review and make changes, pull and commit and push
# colleague file a pull-request

# I pull and commit and push
# done...

# project manager merge new_feature to master:
git checkout master
git pull
git pull origin marys-feature
git push
```

## Gitflow workflow
This similar to feature branch flow. However, it assign different roles to branches, and how and when they should interact. 
It's like Perforce's dev/main/preparing/releas branches. It use individual branches for preparing, maintaining, and recording
releases.

## Foking workflow
There are multiple central codebases in this workflow. Every developer has a server-side repository, i.e., eache developer has
TWO git repos: a private local one and a public server-side one. This is ideal for open source projects.
* There is an official repo from beginning
* A developer fork a new repo on server
* developer clone a local of new repo, and commits, push to the remote new repo
* Project manager can pull from new repo, and merge changes to official repo

# [`bare` vs `non-bare` repos](http://bitflop.com/tutorials/git-bare-vs-non-bare-repositories.html)
* `bare` repo allows pull and push. `git --bare init`
* `non-bare` repo allows only pull, but NOT push. `git init`
* To convert from `bare` to `non-bare` repo: `git clone bare_repo non_bare_repo`, then delete `bare` repo.
* To convert from a `non-bare` repo to a `bare` repo: `git clone --bare -l non_bare_repo new_bare_repo`. 

# Git for CI
* master is production - promoted from staging, also can add hotfix directly to master, then need merge back to staging
* staging is the next version 
* new feature off staging, named like: username/ISSUE-KEY-summary
* buildoing everything is expensive
* automatically build `stable` and `master`
* manually trigger feature branch builds
 
# Git for production release
Merge can be done automatically in case of from firmer to softer. `git merge --strategy=ours`
* release branches (firmer)
 * spin-off bugfix branch(soft) 
* one central repo master (firm)
* one branch per feature (soft)
* one branch per bugfix (soft)
* Merge continously from firmer to soft, e.g., from release to master
* Copy from soft to firm with git cherry-pick.

# Hooks
Hooks are little scripts you can place in the `.git/hooks` directory to trigger action at certain points.

|Local              |Remote            |
|-------------------|------------------|
|pre/post-applypatch|pre-receive       |
|pre/post-commit    |update            |
|pre-rebase         |post-receive      |
|post-checkout      |post-update       |
|post-merge         |                  |
|pre-push           |                  |

# `git bisect`
Search bug by dichotomy(binary search). The idea is b-search in between a range of commits, and find the bug commit.

# aggregate multiple repos
```
git init repo1 && cd repo1 && git commit -m "Initial 1" --allow-empty && cd ..
git init repo2 && cd repo2 && git commit -m "Initial 2" --allow-empty
git remote add other file://$PWD/../repo1
git fetch other
git merge other/master -m "let's merge them"

git log --oneline --graph
*   c2be901 let's merge them
|\
| * 1763514 Initial 1
* 8208b0a Initial 2
```

# split repo to multiple repos
`filter-branch` will help

```
Tell me and I forget, teach me and I may remember, involve me and I learn.
- Benjamin Franklin
```
