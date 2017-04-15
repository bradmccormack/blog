---
title: The importance of power output for the Raspberry Pi
author: Brad McCormack
type: post
date: 2012-08-18T10:12:01+00:00
url: /operating-systems/the-importance-of-power-output-for-the-raspberry-pi/
dsq_thread_id:
  - 3363537693
categories:
  - Linux
  - Operating Systems
  - Raspberry Pi

---
I haven&#8217;t had much time to work on various development projects I have in mind for various things such as HDMI Cec work for the Pi (Although LibCEC is now integrated so my efforts/intentions are obsolete it looks like) due to university requirements, but I have had time to enjoy the occasional movie with my wife using XBMC on the PI.

Unfortunately over the last few weeks I&#8217;ve been experiencing more and more haphazard experiences. Network dropping out, SMB shares being problematic. Locking up of the user interface and so on. Not a happy time.

I&#8217;m a software guy first and foremost. I love to develop and learn about all sorts of Operating Systems and techniques. This led me down various tracks such as trying various &#8220;Disto&#8217;s&#8221; for the Pi such as RaspBMC, OpenElec and Xbian. I placed the blame on updated versions of these distros and kept trying to find a stable experience. Xbian seemed to be the best but still had many problems.

I continued to diagnose the situation and noticed the network activity on the router was continuous. So of course I start delving into the router and its TomatoUSB open source based firmware for a fix. Various tweaks resulted in no improvement. I ended observing the output of ifconfig and the RX error count really jumped out at me. Bizzare I thought.

I researched around and ended up finding the fault was the power adapter I was using for the Pi. At some stage I had plugged in a 5.2v 650mA and I also had a USB keyboard plugged in. This is certainly not enough current to power the LAN, Usb keyboard and all the other tasks the Pi was performing GPU Decoding and so on).

Long story short, finding a 5.3V,2 amp adapter removed ALL of my problems instantly. Whilst 5V 1.0A is probably better, the Pi should simply draw what it needs to.

&nbsp;

For those that care,  I&#8217;m using the [HP Touchpad][1] adapter. When I need to charge the Touchpad (Running CM9 🙂 ) I decommision the Pi for a couple of hours.

If you are experiencing weird issues with your PI, check the adapter !

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

###

 [1]: http://www.ardamis.com/2012/06/06/finding-a-power-supply-for-the-raspberry-pi/