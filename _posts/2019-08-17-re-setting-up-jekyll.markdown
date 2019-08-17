---
layout: post
title:  "Re-setting up this blog..."
date:   2019-08-17 17:22:00
categories: yak-shaving jekyll update
---

I never use Ruby, so for me to get going with Jekyll, I first needed
to check the [basics](https://jekyllrb.com/docs/installation/macos/).
Fortunately, I somehow have a recent enough version of Ruby and
[RubyGems](https://rubygems.org/pages/download):

{% highlight bash %}
$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-darwin16]
$ gem -v
2.7.6
{% endhighlight %}

It looks like I probably got Ruby with [Homebrew](https://brew.sh/) at some
point. (Homebrew is absolutely awesome, by the way.)

Next came `jekyll`:

{% highlight bash %}
$ gem install --user-install bundler jekyll
Fetching: bundler-2.0.2.gem (100%)
WARNING:  You don't have /Users/perdue/.gem/ruby/2.5.0/bin in your PATH,
	  gem executables will not run.
Successfully installed bundler-2.0.2
Parsing documentation for bundler-2.0.2
Installing ri documentation for bundler-2.0.2
Done installing documentation for bundler after 4 seconds
Fetching: jekyll-3.8.6.gem (100%)
Successfully installed jekyll-3.8.6
Parsing documentation for jekyll-3.8.6
Installing ri documentation for jekyll-3.8.6
Done installing documentation for jekyll after 1 seconds
2 gems installed
{% endhighlight %}

This required an update to my Bash profile (to the "local"
modifications I symlink in):

{% highlight bash %}
export PATH=$PATH:/Users/perdue/.gem/ruby/2.5.0/bin
{% endhighlight %}

After this:

{% highlight bash %}
$ gem env
RubyGems Environment:
  - RUBYGEMS VERSION: 2.7.6
  - RUBY VERSION: 2.5.1 (2018-03-29 patchlevel 57) [x86_64-darwin16]
  - INSTALLATION DIRECTORY: /usr/local/lib/ruby/gems/2.5.0
  - USER INSTALLATION DIRECTORY: /Users/perdue/.gem/ruby/2.5.0
  - RUBY EXECUTABLE: /usr/local/opt/ruby/bin/ruby
  - EXECUTABLE DIRECTORY: /usr/local/bin
  - SPEC CACHE DIRECTORY: /Users/perdue/.gem/specs
  - SYSTEM CONFIGURATION DIRECTORY: /usr/local/Cellar/ruby/2.5.1/etc
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86_64-darwin-16
  - GEM PATHS:
     - /usr/local/lib/ruby/gems/2.5.0
     - /Users/perdue/.gem/ruby/2.5.0
     - /usr/local/Cellar/ruby/2.5.1/lib/ruby/gems/2.5.0
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :backtrace => false
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - https://rubygems.org/
  - SHELL PATH:
     - /Library/Frameworks/Python.framework/Versions/3.7/bin
     - /usr/local/bin
     - /usr/local/bin
     - /usr/bin
     - /bin
     - /usr/sbin
     - /sbin
     - /Library/TeX/texbin
     - /usr/local/share/dotnet
     - /opt/X11/bin
     - ~/.dotnet/tools
     - /Users/perdue/PersonalScripts
     - /Users/perdue/Dropbox/UnixSettings/LocalScripts
     - /Users/perdue/Software/bin
{% endhighlight %}

Evidently we need to keep these paths up to date every time we
update Ruby (which probably won't be too often).

So, I have a bunch of material here already - will it work as is?

{% highlight bash %}
$ cd $HOME/Dropbox/Web/gnperdue.github.io
gnperdue.github.io$ bundle exec jekyll serve
Could not locate Gemfile or .bundle/ directory
{% endhighlight %}

Hmmm, what if we start fresh?

{% highlight bash %}
$ mkdir temp
$ cd temp/
temp$ ls
temp$ jekyll new myblog
Running bundle install in /Users/perdue/Dropbox/Web/temp/myblog...
  Bundler: Fetching gem metadata from https://rubygems.org/...........
...

  Bundler: * For more details, please refer to the Sass blog:
  Bundler: https://sass-lang.com/blog/posts/7828841
New jekyll site installed in /Users/perdue/Dropbox/Web/temp/myblog.
temp$
temp$ cd myblog/
myblog$ ls
404.html      Gemfile       Gemfile.lock  _config.yml   _posts/       about.md      index.md
myblog$ bundle exec jekyll serve
Configuration file: /Users/perdue/Dropbox/Web/temp/myblog/_config.yml
            Source: /Users/perdue/Dropbox/Web/temp/myblog
       Destination: /Users/perdue/Dropbox/Web/temp/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.48 seconds.
 Auto-regeneration: enabled for '/Users/perdue/Dropbox/Web/temp/myblog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
[2019-08-17 17:48:52] ERROR `/favicon.ico' not found.
{% endhighlight %}

Looks promising. Okay, I guess I need to migrate everything.

Let's nuke "myblog" and restart... okay, so next was copying in any relevant
styling, etc. from the old site, and then overwriting everything in the
old repository.

One issue - it seems we have to `bundle exec jekyll serve` from the directory
we created with `jekyll new <dir>`... does Ruby register this somewhere?
This is something I'm just not going to track down now.
