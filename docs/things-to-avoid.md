---
title: Things To Avoid
has_children: false
nav_order: 13
---

Things to Avoid
----

There are few practices that experience has shown will cause you endless grief
in the form of programs that don't work and bugs that are very hard to find
and sometimes not repeatable ("Heisenbugs").

1. Since you are likely to keep various information in files (such as commits),
   you might be tempted to use apparently convenient file-system operations
   (such as listing a directory) to sequence through all of them.  Be
   careful.  Methods such as `File.list` and `File.listFiles` produce file
   names in an undefined order.  If you use them to implement the `log`
   command, in particular, you can get random results.

2. Windows users especially should beware that the file separator character is
   `/` on Unix (or MacOS) and `\ ` on Windows.  So if you form file names in
   your program by concatenating some directory names and a file name together
   with explicit `/`s or `\ `s, you can be sure that it won't work on one
   system or the other.  Java provides a system-dependent file separator
   character `File.separator`, as in `".gitlet" + File.separator + "something"`,
   or the multi-argument constructors to `File`, as in \
   `new File(".gitlet", "something")`, which you can use in place of
   `".gitlet/something"`).

3. **Be careful using a `HashMap` when serializing!** The order of things within the
   `HashMap` is non-deterministic. The solution is to use a `TreeMap` which will
   always have the same order. More details [here](https://stackoverflow.com/questions/5993752/hashmap-serialization-and-deserialization-changes).
   Specifically, iterating through a `HashMap` can lead to non-deterministic behavior, which is avoided
   with a TreeMap.
