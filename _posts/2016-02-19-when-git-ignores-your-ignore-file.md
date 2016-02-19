---
layout: post
title: When Git ignores your ignore file
date: 2016-02-19 16:54:29
categories: git
---

This was an annoying problem that was tough to Google out of. If you search
for "gitignore not ignoring untracked files" or something like that, every
answer assumes you added the file, then edited the `.gitignore` file, and
now are trying to ignore something you already told Git to track. The one
question I found on [StackOverflow](http://stackoverflow.com/questions/6380196/gitignore-does-not-work)
that got at this problem was marked as a duplicate with a link pointing back
to a question about how to ignore files that have already been committed.
Which is _not_ a duplicate of the question at all.

Some comments in the incorrectly-labeled-as-a-duplicate question tipped me
off, but they were a bit oblique.

It turns out that you should not have comments on the same line as the
ignores. Comments in a `.gitignore` file are not like comments in a Python or
shell script. So this is okay

    # ignore all foo.txt, foo.markdown, foo.dat, etc.
    foo*

But this will not work:

    foo*   # ignore all foo.txt, foo.markdown, foo.dat, etc.

`.gitignore` interprets the latter case as "ignore files named 
`"foo*   # ignore all foo.txt, foo.markdown, foo.dat, etc."`, which, of course,
you don't have...

Of course, it probably says this in the `.gitignore` documentation, but I
clearly missed it.
