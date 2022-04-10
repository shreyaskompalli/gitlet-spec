---
title: add
parent: The Commands
has_children: false
nav_order: 2
---

## add

- __Usage__: `java gitlet.Main add [file name]`

- __Description__: Adds a copy of the file as it currently exists to
  the _staging area_ (see the description of the `commit`
  command).  For this reason, adding a file is also
  called _staging_ the file _for addition_.
  Staging an already-staged file overwrites the previous entry
  in the staging area with the new contents.
  The staging area should be somewhere in
  `.gitlet`. If the current working version of the file is identical to
  the version in the current commit, do not stage it to be added,
  and remove it from the staging area if it is already there (as
  can happen when a file is changed, added, and then changed back).
  The file will no longer be staged for removal (see `gitlet rm`), if it
  was at the time of the command.

- __Runtime__: In the worst case, should run in linear time relative
  to the size of the file being added and $\lg N$, for $N$ the
  number of files in the commit.

- __Failure cases__: If the file does not exist, print the error
  message `File does not exist.` and exit without changing
  anything.

- __Dangerous?__: No

- __Our line count__: ~20
