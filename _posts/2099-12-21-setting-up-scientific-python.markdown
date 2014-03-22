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

Lowin recommends using `virtualenv`, but I don't because I like `$PATH` stability. They 
also recommend [Homebrew](http://brew.sh) and this I agree with and highly endorse.
You can, of course, use a different package manager or build from source, but I've 
tried [Fink](http://www.finkproject.org), [MacPorts](http://www.macports.org), and 
[Homebrew](http://brew.sh) and found [Homebrew](http://brew.sh) to be the simplest and
easiest to use.



