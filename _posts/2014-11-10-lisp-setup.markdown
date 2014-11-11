---
layout: post
title:  "Setting up Steel Bank Common Lisp on Mac OSX..."
date:   2014-11-10 20:19:17
categories: yak-shaving osx lisp
---

# Lisp

To get Lisp set up, I mostly followed this post by 
[Jonathan Fischer](http://www.mohiji.org/2011/01/31/modern-common-lisp-on-osx/).
The post is few years old, but it all worked - with some obvious tweaks.

## SBCL

First, I installed [SBCL](http://www.sbcl.org) using [Homebrew](http://brew.sh).
I also installed `rlwrap` to get proper readline support for SBCL:

* `brew install sbcl --with-ldb`
* `brew install rlwrap`

After this I could run `rlwrap sbcl` and get a Lisp prompt where the up and down
arrow keys worked.

## QuickLisp

Next I got QuickLisp set up following Jonathan's instructions:

* `curl -O http://beta.quicklisp.org/quicklisp.lisp`
* `curl -O http://beta.quicklisp.org/quicklisp.lisp.asc`
* `sbcl --load quicklisp.lisp`
* In SBCL: `(quicklisp-quickstart:install)`
* In SBCL: `(ql:add-to-init-file)`
* In SBCL: `(ql:quickload "quicklisp-slime-helper")`

After that, I put the `.sbclrc` file that was automatically generated into Dropbox
so I could link to it on all my machines:

* `ln -s $HOME/Dropbox/Programming/Lisp/sbclrc .sbclrc`

## Emacs

Finally, I wanted to get Emacs set up so I could use `SLIME`. Historically, I've been
a Vim person, but I felt that if I am going to write Lisp code, I should do it with
Emacs:

* `brew install emacs`
* In Terminal.app preferences, check "Use option as meta"

Now, of course, learning how to use Emacs and getting it set up the way I like
is some bit of work...

