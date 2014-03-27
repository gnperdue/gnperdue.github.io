---
layout: post
title:  "Setting up scientific Python on Mac OSX..."
date:   2099-12-31 23:59:59
categories: yak-shaving python osx
---

First, if you just want to get going as fast as possible, I recommend 
[Anaconda](https://store.continuum.io/cshop/anaconda/). It unpacks as a single 
directory that has everything you need, and it doesn't interfere with anything 
on your system. 
All you have to do is prepend it to your
path and you can be off and running. They have a free model and some add-ons you 
can pay for. The sheer number of packages they include is quite impressive and 
they also offer an update system that keeps everything coherent.

If, however, you want to install all the packages and do the configuration 
yourself (as I prefer to, so I better understand what I'm using), it can be a 
bit of work. It is a tough call between Anaconda, which really is great, and setting
things up on your own. I wanted to set things up for myself just to be sure I had
a good fallback in case Anaconda becomes a problem somehow and to understand what 
is under the hood a bit better.

I found this guide from 
[Lowin Data Company](http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/)
to be decisive in getting everything done. There were a couple of extra snags I had 
to work out independently, but basically, this guide is based on their recommendations.

Lowin recommends using `virtualenv`, but I don't because I like `$PATH` stability. 
Most likely I just misunderstand how cool `virtualenv` is - I'm sure I'll come around
someday. They 
also recommend [Homebrew](http://brew.sh) and this I agree with and highly endorse.
You can, of course, use a different package manager or build from source, but I've 
tried [Fink](http://www.finkproject.org), [MacPorts](http://www.macports.org), and 
[Homebrew](http://brew.sh) and found [Homebrew](http://brew.sh) to be the simplest and
easiest to use.

The first "real" thing to do is go get a copy of the latest version of Python. I tried
this with both Python 2.7.6 and Python 3.3.3 and everything essentially works exactly the
same. The system Python on my Mac (Mountain Lion) is 2.7.2 and I just leave it alone through
all of this. I simply downloaded the `.dmg` file from the 
[Python downloads](https://www.python.org/downloads/)
and double-clicked and that was that. 

This puts Python in 
    /Library/Frameworks/Python.framework/Versions/2.7
If you have other 2.7's, (aside from your system Python), they'll be there too. The installer
also puts symlinks in `/usr/local/bin` which I'm less keen on because I regard `/usr/local`
as Homebrew's exclusive playground. Actually, it is worse than that, because to get the 
bundled version of `IDLE` to work, you may need to also download and install a new 
version of [TCL](http://www.activestate.com/activetcl/downloads) (I picked up 8.5.15.1 to
go with Python 3), and that goes to `/usr/local/` also. But, so far I've limited myself 
to just grumbling. `brew doctor` will complain about TCL no matter what, so leaving some 
Python symlinks is no big deal. Just remember if you want to clean things out that you 
need to check `/usr/local` also.

