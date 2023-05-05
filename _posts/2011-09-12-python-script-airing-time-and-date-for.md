---
layout: post
title: "[Python] Script - Airing time and Date for a list of TV Shows"
date: '2011-09-12T05:31:00.001+05:30'
author: Anirvan Mandal
tags:
- python
modified_time: '2011-10-23T17:32:35.465+05:30'
---

Making such a script was in the back of my mind for quite some time now. The main problem is that though many sites provide an option of selecting the TV shows you watch and then give the airing times It is usually in CST or GMT. So here is the script that allows you to have a list of TV shows you watch and then gives you information related to the next episode that is to be aired.


{% highlight python %}    
    #Python 2.7
    #Author n!ghtf0x
    #email anirvan.mandal@gmail.com
    #Script for retreving Tv show information from TVrage.com
    #uses tvrage apy for python ver 0.3.0
    #visit http://static-point.blogspot.com/2011/09/python-script-airing-time-and-date-for.html  for links to pytz and tvrage api
    
    from tvrage import quickinfo # --> python-tvrage-api 0.3.0 
    import string
    import pytz # pytz --> for time zone conversion
    import datetime
    from time import mktime
    from math import trunc
    import time
    import tvrage
    
    #time zones -- to convert the given time of episode airing to the desired timezone for example EST to IST
    us = pytz.timezone('America/New_York')
    india = pytz.timezone('Asia/Calcutta')
    
    #SeriesList -- The list of tv show names for which we want to collect the information #infoList -- List to which series information is appended
    SeriesList = ['supernatural 2005','one tree hill 2003','the big bang theory 2007','strike back 2010','breaking bad 2008', 'the vampire diaries 2009', 'how i met your mother 2005','castle 2009','modern family 2009','nikita 2010','the mentalist 2008','fringe 2008','the good wife 2009', 'gossip girl 2007','dexter 2006','house 2004','chuck 2007']  
    infoList = []
    
    # Note -- Add TV show names in the format "tv_show_name(small letters) year of premier"
    
    
    def timeFix(list): # function for conversion of airing time
      time = str(list['Airtime'])
      try:
        nextDate = (list['Next Episode'])
        nextDateWithTime = time + ' ' + nextDate[2]
        nextDateStrip = datetime.datetime.strptime(nextDateWithTime,'%A at %I:%M %p %b/%d/%Y')
        timeWithZoneus = us.localize(nextDateStrip)
        timeWithZonein = timeWithZoneus.astimezone(india)
        timeStamp = mktime(nextDateStrip.timetuple())
        nextDateWithTime = timeWithZonein.strftime('%A at %I:%M %p %b/%d/%Y')
        (list['Next Episode'])[2] = nextDateWithTime
    
      except KeyError:
        #print " Next episode information does not exist !" 
        list['Next Episode'] = ['N/A','N/A','N/A'] #since information of the next episode is not available
        timeStamp = '0'
    
    
      prevDate = (list['Latest Episode'])
    
    
    
    
      prevDateWithTime = time + ' ' + prevDate[2]
      prevDateStrip = datetime.datetime.strptime(prevDateWithTime,'%A at %I:%M %p %b/%d/%Y')
      timeWithZoneus = us.localize(prevDateStrip)
      timeWithZonein = timeWithZoneus.astimezone(india)
      prevDateWithTime = timeWithZonein.strftime('%A at %I:%M %p %b/%d/%Y')
      (list['Latest Episode'])[2] = prevDateWithTime
      infoList.append([list['Show Name'],(list['Next Episode'])[0],(list['Next Episode'])[1],(list['Next Episode'])[2],timeStamp]) #all the desired information -- checkout pytvrage api for other information
      return list
    
    
    try:
      i = 0
      while i <= 16:
        try:
          #print i --used for debugging
          seriesInfo = quickinfo.fetch(SeriesList[i])
          series_time_fixed = timeFix(seriesInfo)# Print series_time_fixed[] for having a look at various information returned by tvrage
    
          i = i+1
        except tvrage.util.TvrageError: # If server does not reply 
          print "Retrying " 
    
    
    
    
    
      # creating the key for sorting according to time of release  
      def stamp(t):
        return t[4]
      sortedInfoList = sorted(infoList,key=stamp)
    
      #Printing the data in a desired format
      print 'Title'.ljust(25,' ')  +'Next Episode'.ljust(18,' ')  + 'Episode name'.ljust(30,' ') + 'Airing Time (IST)'.ljust(30,' ') 
      for x in sortedInfoList:
        print x[0].ljust(25,' ')  +x[1].ljust(18,' ')  + x[2].ljust(30,' ') + x[3].ljust(30,' ')
    except AttributeError:# When no internet connectivity is found --> restart script
      print "\n\n\tCheck Your Internet Connectivity !\n"
    
{% endhighlight %}

The following code requires you to have [python-tvrage-api 0.3.0](http://pypi.python.org/pypi/python-tvrage/0.3.0) and [pytz](http://pytz.sourceforge.net/) packages to be installed with your python.

[![](/assets/images/tv-ratings-output.jpg)](/assets/images/tv-ratings-output.jpg)

Output Produced by the scriptFor now I have used a tv rage api that provides output int he form of lists. TV rage provides it's own api for getting xml from rss scripts. 
