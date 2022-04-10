---
title: Going Remote
parent: Extra Credit
has_children: false
nav_order: 1
---

Going Remote
----

This project is all about mimicking git's local features. These are
useful because they allow you to backup your own files and maintain
multiple versions of them. However, git's true power is really in its
_remote_ features, allowing collaboration with other people over the
internet. The point is that both you and your friend could be
collaborating on a single code base. If you make changes to the files,
you can send them to your friend, and vice versa. And you'll both have
access to a shared history of all the changes either of you have made.

To get extra credit, implement some basic remote commands:
namely `add-remote`, `rm-remote`, `push`, `fetch`, and `pull`
You will get 4 extra-credit points for completing them.
Don't attempt or plan for extra credit until you have completed the
rest of the project.

Depending on how flexibly you have designed the rest of the project,
4 extra-credit points
may not be worth the amount of effort it takes to do this section.
We're certainly not expecting everyone to do it.
Our priority will be in helping students complete the main project;
if you're doing the
extra credit, we expect you to be able to stand on your own a little
bit more than most students.

## The Remote Commands

A few notes about the remote commands:

- Execution time will not be graded. For your own edification, please don't
  do anything ridiculous, though.

- All the commands are significantly simplified from their git
  equivalents, so specific differences from git are usually not
  notated. Be aware they are there, however.

So now let's go over the commands:

### add-remote

- __Usage__: `java gitlet.Main add-remote [remote name] [name of remote directory]/.gitlet

- __Description__: Saves the given login information under the given
  remote name. Attempts to push or pull from the given remote name
  will then attempt to use this `.gitlet` directory.
  By writing, e.g.,
      java gitlet.Main add-remote other ../testing/otherdir/.gitlet
  you can provide tests of remotes that will work from all
  locations (on your home machine or within the grading program's software).
  Always use forward slashes in these commands. Have your program convert
  all the forward slashes into the path separator character (forward slash on
  Unix and backslash on Windows).  Java helpfully defines the class variable
  `java.io.File.separator` as this character.

- __Failure cases__: If a remote with the given name already exists,
  print the error message: `A remote with that name already exists.`
  You don't have to check if the user name and server
  information are legit.

- __Dangerous?__: No.

### rm-remote

- __Usage__: `java gitlet.Main rm-remote [remote name]`

- __Description__: Remove information associated with the given remote
  name. The idea here is that if you ever wanted to change a remote
  that you added, you would have to first remove it and then re-add
  it.

- __Failure cases__: If a remote with the
  given name does not exist, print the error message: `A remote with
  that name does not exist.`

- __Dangerous?__: No.

### push

- __Usage__: `java gitlet.Main push [remote name] [remote branch name]`

- __Description__: Attempts to append the current branch's commits to
  the end of the given branch at the given remote. Details:

  This command only works if the remote branch's head is in the
  history of the current local head, which means that the local
  branch contains some commits in the future of the remote branch.
  In this case, append the future commits to the remote branch.
  Then, the remote should reset to the front of the appended
  commits (so its head will be the same as the local head). This
  is called fast-forwarding.

  If the Gitlet system on the remote machine exists but does not
  have the input branch, then simply add the branch to the remote
  Gitlet.

- __Failure cases__: If the remote branch's head is not in the history
  of the current local head, print the error message `Please pull down
  remote changes before pushing.`  If the remote `.gitlet` directory does not
  exist, print `Remote directory not found.`

- __Dangerous?__: No.

### fetch

- __Usage__: `java gitlet.Main fetch [remote name] [remote branch name]`

- __Description__: Brings down commits from the remote Gitlet repository into
  the local Gitlet repository. Basically, this copies all commits and
  blobs from the given
  branch in the remote repository (that are not already in the current
  repository) into a branch named `[remote name]/[remote branch name]` in the
  local `.gitlet` (just
  as in real Git), changing `[remote name]/[remote branch name]` to point
  to the head commit (thus copying the contents of the branch from the remote
  repository to the current one).  This branch is created in the local
  repository if it did not previously exist.

- __Failure cases__: If the remote Gitlet repository does not have the given
  branch name, print the error message `That remote does not have that
  branch.`  If the remote `.gitlet` directory does not
  exist, print `Remote directory not found.`
  <!-- Need failure case for remote name not defined. -->

- __Dangerous?__ No

### pull

- __Usage__: `java gitlet.Main pull [remote name] [remote branch name]`

- __Description__: Fetches branch `[remote name]/[remote branch name]` as
  for the `fetch` command, and then merges that fetch into the current branch.

- __Failure cases__: Just the failure cases of `fetch` and `merge`
  together.

- __Dangerous?__ Yes!
