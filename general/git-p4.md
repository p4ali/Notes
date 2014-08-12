
Let's talk about rebase.

One of the most common questions coming into the DVCS project is:

        Will there be a 'p4 rebase'?

I'm hoping that the answer to that is "no".

The behavior of the 'git rebase' command is very tied to the internal
git data model, and providing a command which has the same name,
but dramatically different behavior, will lead to confusion and frustration.

A more useful approach is to look at what people do with 'git rebase',
and talk about how people can do that with Perforce.

Sadly, 'git rebase' is the Swiss Army Knife of git; it is used for many
wildly different purposes, in many wildly different ways.

So this is not a small topic.

Executive Summary:
==================

- git rebase can do MANY things

- So can the 2015.1 version of Perforce

- which way is better? git or Perforce? emacs or vi? Windows or Linux?
   iPhone or Android? Such arguments are not productive.

- Instead: what, SPECIFICALLY, can we do to improve the 2015.1 version
   of Perforce to make its capabilities stronger in this area?

Merge Down:
===========

The easiest and simplest use of git rebase is to merge changes down from
the parent branch. Here's how it's described in the git documentation
(at http://git-scm.com/docs/git-rebase):

     Assume the following history exists and the current branch is "topic":

           A---B---C topic
          /
     D---E---F---G master

     From this point, the result of either of the following commands:

     git rebase master
     git rebase master topic

     would be:

                   A'--B'--C' topic
                  /
     D---E---F---G master

This usage is quite simple in Perforce, for the most part:

     p4 merge -b topic
     p4 resolve -am
     p4 submit

The actual history that you get in Perforce is slightly different than
in git, because we don't erase the old history, so our history looks like:

           A---B---C---G' topic
          /           /
     D---E-----F-----G master

But the effect is the same: changes F and G from the master branch have
now been included into the topic branch.

Also, note that, just like git, Perforce handles the situation where one
of the changes in the topic branch was cherry-picked back to the master
branch already (as well as many other branch merging scenarios).

So I don't see that we necessarily need to make any changes here.

Branch Transplantation:
=======================

The git rebase command can be used for various 'reparenting' scenarios.

Here's an example, from the git documentation:

     Here is how you would transplant a topic branch based on one branch
     to another, to pretend that you forked the topic branch from the
     latter branch, using rebase --onto .

     First let’s assume your topic is based on branch next. For example,
     a feature developed in topic depends on some functionality which is
     found in next.

     o---o---o---o---o  master
          \
           o---o---o---o---o  next
                            \
                             o---o---o  topic

     We want to make topic forked from branch master; for example,
     because the functionality on which topic depends was merged into
     the more stable master branch. We want our tree to look like this:

     o---o---o---o---o  master
         |            \
         |             o'--o'--o'  topic
          \
           o---o---o---o---o  next

     We can get this using the following command:

     git rebase --onto master next topic

I believe that most Perforce users don't think about this problem this way.

This is because git maintains a STRONG relationship between a branch and
its parent ("upstream"), while in Perforce the relationship is more fluid.

If my 'topic' branch was depending on some code in the 'next' branch,
and that code has now been integrated up to 'master', then I would
simply change my branchspec for 'topic' so that instead of integrating
from 'next' to 'topic', I now integrate from 'master' to 'topic'.

Again, our history will tend to look slightly different; we'll get
something more like:

     o---o---o---o---o  master
         |            \
         |        o'---o'--o'  topic
          \      /
           o---o---o---o---o  next

But the effect is still the same: topic used to contain changes relative
to next, but now contains changes relative to master.

Again, I don't see that we necessarily need to make any changes here.

Obliterating Unwanted Work:
===========================

The git rebase command can be used to destroy unwanted work.

Here's an example, from the git documentation:

     A range of commits could also be removed with rebase. If we have
     the following situation:

     E---F---G---H---I---J  topicA

     then the command

     git rebase --onto topicA~5 topicA~3 topicA

     would result in the removal of commits F and G:

     E---H'---I'---J'  topicA

     This is useful if F and G were flawed in some way, or should not
     be part of topicA.

I have to say, I find this example strange. In my own experience, if I
find that changelists F and G were "flawed in some way," I make a
subsequent submit that fixes the flaws.

If I make a change, then decide it "should not be part of" my project,
I make a subsequent submit that backs out that change.

But I understand that git users find this troubling, and would prefer to
remove any evidence that the change was ever made in the first place.

Here, I think that the 2015.1 'p4 unsubmit' feature could be used. I do:

        p4 unsubmit J
        p4 unsubmit I
        p4 unsubmit H
        p4 unsubmit G
        p4 unsubmit F
        p4 shelve -d -c F; p4 change -d -c F
        p4 shelve -d -c G; p4 change -d -c G
        p4 submit -e H
        p4 submit -e I
        p4 submit -e J

And poof! changes F and G have completely disappeared.

This is quite challenging if changes H, I, and J turn out to depend on
changes F and G, but that is true in git as well. As the git documentation
coyly states:

     In case of conflict, git rebase will stop at the first problematic
     commit and leave conflict markers in the tree. You can use git diff
     to locate the markers (<<<<<<) and make edits to resolve the conflict.         
(Oh, what a world of pain is summed up in those two terse sentences!)

But Perforce already contains equivalent functionality for diffing and
merging changes; I unshelve the changes, run 'p4 resolve', and then
I can 'p4 submit' the results (afterwards I clean up the old shelves).

So I'm not sure if we need to make any more changes here or not.

I think that we first need to get some hands-on experience with the
'p4 unsubmit' command and try to see if it is a viable way to destroy
unwanted work without losing the work you wish to keep.

Interactive Rebase:
===================

Interactive rebase is a world of complexity all its own. As the git
documentation notes:

     Rebasing interactively means that you have a chance to edit the
     commits which are rebased. You can reorder the commits, and you
     can remove them (weeding out bad or otherwise unwanted patches).

     The interactive mode is meant for this type of workflow:

     1. have a wonderful idea

     2. hack on the code

     3. prepare a series for submission

     4. submit

This is what most people mean by "use git rebase to rewrite history", I
believe; you can find examples of people praising its wonderfulness
all over the net, but the best description of why this is a valuable
feature can be found here:

        http://lwn.net/Articles/328438/

or, if you really want to educate yourself, here:

        http://yarchive.net/comp/linux/git_rebase.html

But of course read this, too:

        http://paul.stadig.name/2010/12/thou-shalt-not-lie-git-rebase-ammend.html

What Can You Do With Git Rebase:
================================

git rebase is a powerful tool, if used VERY carefully.

But rather than saying "Oh! Let's build 'p4 rebase -i'!", let's try to
break this down into something more useful, by looking at some of the
things that interactive rebase can do, and how you might do them in
Perforce.

Here are the basic things you can do with interactive rebase:

     By replacing the command "pick" with the command "edit", you can
     tell git rebase to stop after applying that commit, so that you
     can edit the files and/or the commit message, amend the commit,
     and continue rebasing.

     If you just want to edit the commit message for a commit, replace
     the command "pick" with the command "reword".

     If you want to fold two or more commits into one, replace the
     command "pick" for the second and subsequent commits with "squash"
     or "fixup". If the commits had different authors, the folded commit
     will be attributed to the author of the first commit. The suggested
     commit message for the folded commit is the concatenation of the
     commit messages of the first commit and of those with the "squash"
     command, but omits the commit messages of commits with the "fixup"
     command.

And here is how to do those things in Perforce:

Rewording Commit Log Messages:
==============================

This is much easier in Perforce; simply run 'p4 change -u'.

Squashing Commits:
==================

I can totally see the appeal to squashing commits. There are plenty of
obvious scenarios where this is beneficial, e.g.

1) I submitted a change but I forgot to 'p4 add' a file, so I added that
    and submitted it in a second change, but it should have been part of
    the first change.

