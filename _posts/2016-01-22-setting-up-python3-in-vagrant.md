---
layout: post
title: Setting up a Python3 `virtualenv` in Vagrant
date: 2016-01-22 16:17:46
categories: vagrant python virtualenv
---
If we grab a bleeding edge Ubuntu in a Vagrant VM, we get Python3 for free.

So, if we add the following to our provisioning set up script (run when we
`vagrant up`):

    sudo apt-get install python3-setuptools
    sudo easy_install3 pip       # will be a Python3 pip
    sudo pip install virtualenv  # will be py3

then we have `virtualenv` ready to go. We can just `vagrant ssh` into the VM
and do the following:

    virtualenv testEnv
    cd testEnv/
    source bin/activate
    python --version              # shows py3
    pip install beautifulsoup4
    python                        # now, import bs4 works

Then, we can do things in our virtual environemnt like:

    pip install django==1.7
    pip install selenium==2.45.0

As an aside, we can check the installed versions are what we expect at runtime
with:

    (testEnv) vagrant@vagrant-ubuntu-wily-64:~/testEnv$ python
    Python 3.4.3+ (default, Oct 14 2015, 16:03:50)
    [GCC 5.2.1 20151010] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import pkg_resources
    >>> pkg_resources.get_distribution("selenium").version
    '2.45.0'

Then, to get out of `virtualenv`, just do the usual:

    deactivate

(Note: `which deactivate` doesn't show anything, but it works anyway.)
