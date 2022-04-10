---
title: Setting Up A Test
parent: Understanding Acceptance Tests
has_children: false
nav_order: 2
---

## Setting Up An Acceptance Test

As you'll soon discover, there can be a lot of repeated setup to test a
particular command: for example, if you're testing the `checkout` command you
need to:

1. Initialize a Gitlet Repository
2. Create a commit with a file in some version (v1)
3. Create another commit with that file in some other version (v2)
4. Checkout that file to v1

And perhaps even more if you want to test with files that were untracked in the
second commit but tracked in the first.

So the way you can save yourself time is by adding all that setup in a file and
using the `I` command. Say we do that here:

    # Initialize, add, and commit a file.
    > init
    <<<
    + a.txt wug.txt
    > add a.txt
    <<<
    > commit "a is a wug"
    <<<

We should place this file with the rest of the tests in the `samples` directory,
but with a file extension `.inc`, so maybe we name it
`samples/commit_setup.inc`. If we gave it the file extension `.in`, our testing
script will mistake it for a test and try to run it individually. Now, in our
actual test, we simply use the command:

    I commit_setup.inc

This will have the testing script run all of the commands in that file and keep
the temporary directory it creates. This keeps your tests relatively short and
thus easier to read.

We've included one `.inc` file called `definitions.inc` that will set up
patterns for your convenience. Let's understand what patterns are.

### Pattern matching output

The most confusing part of testing is the output for something like `log`. There
are a few reasons why:

1. The commit SHA will change as you modify your code and hash more things, so
   you would have to continually modify your test to keep up with the changes to
   the SHA.
2. Your date will change every time since time only moves forwards.
3. It makes the tests very long.

We also don't really care the exact text: just that there is some SHA there and
something with the right date format. For this reason, our tests use pattern
matching.

This is not a concept you will need to understand, but at a high level we define
a pattern for some text (i.e. a commit SHA) and then just check that the output
has that pattern (without caring about the actual letters and numbers).

Here is how you'd do that for the output of `log` and check that it matches the
pattern:

    # First "import" the pattern definitions from our setup
    I definitions.inc
    # You would add your lines here that create commits with the
    # specified messages. We'll omit this for this example.
    > log
    ===
    ${COMMIT_HEAD}
    ${DATE}
    added wug

    ===
    ${COMMIT_HEAD}
    ${DATE}
    initial commit

    <<<*

The section we see is the same as a normal Gitlet command, except it ends in
`<<<*` which tells the testing script to use patterns. The patterns are enclosed
in `${PATTERN_NAME}`.

All the patterns are defined in `samples/definitions.inc`. You don't need to
understand the actual pattern, just the thing it matches. For example, `HEADER`
matches the header of a commit which should look something like:

    commit fc26c386f550fc17a0d4d359d70bae33c47c54b9

That's just some random commit SHA.

So when we create the expected output for this test, we'll need to know how many
entries are in this log and what the commit messages are.

You can do similar things for the `status` command:

    I definitions.inc
    # Add commands here to setup the status. We'll omit them here.
    > status
    === Branches ===
    \*master

    === Staged Files ===
    g.txt

    === Removed Files ===

    === Modifications Not Staged For Commit ===

    === Untracked Files ===
    ${ARBLINES}

    <<<*

The pattern we used here is `ARBLINES` which is arbitrary lines. If you actually
care what is untracked, then you can add that here without the pattern, but
perhaps we're more interested in seeing `g.txt` staged for addition.

Notice the `\*` on the branch `master`. Recall that in the `status` command, you
should prefix the HEAD branch with a `*`. If you use a pattern, you'll need to
replace this `*` with a `\*` in the expected output. The reason is out of the
scope of the class, but it is called "escaping" the asterisk. If you don't use a
pattern (i.e.  your command ends in `<<<` not `<<<*`, then you can use the `*`
without the `\`).

The final thing you can do with these patterns is "save" a matched portion.
**Warning**: this seems like magic and we don't care at all if you understand how
this works, just know that it does and it is available to you. You can copy and
paste the relevant part from our provided tests so you don't need to worry too
much about making these from scratch. With that out of the way, let's see what
this is.

If you're doing a `checkout` command, you need to use the SHA identifier to
specify which commit to checkout to/from. But remember we used patterns, so we
don't actually know the SHA identifier at the time of creating the test. That is
problematic. We'll use `test04-prev-checkout.in` to see how you can "capture" or "save" the SHA:

    I definitions.inc
    # Each ${COMMIT_HEAD} captures its commit UID.
    > log
    ===
    ${COMMIT_HEAD}
    ${DATE}
    version 2 of wug.txt

    ===
    ${COMMIT_HEAD}
    ${DATE}
    version 1 of wug.txt

    ===
    ${COMMIT_HEAD}
    ${DATE}
    initial commit

    <<<*

This will set up the UID (SHA) to be captured after the `log` command. So right
after this command runs, we can use the `D` command to define the UIDs to
variables:

    # UID of second version
    D UID2 "${1}"
    # UID of first version
    D UID1 "${2}"

Notice how the numbering is backwards: the numbering begins at 1 and starts at
the top of the log. That is why the current version (i.e. second version) is
defined as `"${1}"`. We don't care about the initial commit, so we don't bother
capturing it's UID.

Now we can use that definition to checkout to that captured SHA:

    > checkout ${UID1} -- wug.txt
    <<<

And now you can make your assertions to ensure the checkout was successful.
