---
title: rm
parent: The Commands
has_children: false
nav_order: 4
---

## rm

- __Usage__: `java gitlet.Main rm [file name]`

- __Description__: Unstage the file if it is currently staged for addition.
  If the file is tracked in the current commit, stage it for removal and
  remove the file from the working directory if the user has not already
  done so (do _not_ remove it unless
  it is tracked in the current commit).

- __Runtime__: Should run in constant time relative to any significant measure.

- __Failure cases__: If the file is neither staged nor tracked by the
  head commit, print the error message `No reason to remove the file.`

- __Dangerous?__: Yes (although if you use our utility methods, you will only
  hurt your repository files, and not all the other files in your
  directory.)

- __Our line count__: ~20
