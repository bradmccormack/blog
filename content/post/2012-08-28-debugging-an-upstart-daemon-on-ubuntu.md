---
title: Debugging an Upstart Daemon on Ubuntu
author: Brad McCormack
type: post
date: 2012-08-28T09:23:06+00:00
url: /operating-systems/debugging-an-upstart-daemon-on-ubuntu/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363537480
categories:
  - Linux
  - Operating Systems

---
I was trying to get Hubot to work as an upstart daemon so I installed upstart.

I then placed the script to make an upstart daemon into /etc/init/hubot.conf
  
<a href= https://github.com/github/hubot/wiki/Deploying-Hubot-onto-UNIX >Hubot Script example </a> </br> 

However upon trying to start the daemon I noticed it was not running after consulting ps aux | grep node.

I was wondering how to debug this and realized I could just tail the syslog.

To debug starting the daemon do the following as an appropriately privileged user.

[code language=&#8221;bash&#8221;]
  
tail -f /var/log/syslog
  
[/code]

Then checkout the error. Such as .. init: /etc/init/hubot.conf:6: Unknown Stanza which pointed me in the right direction straight away.