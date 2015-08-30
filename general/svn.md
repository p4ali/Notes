# svn vs Perforce
* No locking in svn
* no .svn folder in Perforce
* svn open source, Perforce is proprietary

# A SVN repository 
From data structure point of view, a SVN repository is an array of revisions, each revision is a tree represent the file structure of the project. Project is the basic unit of work for svn.
```
For a repository structure:
root
|-B
|-A
  |-fish
     |- tuna.txt

The following is the data in repository

1                         2                 3   ...
DNode(A,B)
|         \
DNode()  DNode(fish)
           \
           DNode(tuna.txt)
            \
            FNode(contents of tuna.txt)
```

# Revision
A snapshot of what currently in the respository, including files and folders. Each time the repository accepts a commit, it
creates a new state of the filesystem tree, called a revision. Each revision is assigned a global unique (including trunks 
and branches) natural number, one greater than the number assigned to the previous revision. The initial revision is 0 and
consists of nothing but an empty root directory.

There is a subtle difference between Perforce and SVN: when svn user talks about 'revision 5 of foo.c', they really mean 
'foo.c as it appears in revision 5'. But in Perforce, it means 'the 5th version of file foo.c'. The revision in svn is 
similar to changelist in Perforce.

## Revision Keywords
* HEAD - the latest(or 'yougest') revision in the repository
* BASE - the revision number of an item in a working copy. It's the pristine copy of the item after checkout or update.
* COMMITTED - the most recent revision prior to, or equal to, BASE, in which an item changed.
* PREV - the revision immediatedly before the last revision in which an item changed. i.e., COMMITTED-1.
```
svn log -r HEAD
svn diff -r BASE:HEAD foo.c
svn log -r BASE:HEAD
svn update -r PREV foo.c # revides the last change on foo.c, decreasing foo.c's working revision
svn log -r {2006-01-01}:{2016-01-01}
```

# Cheap copies
Using similar technology([Bubble-Up method](http://svn.apache.org/repos/asf/subversion/trunk/notes/subversion-design.html#server.fs.struct.bubble-up)) to the linux *hard link*, a branch is a new copy of original source with hard links, and only update to
a new file if it's necessary(e.g., the content is changed, or moved).

# [.svn directory](https://pythonconquerstheuniverse.wordpress.com/2011/03/16/learning-subversion-the-mystery-of-svn-2/) - adminstrative directory

The directory contains meta-data and pristine copies of you working files locally.
* only `svn checkout` will generate .svn directory for each folder under working copy
* export will create a clean copy without .svn
* `svn update` will fetch the new version from remote repo to .svn folder, and then attempt to merge changes.
* A workingCopy directory is a directory that has a hidden subdirectory called `.svn`
* If you accidentally delete .svn folder, do a fresh check out for that folder to some where else and then copy .svn back to your workingCopy.

