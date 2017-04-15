---
title: RSync files for easy backup
author: Brad McCormack
type: post
date: 2012-03-02T23:49:44+00:00
url: /uncategorized/rsync-files-for-easy-backup/
dsq_thread_id:
  - 3406297244
categories:
  - Information Technology
  - Operating Systems
  - Uncategorized

---
I figured I should probably backup some of our important family photos. I&#8217;ve got a trusty WDTVLive sitting in the lounge room running WDLXTV custom firmware with a 2TB drive attached waiting.

It&#8217;s ideal to have a little device like this always on and ready to go. You have to love Arm processors and their low power requirements 🙂  (I&#8217;m currently waiting on a few Rasperry Pi to be delivered )

Make sure you have a share available on the target machine (I&#8217;m using Samba in this case as it is shared with a Windows environment)

The command to run is ..

rsync &#8211;progress &#8211;partial -avz /Source/Volume user@ip:/mount/point

Simple as that! If you want to delve into other parameters and options use your favorite search engine and lookup the many great RSYNC guides that are already out there.

Next time you run the same command it will to a diff (find out what has changed) and only update the changed data which will be much faster!

&nbsp;

&nbsp;