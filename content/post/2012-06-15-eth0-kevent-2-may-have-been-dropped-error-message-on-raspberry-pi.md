---
title: 'eth0: kevent 2 may have been dropped  error message on Raspberry Pi'
author: Brad McCormack
type: post
date: 2012-06-15T21:10:55+00:00
url: /operating-systems/eth0-kevent-2-may-have-been-dropped-error-message-on-raspberry-pi/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363618716
categories:
  - Information Technology
  - Operating Systems
  - Raspberry Pi

---
If you have received the error message eth0: kevent 2 may have been dropped whilst doing large network operations on your Pi please try the following.

[code language=&#8221;bash&#8221;]
  
ssh user@yourpihost
  
sudo bash -c &#8220;echo vm.min\_free\_kbytes = 16384 (or higher). >> /etc/sysctl.conf&#8221;
  
sudo sysctl -p
  
[/code]