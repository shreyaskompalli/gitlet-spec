---
title: status
parent: The Commands
has_children: false
nav_order: 8
---

## status

- __Usage__: `java gitlet.Main status`

- __Description__: Displays what branches currently exist, and marks
  the current branch with a `*`. Also displays what files have been
  staged for addition or removal. An example of the _exact_ format it
  should follow is as follows.

        === Branches ===
        *master
        other-branch

        === Staged Files ===
        wug.txt
        wug2.txt

        === Removed Files ===
        goodbye.txt

        === Modifications Not Staged For Commit ===
        junk.txt (deleted)
        wug3.txt (modified)

        === Untracked Files ===
        random.stuff

  There is an empty line between sections. Entries should be listed in
  lexicographic order, using the Java string-comparison order (the asterisk
  doesn't count).  A file in the working directory
  is "modified but not staged" if it is

  - Tracked in the current commit, changed in the working directory,
    but not staged; or
  - Staged for addition, but with different contents than in the working
    directory; or
  - Staged for addition, but deleted in the working directory; or
  - Not staged for removal, but tracked in the current commit and deleted from
    the working directory.

  The final category ("Untracked Files") is for files present in the working
  directory but neither staged for addition nor tracked.  This includes files
  that have been staged for removal, but then re-created without Gitlet's
  knowledge. Ignore any subdirectories
  that may have been
  introduced, since Gitlet does not deal with them.

  The last two sections (modifications not staged and untracked files) are
  extra credit, worth 1 point. Feel free to leave them blank (leaving
  just the headers).

- __Runtime__: Make sure this depends only on the amount of data in the
  working directory plus the number of files staged to be added or deleted
  plus the number of branches.

- __Failure cases__: None

- __Dangerous?__: No

- __Our line count__: ~45
