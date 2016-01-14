---
layout: post
title: Slack and GitHub + HepForge SVN
date: 2016-01-14 16:42:42
categories: slack git svn
---

Basically, if you poke around on the internet, it is pretty easy to get started
on these integration. The one trick thing is I can't get the Subversion post-commit
scripts to pass along all the information I want them to. I'm not sure if the problem
is with Slack accepting the full JSON payload or if it is some problem constructing
it (hard to imagine what that would be though).

### GitHub+Slack

* Go to http://www.slack.com/apps
* Search for "GitHub", and install the GitHub app in Slack for my team.
* Instead of providing blanket authentication, `switch to unauthed mode`.
* They provide an expandable tab with instructions.
* You can choose which channel to post to (or create a new one).
* Go repo-by-repo and visit the `Settings` tab.
* Go to `Webhooks & services`
* Add a webhook.
* Copy the URL that Slack provides, and leave the `Secret` field blank.
* Leave content type as `application/json`.
* You can save at this point and come back and "edit" the configutation again
when you need to add another repo.

### GitHub+Subversion

* Go to http://www.slack.com/apps
* Search for "Subversion" and install the app.
* Basically, they provide an example post-commit handler with all the
goodies. We only need to get the token and name the team correctly in the script.
Be careful to supply the correct arguments to the script in the `post-commit`.


