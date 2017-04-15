---
title: Fixing bootmgr is missing for Windows 7
author: Brad McCormack
type: post
date: 2012-01-30T13:09:14+00:00
url: /operating-systems/fixing-bootmgr-is-missing-for-windows-7/
dsq_thread_id:
  - 3580555551
categories:
  - Operating Systems
  - Windows

---
For some reason I was bored and felt like playing a game. Normally I’ll
  
just boot into Arch Linux or the Hackintosh (I’m not much of a gamer).
  
Unlucky for me I got the dreaded “bootmgr is missing message”.

Sigh. It’d probably be as simple as booting the Windows 7 disc and
  
Choosing Startup repair. Or so i thought… I gave that a crack and failed.
  
Then I went down the bootrec /fixboot bootrec /RebuildBcd path and they
  
too failed. I found it quite interesting that the Windows Install wasn’t
  
showing up in the System Recovery Options as well.

The fix

Boot into GParted and mark the partition as bootable (how did this
  
get messed up ?)
      
Start the System Recovery again from the Windows 7 install disc
      
Follow the snippet from Microsoft’s site using the console in System
  
Recovery.
          
bcdedit /export C:\BCD_Backup
          
c:
          
cd boot
          
attrib bcd -s -h -r
          
ren c:\boot\bcd bcd.old
          
bootrec /RebuildBcd
      
Reboot successfully 🙂

Somewhere along the way I busted something when working with a triple boot
  
setup and I hadn’t been into Windows for a while so I didn’t notice. I
  
hope this helps somebody.