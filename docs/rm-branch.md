---
title: rm-branch
parent: The Commands
has_children: false
nav_order: 11
---

## rm-branch

- __Usage__: `java gitlet.Main rm-branch [branch name]`

- __Description__: Deletes the branch with the given name. This only
  means to delete the pointer associated with the branch; it does not
  mean to delete all commits that were created under the branch, or
  anything like that.

- __Runtime__: Should be constant relative to any significant measure.

- __Failure cases__: If a branch with the given name does not exist,
  aborts. Print the error message `A branch with that name does not
  exist.` If you try to remove the branch you're currently on, aborts,
  printing the error message `Cannot remove the current branch.`

- __Dangerous?__: No

- __Our line count__: ~15
