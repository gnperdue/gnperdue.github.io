---
layout: post
title:  "Installing statsmodels for Python on Mac OSX..."
date:   2014-05-10 18:30:59
categories: yak-shaving osx python 
---

First, we're forced to install [patsy](http://patsy.readthedocs.org/en/latest/):

    pip install --upgrade patsy

This runs fine, but produces a scary-looking set of comments about uninstalling 
NumPy and re-installing it. Hmmm. Well, carrying on...

    pip install statsmodels

After this, I can `import statsmodels.api as sp` in an IPython pylab session.
NumPy seems fine too, so, a simple victory.
