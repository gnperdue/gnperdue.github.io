---
layout: post
title:  "Setting up this blog..."
date:   2014-03-17 16:18:00
categories: yak-shaving
---

Jekyll turned out to be pretty easy to set up.

* `rubygems` was already installed on my Mac. However, I did have to update it:
{% highlight bash %}
    sudo gem update --system
{% endhighlight %}
I also had to pipe it through a `sudo`, which is sort of unfortunate, but I guess 
that was the price for using the system Ruby.

* Next I had to install `jekyll` and `json` (with the latter being required to 
avoid an error that occurred downstream, but which I'd backported to this point 
in the log: 
{% highlight bash %}
    sudo gem install jekyll
    sudo gem install json
{% endhighlight %}

* To test `jekyll`, it is sufficient to run `jekyll new myproject` (or whatever 
you would like to call it). This produces a scaffolded site:

{% highlight bash %}
    $ ls -1
    _config.yml
    _layouts/
    _posts/
    _site/
    css/
    index.html
{% endhighlight %}

* At this point, `jekyll serve` compiles the site and launches a local server you can 
browse to (it tells you the url).

* Next I went and fiddled with the content a bit so it was a vaguely plausible first
blog post rather than an obvious scaffolding example. Well, it is still a pretty 
obvious scaffolding example, but at least there is somewhat personalized content.
Adding a post is as simple as writing a file with the same pre-pended date format
and copying the header from the example. 

* Finally, I followed the instructions for GitHub pages to serve it. Their example 
(echo "Hello World" into an index.html file and push that) did not work. I got an 
email from GitHub almost immediately claiming the build failed. But, if I instead 
included my example jekyll files as the top level of my repository and pushed _that_,
everything worked fine. 
