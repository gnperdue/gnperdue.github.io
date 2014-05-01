---
layout: post
title:  "Setting up the Basemap Matplotlib Tooklkit on Mac OSX..."
date:   2014-05-01 08:30:59
categories: yak-shaving osx python matplotlib
---

To install the Basemap toolkit, first read the instructions here:

    http://matplotlib.org/basemap/users/installing.html

There is a source tarball. Download it and unpack it. It will look like this:

    basemap-1.0.7$ ls
    API_CHANGES    KNOWN_BUGS     LICENSE_proj4  PKG-INFO       examples/      nad2bin.c
    Changelog      LICENSE_data   LICENSE_pyshp  README         geos-3.3.3/    setup.py
    FAQ            LICENSE_geos   MANIFEST.in    doc/           lib/           src/

First, `cd geos-3.3.3/`. Then, we need to set the environment variable for 
the install directory. I chose `export GEOS_DIR=/usr/local/geos`. After that:

    geos-3.3.3$ ./configure --prefix=$GEOS_DIR
    checking build system type... x86_64-apple-darwin12.5.0
    ...
    config.status: executing libtool commands
    Swig: false
    Python bindings: false
    Ruby bindings: false
    PHP bindings: false

The conclusion was a little unsatisfying, but soldiering on:

    geos-3.3.3$ make
    geos-3.3.3$ make install

There were no error messages. So, finally, going back up a directory:

    basemap-1.0.7$ python setup.py install

Then, last of all, to test:

    basemap-1.0.7$ python
    Python 2.7.6 (v2.7.6:3a1db0d2747e, Nov 10 2013, 00:42:54)
    [GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> from mpl_toolkits.basemap import Basemap
    >>>
    basemap-1.0.7$ cd examples/
    examples$ python simpletest.py

Everything seems to work!
