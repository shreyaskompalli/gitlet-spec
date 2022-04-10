---
title: find
parent: The Commands
has_children: false
nav_order: 7
---

## find

- __Usage__: `java gitlet.Main find [commit message]`

- __Description__: Prints out the ids of all commits that have the given
  commit message, one per line.
  If there are multiple such commits, it prints the
  ids out on separate lines.  The commit message is a single operand; to
  indicate a multiword message, put the operand in quotation marks, as for
  the `commit` command above.

- __Runtime__: Should be linear relative to the number of commits.

- __Failure cases__: If no such commit exists, prints the error
  message `Found no commit with that message.`

- __Dangerous?__: No

- __Differences from real git__: Doesn't exist in real git. Similar
  effects can be achieved by grepping the output of log.

- __Our line count__: ~15
