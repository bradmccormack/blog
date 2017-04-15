---
title: Change disk sleeping on OSX
author: Brad McCormack
type: post
date: 2012-05-15T23:36:12+00:00
url: /operating-systems/change-disk-sleeping-on-osx/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363630794
categories:
  - Apple
  - Information Technology
  - Operating Systems

---
I&#8217;m still learning OSX each day and this morning I finally had enough of the USB drive spinning up and down. To modify this open up a terminal and type the following
  
[code language=&#8221;bash&#8221;]
  
sudo pmset -a disksleep 0
  
[/code]

This basically means never put the disk to sleep for all profiles (battery, plugged in etc).
  
Type
  
[code language=&#8221;bash&#8221;]
  
man pmset
  
[/code]

to find out more.