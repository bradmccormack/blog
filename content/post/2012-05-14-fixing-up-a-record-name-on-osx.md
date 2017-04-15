---
title: Fixing up a Record name on OSX
author: Brad McCormack
type: post
date: 2012-05-14T22:53:47+00:00
url: /operating-systems/fixing-up-a-record-name-on-osx/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363632517
categories:
  - Apple
  - Information Technology
  - Operating Systems

---
So I screwed up the Record name entry when I created my profile. The following steps fix it !
  
1) enable root http://support.apple.com/kb/ht1528
  
2) login as root open up a terminal and type the following
  
[code language=&#8221;bash&#8221;]
  
su
  
dscl . -change /Users/username RecordName oldrecordname newrecordname
  
[/code]