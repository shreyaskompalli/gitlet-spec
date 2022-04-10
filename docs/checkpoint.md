---
title: Checkpoint Details
has_children: false
nav_order: 10
---

Checkpoint Details
----

TODO: Add checkpoint grader/velocity limiting details

Since you are not working from a substantial skeleton this time, we are
asking that everybody submit a design document describing their implementation
strategy.  It is not graded, but we will insist on having it before helping you
with bugs in your program.  Here are [some guidelines](./design.html), as
well as an [example from the Enigma project](./enigma-example.html).

There will be an initial **required** checkpoint for the project, due **Friday 4/22 at 11:59PM**.
The checkpoint is worth **6 points**. It consists of a programming portion, as well as a
conceptual quiz on Gradescope.

You can complete the conceptual quiz on Gradescope by clicking on the assignment titled
[Projet 3: Gitlet Checkpoint Quiz](https://www.gradescope.com/courses/350363/assignments/1983273).
The quiz is out of 1 point, and tests your understanding of the Gitlet commands. You have
unlimited tries to complete the quiz before the deadline, and should see feedback on the answers
you choose. **The project lateness policy will not apply to the checkpoint quiz, so any late quizzes
will receive a score of 0**.


For the remaining 5 points from the checkpoint, you can submit the
programming portion using the tag `proj3a-` _n_ (where,
as usual, _n_ is simply an integer.)  The checkpoint autograder will check
that

* Your program compiles.
* You pass the sample tests from the skeleton: `testing/samples/*.in`.
  These require you to implement `init`, `add`, `commit`,
  `checkout -- [file name]`,
  `checkout [commit id] --  [file name]`, and `log`.

In addition, it will comment on (but not score):

* Whether you pass style checks (it will ignore FIXME comments for now; we won't
  in the final submission.)
* Whether there are compiler warning messages.
* Whether you pass your own unit and acceptance tests (as run by `make check`).

We ***will*** score these in your final submission.

For the checkpoint grader, when it is released on Monday 4/11, you will be able to submit
once every 6 hours with full grader outputs. Starting Monday 4/18, you will be able to submit once
every 3 hours with full grader outputs. On the due date, Friday 4/22, there will be no
restrictions on the grader.

**NOTE:** The checkpoint grader restrictions are much more lenient than the main project 3 autograder.
