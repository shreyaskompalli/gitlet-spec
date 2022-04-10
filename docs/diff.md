---
title: Diffs
parent: Extra Credit
has_children: false
nav_order: 2
---

Diffs
----

This is the second of two possible extra-credit features.  Again, we will
only count one of them.

__Usage__: `java gitlet.Diff [ branch-name [ branch-name ] ]`

__Description__:
The `git diff` command compares the contents of a commit with a
working directory or compares two commits, and presents any
differences as a _unified diff_.  Suppose that the latest commit
on the current branch contains the following three files:

        f.txt              g.txt              h.txt
        ---------------------------------------------------------
        Line 1.            This is a wug.     This is not a wug.
        Line 2.            
        Line 3.            
        Line 4.            
        Line 5.            
        Line 6.            
        Line 7.            
        Line 8.            
        Line 9.            
        Line 10.           
        Line 11.           
        Line 12.           
        Line 13.           
        Line 14.           
        Line 15.           
        Line 16.           
        Line 17.           



and suppose that the working directory contains the following two
files:

        f.txt              g.txt       
        -------------------------------
        Line 0.            This is a wug.
        Line 0.1.  
        Line 1.    
        Line 3.    
        Line 4.    
        Line 7.    
        Line 8.    
        Line 9.    
        Line 9.1.  
        Line 9.2.  
        Line 10.   
        Line 11.   
        Line 11.1.
        Line 12.   
        Line 13.1  
        Line 14.   
        Line 15.   
        Line 16.1  
        Line 17.1  
        Line 18.   

Performing the command `java gitlet.Main diff` should produce the
following output:

        diff --git a/f.txt b/f.txt
        --- a/f.txt
        +++ b/f.txt
        @@ -0,0 +1,2 @@
        +Line 0.
        +Line 0.1.
        @@ -2 +3,0 @@
        -Line 2.
        @@ -5,2 +5,0 @@
        -Line 5.
        -Line 6.
        @@ -9,0 +9,2 @@
        +Line 9.1.
        +Line 9.2.
        @@ -11,0 +13 @@
        +Line 11.1.
        @@ -13 +15 @@
        -Line 13.
        +Line 13.1
        @@ -16,2 +18,3 @@
        -Line 16.
        -Line 17.
        +Line 16.1
        +Line 17.1
        +Line 18.
        diff --git a/h.txt /dev/null
        --- a/h.txt
        +++ /dev/null
        @@ -1 +0,0 @@
        -This is not a wug.

The two lines starting `diff --git` indicate the start of the
differences for one of the files in the two versions (the head of the
current branch and the current (uncommitted) contents of the working directory.
These `diff` lines are followed by a `---` and `+++` line, also giving the
file name.  Next come a sequence of _edits_, each starting with a line
beginning and ending with `@@`.  The entry

         @@ -L1,N1 +L2,N2

says that this entry indicates that to get the second version of the
file from the first, one removes N1 lines starting at line L1 of the
first version, and inserts N2 lines from the second version starting
with line L2 of the second version.  When N1 is 0, L1 is the number of
lines before the lines to be deleted.  Likewise, when N2 is 0, L2 is
the number of lines in the second version that correspond to the lines
preceding line L1 in the first version.  When N1 or N2 is 1, it (and
the comma in front of it) is not printed (e.g., `@@ -13 +15 @@` in the
example.)  When one of the versions of the file is missing (as for
`h.txt` here), its name is printed as `/dev/null` (the standard Unix
empty file), and it is in fact treated as an empty file when
comparing.   If both versions of a file are identical, no diff is
given for that file.

With no arguments, we compare the commit at the head of the current
branch with the files in the working directory.  Any files in the
working directory and not in the current branch are ignored.

With one argument, as in `git diff branch1`, the contents of the
commit at the head of the branch named `branch1` are compared to the
versions in the working directory.

With two arguments, as in `git diff branch1 branch2`, the contents of
the commit at the head of the branch named `branch1` are compared with
those in `branch2`.  It is only in this case, by the way, that the
first (`a/`) file can be `/dev/null`.

__Failure cases__: If the branch in the one-argument case does not
exist, print the message `A branch with that name does not exist.`
If one or both of the branches in the two-argument case does not
exist, print the message `At least one branch does not exist.`

__Dangerous?__: No.

### Diff Utility

We have provided a file `gitlet/Diff.java` in the skeleton, which will compare
two sequences of lines read from files, and compute the edits needed
to convert one to the other.  You will have to read its comments and
figure out how to use it to produce the desired output format.

This utility works by finding a _longest common subsequence_ of lines in the
two files<--->a subsequence that appears in both files, but not necesaarily
in the same place.  The lines of the subsequence may be scattered in
different ways in the two files (they are not necessarily adjacent to each
other), but the lines appear in the same order in each.
From such a subsequence, the utility is able to figure out how to
change one file into the other by modifying a minimal number of lines.
