---
title: checkout
parent: The Commands
has_children: false
nav_order: 9
---

## checkout

Checkout is a general command that can do a few different
things depending on what its arguments are. There are 3 possible use
cases. In each section below, you'll see 3 bullet points. Each
corresponds to the respective usage of checkout.

- __Usages__:

    1. `java gitlet.Main checkout -- [file name]`

    2. `java gitlet.Main checkout [commit id] -- [file name]`

    3. `java gitlet.Main checkout [branch name]`

- __Descriptions__:

    1. Takes the version of the file as it exists in the head commit,
      the front of the current branch, and puts it in the working directory,
      overwriting the version of the file that's already there if there is
      one.  The new version of the file is not staged.

    2. Takes the version of the file as it exists in the commit with
      the given id, and puts it in the working directory, overwriting
      the version of the file that's already there if there is one.
      The new version of the file is not staged.

    3. Takes all files in the commit at the head of the given branch,
      and puts them in the working directory, overwriting the versions
      of the files that are already there if they exist. Also, at the
      end of this command, the given branch will now be considered the
      current branch (HEAD).  Any files that are tracked in the current
      branch but are not present in the checked-out branch are deleted.
      The staging area is cleared, unless the checked-out branch is the
      current branch (see __Failure cases__ below).

- __Run times__:

    1. Should be linear relative to the size of the file being checked out.

    2. Should be linear with respect to the total size of the files in
      the commit's snapshot. Should be constant with respect to any
      measure involving number of commits. Should be constant with
      respect to the number of branches.

- __Failure cases__:

    1. If the file does not exist in the previous commit, abort,
      printing the error message `File does not exist in that
      commit.`

    2. If no commit with the given id exists, print `No commit with
      that id exists.` Otherwise, if the file does not exist in the given
      commit, print the same message as for failure case 1.

    3. If no branch with that name exists, print `No such branch exists.`
       If that branch is the current branch, print `No need to checkout the
      current branch.`  If a working file is untracked in the current
      branch and would be overwritten by the checkout, print
      `There is an untracked file in the way; delete it, or add and commit it first.`
      and exit; perform this check before doing anything else.

- __Differences from real git__: Real git does not clear the staging area
  and stages the file that is checked out.
  Also, it won't do a checkout that would overwrite or undo changes (additions
  or removals) that you have staged.

A [commit id] is, as described earlier, a hexadecimal numeral.  A convenient
feature of real Git is that one can abbreviate commits with a unique
prefix.  For example, one might abbreviate
    a0da1ea5a15ab613bf9961fd86f010cf74c7ee48
as
    a0da1e
in the (likely) event that no other object exists with a SHA-1 identifier that
starts with the same six digits.  You should arrange for the same thing to
happen for commit ids that contain fewer than 40 characters.  Unfortunately,
using shortened ids might slow down the finding of objects if implemented
naively (making the time to find a file linear in the number of objects), so
we won't worry about timing for commands that use shortened ids.  We suggest,
however, that you poke around in a `.git` directory (specifically,
`.git/objects`) and see how it manages to speed up its search. You will perhaps
recognize a familiar data structure implemented with the file system rather
than pointers.

Only version 3 (checkout of a full branch) modifies the staging area:
otherwise files scheduled for
addition or removal remain so.

- __Dangerous?__: Yes!

- __Our line counts__:

    - ~15
    - ~5
    - ~15
