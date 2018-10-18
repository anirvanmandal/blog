---
layout: post
title: MKV files locked by windows explorer.
date: '2011-08-07T10:34:00.000+05:30'
author: Anirvan Mandal
tags:
- windows 7
modified_time: '2011-08-07T10:34:25.391+05:30'
blogger_id: tag:blogger.com,1999:blog-878378505151338176.post-7334542520484138701
blogger_orig_url: https://static-point.blogspot.com/2011/08/mkv-files-locked-by-windows-explorer.html
---

Well I wouldn't want my first post to be a technical one, but I simply can't resist writing this one.

Sometimes matroska files get locked by the windows explorer and then you are just stuck with the file on your HDD unable to move it or delete it. If this is not enough if you play the file in any media player your explorer stops responding and the computer gets hung with 100% CPU usage.

I tried googling the problem on the Internet and found several interesting solutions. Basically the solution was to unlock the file from the windows explorer using a software. One of the software available is [unlocker](http://www.filehippo.com/download_unlocker/). But as soon as I tried unlocking a file using unlocker my windows explorer stopped responding and my task manager showed 100% CPU usage ... irritating. Now there was one other solution given [lockhunter](http://lockhunter.com/). But just when i was about to download that I got an idea.

Instead of using any 3rd party software why not try deleting the file using the windows command prompt after killing the explorer. So I started the command prompt, browsed to the file directory, then killed the explorer. and then  typed in the command "DEL -A /P <file_name>" . The command prompt then asked for the confirmation . Just type Y and press enter  and voila the file was deleted.

Well it was really exciting to find a solution that was never documented before.