2) I submitted a change but I left a debugging printf() in my code, so
    I submitted a second change which removed that printf, it should have
    been part of the first change.

3) I submitted a change but it had a horrible bug, so I fixed it, etc.

If you figure this out right away (which seems like the most common case),
'p4 unsubmit' handles this very nicely. As soon as you realize that the
change you just submitted had a problem, do 'p4 unsubmit', fix the
problem (i.e., 'p4 add' the missing file, remove the printf, fix the
horrible bug, etc.), and then 'p4 submit' to submit the correct changelist.

The trick, I think is when you don't figure this out right away, but only
sometime afterward.

You can still do it with 'p4 unsubmit', but it will be more complicated,
because you'll have to unsubmit all the changes that came AFTER the bad
change which touched the files in the bad change. That's a hassle.

So I could see that it might be nice to have a command like:

        p4 combinechanges -c 1407 -c 1408 -c 1411

which would do the following:
- find all the files touched by changes 1407 and 1408
- if those files are also touched by change 1411, obliterate them;
   else update them to make them part of change 1411
- include the change description from changelists 1407 and 1408 into 1411
- delete the empty changeslists 1407 and 1408

Splitting Commits:
==================

Splitting commits is a fascinating topic.

Again, I think the key question is whether you discover this right away
or not.

If you discover this right away, it's easy with 'p4 unsubmit':

        p4 unsubmit -c 791
        p4 unshelve -s 791
        ... edit the files to remove the unwanted changes, diff, etc...
        p4 submit
        p4 unshelve -s 791
        p4 sync
        p4 resolve
        p4 submit

The question is: how often do you want to split a commit, but you don't
notice that until you've done a lot more work, and you don't want to
unsubmit it all?

It would be really good to get some concrete scenarios here.

Reordering Commits:
===================

As the 'git rebase' doc says, you can reorder commits, but it's not clear
how well it works. In particular, in the "Bugs" section of the manual
page, the git documentation says:

     For example, an attempt to rearrange

     1 --- 2 --- 3 --- 4 --- 5

     to

     1 --- 2 --- 4 --- 3 --- 5

     by moving the "pick 4" line will result in the following history:

            3
            /
     1 --- 2 --- 4 --- 5

Two good reasons for reordering changes are:

1. In git, the "squash" functionality can only squash adjacent changes. So,
    if you want to squash two changes that aren't in consecutive order, you
    may need to reorder commits.

2. In git, the "push" command can't cherry-pick a commit to push from the
    middle of a branch. So if you want to push only part of a branch, you
    may need to reorder those commits to come at the front.

The Perforce equivalents of these features don't have those restrictions,
so we may not need the ability to reorder commits.

But for those situations where you DO wish to re-order commits, 'p4 unsubmit'
actually makes this rather easy:

        p4 unsubmit -c 5 -c 4 -c 3 -c 2 -c 1
        p4 submit -e 1
        p4 submit -e 2
        p4 submit -e 4
        p4 submit -e 3
        p4 submit -c 5

This of course assumes that changes 3 and 4 have no interdependencies, of
course; if they do, you'll have to unshelve, resolve, etc.

Anyway, after doing a bunch of research and trying a bunch of tests, I've
come to believe that we can already do many of the things that you can
do with git rebase, although the mechanics are different and the interface
is dramatically different.

So hopefully this will encourage people to move past the surface-level
discussion and have a more detailed discussion about the pros and cons
of accomplishing certain tasks in Perforce vs git.

thanks,

bryan
