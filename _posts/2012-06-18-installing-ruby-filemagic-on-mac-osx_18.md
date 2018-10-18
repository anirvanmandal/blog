---
layout: post
title: Installing ruby-filemagic on mac osx lion 10.7.4
date: '2012-06-18T03:35:00.001+05:30'
author: Anirvan Mandal
tags:
- ruby
- macosx
- ruby-gems
modified_time: '2014-01-24T01:40:03.600+05:30'
blogger_id: tag:blogger.com,1999:blog-878378505151338176.post-7207798318109411474
blogger_orig_url: https://static-point.blogspot.com/2012/06/installing-ruby-filemagic-on-mac-osx_18.html
---

When you try to install ruby-filemagic gem into your rails application or your gems You'll get an error which will look something like this.

    
    Building native extensions.  This could take a while...
    
    ERROR:  Error installing ruby-filemagic:
    
    ERROR: Failed to build gem native extension.
    
    
    
            /Users/anirvan/.rvm/rubies/ruby-1.9.3-p125/bin/ruby extconf.rb
    
    checking for magic_open() in -lmagic... no
    
    *** ERROR: missing required library to compile this module
    
    *** extconf.rb failed ***
    
    Could not create Makefile due to some reason, probably lack of
    
    necessary libraries and/or headers.  Check the mkmf.log file for more
    
    details.  You may need configuration options.
    
    
    
    Provided configuration options:
    
    --with-opt-dir
    
    --with-opt-include
    
    --without-opt-include=${opt-dir}/include
    
    --with-opt-lib
    
    --without-opt-lib=${opt-dir}/lib
    
    --with-make-prog
    
    --without-make-prog
    
    --srcdir=.
    
    --curdir
    
    --ruby=/Users/anirvan/.rvm/rubies/ruby-1.9.3-p125/bin/ruby
    
    --with-magic-dir
    
    --without-magic-dir
    
    --with-magic-include
    
    --without-magic-include=${magic-dir}/include
    
    --with-magic-lib
    
    --without-magic-lib=${magic-dir}/lib
    
    --with-magiclib
    
    --without-magiclib
    
    
    
    

This error basically implies that the libmagic gem is missing or that it's location is not found in the PATH variable.

So the first thing that we need to is to install libmagic using homebrew

    
    brew install libmagic
    
    

or macports

    
    sudo port install file
    
    

Now we are ready to install filemagic gem

(if installed using homebrew)

    
    [sudo] gem install ruby-filemagic -v '0.4.2' -- --with-magic-dir='~/homebrew/Cellar/libmagic'
    
    

(if installed using macports)

    
    [sudo] gem install ruby-filemagic -v '0.4.2' -- --with-magic-dir='/opt/local'
    
    

There are many blogs on the internet that show us how to compile the gem from source like in this [answer#2](http://stackoverflow.com/questions/3190961/trying-to-install-ruby-filemagic-on-snow-leopard-using-brew-rather-than-ports). The important thing to note here is that when a gem is compiled using make from extconf.rb the gem is not added to the ruby gem list. Credits go to this [blog](http://foundry.ahfr.org/posts/943) for helping me out in installing the gem. 
