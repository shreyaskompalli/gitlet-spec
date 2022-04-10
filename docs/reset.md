---
title: reset
parent: The Commands
has_children: false
nav_order: 12
---

## reset

- __Usage__: `java gitlet.Main reset [commit id]`

- __Description__: Checks out all the files tracked by the given
  commit. Removes tracked files that are not present in that commit.
  Also moves the current branch's head to that commit node.
  See the intro for an example of what happens to the head pointer
  after using reset.  The `[commit id]` may be abbreviated as for
  `checkout`.  The staging area is cleared.  The command is essentially
  `checkout` of an arbitrary commit that also changes the current branch
  head.

- __Runtime__: Should be linear with respect to the total size of
  files tracked by the given commit's snapshot. Should be constant
  with respect to any measure involving number of commits.

- __Failure case__: If no commit with the given id exists, print `No
  commit with that id exists.`  If a working file is untracked in the current
  branch and would be overwritten by the reset, print
  `There is an untracked file in the way; delete it, or add and commit it
  first.` and exit;
  perform this check before doing anything else.

- __Dangerous?__: Yes!

- __Differences from real git__: This command is
  closest to using the `--hard` option, as in `git reset --hard [commit
  hash]`.

- __Our line count__: ~10
