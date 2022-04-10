---
title: Miscellaneous Things To Know
has_children: false
nav_order: 7
---

Miscellaneous Things to Know about the Project
----

Phew! That was a lot of commands to go over just now. But don't worry,
not all commands are created equal. You can see for each command the
approximate number of lines we took to do each part (that this
only counts code specific to that command -- it doesn't double-count
code reused in multiple commands). You shouldn't worry about matching
our solution exactly, but hopefully it gives you an idea about the
relative time consumed by each command. Merge is
a lengthier command than the others, so don't leave it for the
last minute!

This is an ambitious project, and it would not be surprising for you
to feel lost as to where to begin.  Therefore, feel free to collaborate with
others a little more closely than usual, with the following caveats:

- Acknowledge all collaborators in comments near the beginning of your
  `gitlet/Main.java` file.
- Don't share specific code; all collaborators must produce their own versions
  of the algorithms they come up with, so that we can see they differ.

The Piazza megathreads typically get very long for Gitlet, but they
are full of very good conversation and discussion on the approach for
particular commits. In this project more than any you should take
advantage of the size of the class and see if you can find someone
with a similar question to you on the megathread. It's very unlikely
that your question is so unique to you that nobody else has had it
(unless it is a bug that relates to your design, in which case you
should submit a Gitbug).

By now this spec has given you enough information to get
working on the project. But to help you out some more, there are a
couple of things you should be aware of:

## Dealing with Files
This project requires reading and writing of files. In order to do
these operations, you might find the classes `java.io.File` and
`java.nio.file.Files` helpful. Actually, you may find various things
in the `java.io` and `java.nio` packages helpful. Be sure to read the
`gitlet.Utils` package for other things we've written for you.
If you do a little
digging through all of these, you might find a couple of methods that will
make the I/O portion of this project _much_ easier! One warning: If
you find yourself using readers, writers, scanners, or streams,
you're making things more complicated than need be.

## Serialization Details
If you think about Gitlet, you'll notice that you can only run one
command every time you run the program. In order to successfully
complete your version-control system, you'll need to remember the
commit tree across commands. This means you'll have to design not just a
set of classes to represent internal Gitlet structures during execution,
but you'll need an analogous representation as files within your `.gitlet`
directories, which will carry across multiple runs of your program.

As indicated earlier, the convenient way to do this is to serialize
the runtime objects that you will need to store permanently in files.
In Java, this simply involves implementing
the `java.io.Serializable` interface:

    import java.io.Serializable;

    class MyObject implements Serializable {
        ...
    }

This interface has no methods; it simply marks its subtypes for the benefit
of some special Java classes for performing I/O on objects.  For example,

    import java.io.File;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectOutputStream;
    ...
        MyObject obj = ....;
        File outFile = new File(someFileName);
        try {
            ObjectOutputStream out =
                new ObjectOutputStream(new FileOutputStream(outFile));
            out.writeObject(obj);
            out.close();
        } catch (IOException excp) {
            ...
        }

will convert `obj` to a stream of bytes and store it in the file whose
name is stored in `someFileName`.  The object may then be reconstructed with
a code sequence such as

    import java.io.File;
    import java.io.FileInputStream;
    import java.io.IOException;
    import java.io.ObjectInputStream;
    ...
        MyObject obj;
        File inFile = new File(someFileName);
        try {
            ObjectInputStream inp =
                new ObjectInputStream(new FileInputStream(inFile));
            obj = (MyObject) inp.readObject();
            inp.close();
        } catch (IOException | ClassNotFoundException excp) {
            ...
            obj = null;
        }

The Java runtime does all the work of figuring out what fields need to be
converted to bytes and how to do so.

There is, however, one annoying subtlety to watch out for: Java serialization
follows pointers.  That is, not only is the object you pass into `writeObject`
serialized and written, but any object it points to as well.  If your internal
representation of commits, for example, represents the parent commits as
pointers to other commit objects, then writing the head of a branch will
write all the commits (and blobs) in the entire subgraph of commits
into one file, which is generally not what you want.  To avoid this,
don't use Java pointers to
refer to commits and blobs in your runtime objects, but instead use
SHA-1 hash strings.  Maintain a runtime map between these strings
and the runtime objects they refer to.  You create and fill in this map
while Gitlet is running, but never read or write it to a file.

You might find
it convenient to have (redundant) pointers commits as well as SHA-1 strings
to avoid the bother and execution time required to look them up each time.
You can store such pointers in your objects while still avoiding having them
written out by declaring them "transient", as in

        private transient MyCommitType parent1;

Such fields will not be serialized, and when back in and deserialized, will be
set to their default values (null for reference types).
You must be careful when reading the
objects that contain transient fields back in to set the transient fields to
appropriate values.

Unfortunately, looking at the serialized files your program has produced with
a text editor (for debugging purposes) would be rather unrevealing;
the contents are encoded in Java's
private serialization encoding.  We have therefore provided a simple debugging
utility program you might find useful: `gitlet.DumpObj`. See the Javadoc
comment on `gitlet/DumpObj.java` for details.
