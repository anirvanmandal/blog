---
layout: post
title: "[Python] Delete Current Playing Song from iTunes Library and HDD"
date: '2011-10-24T17:21:00.000+05:30'
author: Anirvan Mandal
categories: [Python, Windows 7]
tags:
- python
- windows-7
modified_time: '2011-10-24T17:24:13.651+05:30'
---

One of my Friends made a script that allows him to delete the currently playing song on his mpd on archlinux. I found it fascinating.
The script basically deletes the current playing song from iTunes and removes it from the directory as well.

First I'll brief you on how to make the script executable and how to bind it to a global shortcut.

Download the python script from the [github source](https://github.com/nightfox/pyTunes-Current-Song-Deleter) .

Then Go to your command prompt  in windows 7 and navigate to the location in which you saved the script.
Now open the python console. Now type in the following commands:

```python
import py_compile
py_compile.compile('itunes.py')
```

![Compile image](/assets/images/delete-song-py-script-compile.png)

Now you can will find a file named itunes.pyc in the folder. Right-click and create shortcut for itunes.pyc. Now  right-click on the shortcut and select properties. In the shortcut tab there is an option for the shortcut key. Assign any shortcut key of your choice and click on apply.

![Add Shortcut](/assets/images/delete-song-py-script-shortcut.png)

Now create a folder `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Scripts` and move the shortcut to this location so that it becomes globally accessible.

To run the script python 2.7 is required. Additionally you'll need the install the [Wx Python package](http://www.wxpython.org/download.php) for the python script GUI to run.

Now you are all set to run the script. On pressing the shortcut You'll get a window saying that the database is outdated. Click on the Refresh button to sync the iTunes database with the scripts hash table. You'll need to do this each time songs are added to the iTunes music library.

![Hash not updated](/assets/images/delete-song-py-script-hash-not-updated.png)

Now when the Song is found in the hash table. The script will ask for your confirmation before finally deleting the song from the HDD and the iTunes Music Library.

![Confirmation box](/assets/images/delete-song-py-script-confirmation.png)

Finally when you click on delete, the song will be deleted from both the HDD as well the iTunes Music Library.
