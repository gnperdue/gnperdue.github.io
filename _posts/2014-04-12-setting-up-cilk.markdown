---
layout: post
title:  "Setting up Cilk on Mac OSX..."
date:   2014-04-12 08:30:59
categories: yak-shaving cilk osx
---

The first thing I tried was to check out and build a version of GCC that was 
[Cilk](http://www.cilkplus.org) enabled. After eight hours of the slowest SVN
checkout I've ever watched, the checkout of the code failed for some reason. 
The timing (it was very close to eight hours after I started the checkout)
makes me think the remote server decided to just go ahead and put the 
checkout out of its misery.

The next thing I tried was to check out and build a version of Clang that was
[Cilk](http://www.cilkplus.org) enabled. This time, the checkout succeeded in
about one second:

    git clone -b cilkplus https://github.com/cilkplus/llvm
    git clone -b cilkplus https://github.com/cilkplus/clang llvm/tools/clang
    git clone -b cilkplus https://github.com/cilkplus/compiler-rt llvm/projects/compiler-rt

The [instructions](http://cilkplus.github.io) recommended [CMake](http://www.cmake.org)
for the build. I used [Homebrew](http://brew.sh) to install it:

    brew install cmake

Then, building Clang was very easy:

    $ cd llvm/
    $ mkdir build && cd build
    $ cmake -DCMAKE_INSTALL_PREFIX=/usr/local/cilk/cilk -DCMAKE_BUILD_TYPE=Release \
     -DLLVM_TARGETS_TO_BUILD=X86 -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ \
     /usr/local/cilk/llvm
    $ make && make install

Here I targeted `/usr/local/cilk/cilk/`, but to repeat this, you should just target
the area you want to install in.

This was very easy to setup and install, but the big downside is that the implementation
of Cilk is incomplete for Clang - it doesn't support array notation. So you can't do 
things like:

    a[0:n] = b[0:n] * c[0:n]

and multiply all the elements in two arrays elementwise with free paralellism. Everything
else seems to be there though. 
