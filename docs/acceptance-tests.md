---
title: Understanding Acceptance Tests
has_children: true
nav_order: 9
---

Understanding Acceptance Tests
-------------------------------

The first thing we'll ask for in Gitbugs and when you come to receive help in
Office Hours is a test that you're failing, so it's paramount that you learn to
write tests in this project.  We've done a lot of work to make this as painless
as possible, so please take the time to read through this section so you can
understand the provided tests and write good tests yourself.

The provided tests are hardly comprehensive, and you'll definitely need to write
your own tests to get a full score on the project. To write a test, let's first
understand how this all works.

Here is the structure of the `testing` directory:

    .
    ├── Makefile
    ├── student_tests                    <==== Your .in files will go here
    ├── samples                          <==== Sample .in files we provide
    │   ├── test01-init.in               <==== An example test
    │   ├── test02-basic-checkout.in
    │   ├── test03-basic-log.in
    │   ├── test04-prev-checkout.in
    │   └── definitions.inc
    ├── src                              <==== Contains files used for testing
    │   ├── notwug.txt
    │   └── wug.txt
    ├── runner.py                        <==== Script to help debug your program
    └── tester.py                        <==== Script that tests your program

Just like Capers, these tests work by creating a temporary directory within the
`testing` directory and running the commands specified by a `.in` file. If you
use the `--keep` flag, this temporary directory will remain after the test
finishes so you can inspect it.

Unlike Capers, we'll need to deal with the _contents_ of files in our working
directory. So in this `testing` folder, we have an additional folder called
`src`. This directory stores many pre-filled `.txt` files that have particular
contents we need. We'll come back to this later, but for now just know that
`src` stores actual file contents. `samples` has the `.in` files of the
sample tests (which are the checkpoint tests). When you create your own tests,
you should add them to the `student_tests` folder which is initially empty in
the skeleton.

The `.in` files have more functions in Gitlet. Here is the explanation straight
from the `tester.py` file:

    # ...  A comment, producing no effect.
    I FILE Include.  Replace this statement with the contents of FILE,
          interpreted relative to the directory containing the .in file.
    C DIR  Create, if necessary, and switch to a subdirectory named DIR under
          the main directory for this test.  If DIR is missing, changes
          back to the default directory.  This command is principally
          intended to let you set up remote repositories.
    T N    Set the timeout for gitlet commands in the rest of this test to N
          seconds.
    + NAME F
          Copy the contents of src/F into a file named NAME.
    - NAME
          Delete the file named NAME.
    > COMMAND OPERANDS
    LINE1
    LINE2
    ...
    <<<
          Run gitlet.Main with COMMAND ARGUMENTS as its parameters.  Compare
          its output with LINE1, LINE2, etc., reporting an error if there is
          "sufficient" discrepancy.  The <<< delimiter may be followed by
          an asterisk (*), in which case, the preceding lines are treated as
          Python regular expressions and matched accordingly. The directory
          or JAR file containing the gitlet.Main program is assumed to be
          in directory DIR specified by --progdir (default is ..).
    = NAME F
          Check that the file named NAME is identical to src/F, and report an
          error if not.
    * NAME
          Check that the file NAME does not exist, and report an error if it
          does.
    E NAME
          Check that file or directory NAME exists, and report an error if it
          does not.
    D VAR "VALUE"
          Defines the variable VAR to have the literal value VALUE.  VALUE is
          taken to be a raw Python string (as in r"VALUE").  Substitutions are
          first applied to VALUE.

Don't worry about the Python regular expressions thing mentioned in the above
description: we'll show you that it's fairly straightforward and even go through
an example of how to use it.

Let's walk through a test to see what happens from start to finish. Let's
examine `test02-basic-checkout.in`.
