---
title: An Example
parent: Understanding Acceptance Tests
has_children: false
nav_order: 1
---

## An Example Acceptance Test

When we first run this test, a temporary directory gets created that is
initially empty. Our directory structure is now:

    .
    ├── Makefile
    ├── student_tests
    ├── samples
    │   ├── test01-init.in
    │   ├── test02-basic-checkout.in
    │   ├── test03-basic-log.in
    │   ├── test04-prev-checkout.in
    │   └── definitions.inc
    ├── src
    │   ├── notwug.txt
    │   └── wug.txt
    ├── test02-basic-checkout_0          <==== Just created
    ├── runner.py
    └── tester.py

This temporary directory is the Gitlet repository that will be used for this
execution of the test, so we will add things there and run all of our Gitlet
commands there as well.  If you ran the test a second time without deleting the
directory, it'll create a new directory called `test02-basic-checkout_1`, and so
on.  Each execution of a test uses it's own directory, so don't worry about
tests interfering with each other as that cannot happen.

The first line of the test is a comment, so we ignore it.

The next section is:

    > init
    <<<

This shouldn't have any output as we can tell by this section not having any
text between the first line with `>` and the line with `<<<`. But, as we know,
this should create a `.gitlet` folder. So our directory structure is now:

    .
    ├── Makefile
    ├── student_tests
    ├── samples
    │   ├── test01-init.in
    │   ├── test02-basic-checkout.in
    │   ├── test03-basic-log.in
    │   ├── test04-prev-checkout.in
    │   └── definitions.inc
    ├── src
    │   ├── notwug.txt
    │   └── wug.txt
    ├── test02-basic-checkout_0
    │   └── .gitlet                     <==== Just created
    ├── runner.py
    └── tester.py

The next section is:

    + wug.txt wug.txt

This line uses the `+` command. This will take the file on the right-hand
side from the `src` directory and copy its contents to the file on the
left-hand side in the temporary directory (creating it if it doesn't exist).
They happen to have the same name, but that doesn't matter since they're in
different directories. After this command, our directory structure is now:

    .
    ├── Makefile
    ├── student_tests
    ├── samples
    │   ├── test01-init.in
    │   ├── test02-basic-checkout.in
    │   ├── test03-basic-log.in
    │   ├── test04-prev-checkout.in
    │   └── definitions.inc
    ├── src
    │   ├── notwug.txt
    │   └── wug.txt
    ├── test02-basic-checkout_0
    │   ├── .gitlet
    │   └── wug.txt                     <==== Just created
    ├── runner.py
    └── tester.py

Now we see what the `src` directory is used for: it contains file contents that
the tests can use to set up the Gitlet repository however you wants. If you want
to add special contents to a file, you should add those contents to an
appropriately named file in `src` and then use the same `+` command as we have
here. It's easy to get confused with the order of arguments, so make sure the
right-hand side is referencing the file in the `src` directory, and the
right-hand side is referencing the file in the temporary directory.

The next section is:

    > add wug.txt
    <<<

As you can see, there should be no output. The `wug.txt` file is now staged for
addition in the temporary directory. At this point, your directory structure
will likely change within the `test02-basic-checkout_0/.gitlet` directory since
you'll need to somehow persist the fact that `wug.txt` is staged for addition.

The next section is:

    > commit "added wug"
    <<<

And, again, there is no output, and, again, your directory structure within
`.gitlet` might change.

The next section is:

    + wug.txt notwug.txt

Since `wug.txt` already exists in our temporary directory, its contents changes
to be whatever was in `src/notwug.txt`.

The next section is

    > checkout -- wug.txt
    <<<

Which, again, has no output. However, it should change the contents of `wug.txt`
in our temporary directory back to its original contents which is exactly the
contents of `src/wug.txt`. The next command is what asserts that:

    = wug.txt wug.txt

This is an assertion: if the file on the left-hand side (again, this is in the
temporary directory) doesn't have the exact contents of the file on the
right-hand side (from the `src` directory), the testing script will error and
say your file contents are not correct.

There are two other assertion commands available to you:

    E NAME

Will assert that there exists a file/folder named `NAME` in the temporary
directory. It doesn't check the contents, only that it exists. If no file/folder
with that name exists, the test will fail.

    * NAME

Will assert that there does NOT exist a file/folder named `NAME` in the
temporary directory. If there does exist a file/folder with that name, the test
will fail.

That happened to be the last line of the test, so the test finishes. If the
`--keep` flag was provided, the temporary directory will remain, otherwise it
will be deleted. You might want to keep it if you suspect your `.gitlet`
directory is not being properly setup or there is some issue with persistence.
