---
layout: post
title: "[Python] Delete Current Playing Song from iTunes Library and HDD"
date: '2011-10-24T17:21:00.000+05:30'
author: Anirvan Mandal
tags:
- python
- windows-7
modified_time: '2011-10-24T17:24:13.651+05:30'
---

It's been a long time since I last posted. Most of the time I was busy with exams and placements. Other times I was just lazy.

One of my Friends made a script that allows him to delete the currently playing song on his mpd on archlinux. I found this script to be very handy. So I decided to make a similar script in python which can delete the current playing song from the iTunes library as well as from the Hard disk.

The script basically works like this.

First I'll brief you on how to make the script executable and how to bind it to a global shortcut.

Download the python script from the [github source](https://github.com/nightfox/pyTunes-Current-Song-Deleter) .

Then Go to your command prompt  in windows 7 and navigate to the location in which you saved the script.
Now open the python console. Now type in the following commands:

    import py_compile
    py_compile.compile('itunes.py')
    

[![](http://1.bp.blogspot.com/-4a8S0XCUmoA/TqVHLWFmXYI/AAAAAAAAABY/C5V5XGczyVE/s640/py_compile.png)](http://1.bp.blogspot.com/-4a8S0XCUmoA/TqVHLWFmXYI/AAAAAAAAABY/C5V5XGczyVE/s1600/py_compile.png)compiling python scripts.

[![](http://3.bp.blogspot.com/-x9rOin1ShaM/TqVH96xq_II/AAAAAAAAABg/a24o3Rjc4I0/s400/shortcut.png)](http://3.bp.blogspot.com/-x9rOin1ShaM/TqVH96xq_II/AAAAAAAAABg/a24o3Rjc4I0/s1600/shortcut.png)

Now You can will find a file named itunes.pyc in the folder. Right-click and create shortcut for itunes.pyc. Now  right-click on the shortcut and select properties. In the shortcut tab there is an option for the shortcut key. Assign any shortcut key of your choice and click on apply.

Now create a folder 'C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Scripts' and move the shortcut to this location so that it becomes globally accessible.

To run the script python 2.7 is required. Additionally you'll need the install the [Wx Python package](http://www.wxpython.org/download.php) for the python script GUI to run.

[![](http://3.bp.blogspot.com/-m8-jU83vAw8/TqVLpy8m6dI/AAAAAAAAABo/6Q-obWQO8zI/s320/hashntupdated.png)](http://3.bp.blogspot.com/-m8-jU83vAw8/TqVLpy8m6dI/AAAAAAAAABo/6Q-obWQO8zI/s1600/hashntupdated.png)
Now you are all set to run the script. On pressing the shortcut You'll get a window saying that the database is outdated. Click on the Refresh button to sync the iTunes database with the scripts hash table. You'll need to do this each time songs are added to the iTunes music library.

Now when the Song is found in the hash table. The script will ask for your confirmation before finally deleting the song from the HDD and the iTunes Music Library.

[![](http://4.bp.blogspot.com/-enNxpeD3SgI/TqVNha71MpI/AAAAAAAAABw/0rvVVoTcV2A/s320/confirmation.png)](http://4.bp.blogspot.com/-enNxpeD3SgI/TqVNha71MpI/AAAAAAAAABw/0rvVVoTcV2A/s1600/confirmation.png)

Finally when you click on delete, the song will be deleted from both the HDD as well the iTunes Music Library.
