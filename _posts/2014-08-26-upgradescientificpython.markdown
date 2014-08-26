---
layout: post
title:  "Upgrading the Scientific Python Stack on Mac OSX..."
date:   2014-08-26 8:30:47
categories: yak-shaving osx python 
---

After a big `brew upgrade`, I found `matplotlib` broken because of a change in `libpng`.
The upgrade turned out to be super-simple, modulo the packages I am forgetting right
now. In order:

    pip install --upgrade numpy
    pip install --upgrade scipy

The only tricky part was `matplotlib` itself. The `upgrade` command worked, but when 
launching Python and trying to `import` it, there were errors. So I removed it and 
then re-installed it:

    pip uninstall matplotlib
    pip install matplotlib

After that it was smooth sailing:

    pip install --upgrade ipython
    pip install --upgrade pyzmq
    pip install --upgrade jinja2
    pip install --upgrade pandas

I ran updates on these packages, but they were all already up to date:

    pip install --upgrade pygments
    pip install --upgrade statsmodels
    pip install --upgrade scikit-image
    pip install --upgrade scikit-learn

Likely, I will realize in a day or two that I forgot some other packages. Slightly trickier
will be the ones I had to `easy_install`, I suppose, and even worse will be the ones I 
compiled from source. This, of course, is why I would like to eventually move to using
[Anaconda](http://www.continuum.io).
