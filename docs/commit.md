---
title: commit
parent: The Commands
has_children: false
nav_order: 3
---

## commit

- __Usage__: `java gitlet.Main commit [message]`

- __Description__: Saves a snapshot of tracked files in the current commit
  and staging area so they can be
  restored at a later time, creating a new commit.
  The commit is said to be _tracking_ the saved files. By default,
  each commit's snapshot of files will be exactly the same as its
  parent commit's snapshot of files; it will keep versions of files
  exactly as they are, and not update them. A commit will only update
  the contents of files it is tracking that have been staged
  for addition at
  the time of commit, in which case the commit will now include the
  version of the file that was staged instead of the version it got
  from its parent. A commit will save and start tracking any files
  that were staged for addition but weren't tracked by its parent.
  Finally, files tracked
  in the current commit may be untracked in the new commit as a result
  being _staged for removal_ by the
  `rm` command (below).

  The bottom line: By default a commit is the same as its parent.
  Files staged for addition and removal are the updates to the commit. Of
  course, the date (and likely the message) will also different from the parent.


  - **Some additional points about commit**:
    - The staging area is cleared after a commit.
    - The commit command never adds, changes, or removes files in the
      working directory (other than those in the `.gitlet` directory).  The
      `rm` command _will_ remove such files, as well as staging them for
      removal, so that they will be untracked after a `commit`.
    - Any changes made to files after staging for addition or removal
      are ignored by the
      `commit` command, which _only_ modifies the contents of the `.gitlet`
      directory.  For example, if you remove a tracked file using the Unix
      `rm` command (rather than Gitlet's command of the same name), it has
      no effect on the next commit, which will still contain the deleted version
      of the file.

    - After the commit command, the new commit is added as a new node
      in the commit tree.

    - The commit just made becomes the "current commit", and the head
      pointer now points to it. The previous head commit is this
      commit's parent commit.

    - Each commit should contain the date and time it was made.

    - Each commit has a log message associated with it that describes the
      changes to the files in the commit. This is specified by the
      user. The entire message should take up only one entry in
      the array `args` that is passed to `main`. To include multiword
      messages, you'll have to surround them in quotes.

    - Each commit is identified by its SHA-1 id, which must include the
      file (blob) references of its files,
      parent reference, log message, and commit time.


  - __Runtime__: Runtime should be constant with respect to any measure
    of number of commits. Runtime must be no worse than linear with
    respect to the total size of files the commit is tracking.
    Additionally, this command has a memory requirement: Committing must
    increase the size of the `.gitlet` directory by no more than the total
    size of the files staged for addition at the time of commit, not including
    additional metadata. This means don't store redundant copies of
    versions of files that a commit receives from its parent.
    You _are_ allowed to save whole additional copies of files;
    don't worry about only saving diffs, or anything like that.

  - __Failure cases__: If no files have been staged, abort. Print the message `No
    changes added to the commit.` Every commit must have a non-blank
    message. If it doesn't, print the error message `Please enter
    a commit message.` It is _not_ a failure for tracked files to be
    missing from the working directory or changed in the working directory.
    Just ignore everything outside the `.gitlet` directory entirely.

  - __Dangerous?__: No

  - __Differences from real git__: In real git, commits may have multiple
    parents (due to merging) and also have considerably more metadata.

  - __Our line count__: ~35

Here's a picture of a repository before and after making a commit:

![Before and after commit](image/before_and_after_commit.png)
