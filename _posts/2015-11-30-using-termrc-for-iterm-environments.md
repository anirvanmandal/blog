---
layout: post
title: Using Termrc For Iterm Environments
date: '2015-11-30T12:50:50.000Z'
author: Anirvan Mandal
categories: [iTerm]
tags: iterm osx termrc

---

I was looking for a way to automate my iTerm environment on a per project basis and I happened to stumble upon [termrc](https://github.com/briangonzalez/termrc). It's a really nifty ruby gem that uses a config file to start an iTerm environment.

I split my iTerm window into 4 panes when working on a rails project. 1 server, 1 console, 1 resque worker and 1 console for bash commands.
This can be programmatically accomplished by using a simple configuration in a Termfile (YAML). 

{% highlight bash %}
# /your/project/root/Termfile

root:
  /your/project/root

commands:
  server: bundle exec rails s
  console: bundle exec rails c
  worker: echo "space for worker"
  bash: echo "Run your git/bundle/bower here"

layout:
  - [ server , console ]
  - [ worker , bash ]
{% endhighlight %}

You can initialize your work environment using:
{% highlight bash %}
termrc start /your/project/root/Termfile
{% endhighlight %}

Additionally I added a shell script to perform tasks before starting the iTerm work environment

{% highlight bash %}
#! /usr/bin/env bash
ROOT=/your/project/root
cd $ROOT && G_STATUS="$(git status | grep clean)"
EXPECTED_G_STATUS='nothing to commit, working directory clean'
if [ "$G_STATUS"=="$EXPECTED_G_STATUS" ]
then
        cd $ROOT && git fetch
        cd $ROOT && bundle install

        cd $ROOT && bundle exec rake db:migrate
        cd $ROOT && bundle exec rake db:migrate PRODUCT=recruit
        cd $ROOT && bundle exec rake bower:update
fi

termrc start ~/Termfile
{% endhighlight %}
