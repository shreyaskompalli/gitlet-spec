---
title: Detailed Overview of Behavior
has_children: false
nav_order: 5
---

Detailed Spec of Behavior
----

The only structure requirement we're giving you is that you have a
class named `gitlet.Main` and that it has a main method. Here's your skeleton
code for this project (in package Gitlet):

    public class Main {
        public static void main(String[] args) {
           // FILL IN
        }
    }

We are also giving you some utility methods for performing a number of
mostly file-system-related tasks, so that you can concentrate on the logic
of the project rather than the peculiarities of dealing with the OS.

You may, of course, write additional Java classes to support your
project -- in fact, please do. But don't use any external code (aside
from JUnit), and don't use any programming language other than Java.
You can use all of the Java Standard Library that you wish, plus utilities we
provide.

The majority of this spec will describe how `Main.java`'s main
method must react when it receives various Gitlet commands as
command-line arguments. But before we break down command-by-command,
here are some overall guidelines the whole project should
satisfy:

- In order for Gitlet to work, it will need a place to store old
  copies of files and other
  metadata. All of this stuff __must__ be stored in a directory called
  `.gitlet`, just as this information is stored in directory `.git` for the
  real git system (files with a `.` in front are hidden files. You will
  not be able to see them by default on most operating systems.  On Unix,
  the command `ls -a` will show them.) A
  Gitlet system is considered "initialized" in a particular location if
  it has a `.gitlet` directory there. Most Gitlet commands (except for the
  `init`  command) only need to work when used from a directory where a
  Gitlet system has been initialized<--->i.e. a directory that has a
  `.gitlet` directory. The files that _aren't_ in your `.gitlet`
  directory (which are copies of files from the repository that you are
  using and editing, as well as files you plan to add to the repository) are
  referred to as the files in your _working directory_.

- Most commands have runtime or memory usage requirements. You must
  follow these. Some of the runtimes are described as constant
  "relative to any significant measure". The significant measures are:
  any measure of number or size of files, any measure of number of
  commits. You can ignore time required to serialize or deserialize,
  _with the one caveat that your serialization time cannot depend in
  any way on the total size of files that have been added, committed,
  etc_ (what is serialization? You'll see later in the spec). You can
  also assume that getting from a hash table is constant time.

- Some commands have failure cases with a specified error message. The
  exact formats of these are specified later in the spec. All error
  message end with a period; since our autograding is literal, be
  sure to include it. If your
  program ever encounters one of these failure cases, it must print
  the error message and not change anything else. _You don't need to
  handle any other error cases except the ones listed as failure
  cases_.

- There are some failure cases you need to handle that don't apply to
  a particular command. Here they are:

    - If a user doesn't input any arguments, print the message
      `Please enter a command.` and exit.

    - If a user inputs a command that doesn't exist, print the
      message `No command with that name exists.` and exit.

    - If a user inputs a command with the wrong number or format of
      operands, print the message `Incorrect operands.` and exit.

    - If a user inputs a command that requires being in an initialized
      Gitlet working directory (i.e., one containing a `.gitlet` subdirectory),
      but is not in such a directory, print the message `Not in an initialized
      Gitlet directory.`

- Some of the commands have their differences from real Git
  listed. The spec is not exhaustive in listing _all_ differences from
  git, but it does list some of the bigger or potentially confusing
  and misleading ones.

- Do __NOT__ print out anything except for what the spec says. Some of
  our autograder tests will break if you print anything more than
  necessary.

- Always exit with exit code 0, even in the presence of errors.  This allows
  us to use other exit codes as an indication that something blew up.

- The spec classifies some commands as "dangerous". Dangerous commands
  are ones that potentially overwrite files (that aren't just
  metadata) -- for example, if a user tells Gitlet to restore files to
  older versions, Gitlet may overwrite the current versions of the
  files. Just FYI.
