---
title: Hacking around with a Netgear router
author: Brad McCormack
type: post
date: 2012-06-24T01:16:38+00:00
url: /operating-systems/hacking-around-with-a-netgear-router/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363585749
categories:
  - Information Technology
  - Linux
  - Networking
  - Operating Systems

---
I&#8217;m bored and my daughter is behaving in the next room watching Bannana&#8217;s In Pijammas. I felt like doing a bit of research into getting information about some of my various routers. I&#8217;ve used various Open Source firmware alternatives such as Tomato, OpenWRT and DDWRT but I&#8217;ve never had the desire to find out more detailed information about the actual hardware that runs them (apart from cursory searches on the internet and looking at the boards inside the enclosure themselves).

Usually It&#8217;s available on the internet somewhere but I was curious how I would learn this myself.

I won&#8217;t be divulging any information about this particular router (as I&#8217;m under NDA as part of testing) but I can certainly post the instructions I used on my little boredom quest.

### Step 1 &#8211; Enabling Telnet on the router

Well, for this it was a matter of pointing my browser at the router with a hidden url suffix

<a href="http://192.168.0.1/setup.cgi?todo=debug" target="_blank">http://192.168.0.1/setup.cgi?todo=debug</a>

### Step 2- Telnetting in to the router

Open up a terminal/shell or whatever cli you use on your particular operating system (note Windows disables Telnet by default) and type
  
[code language=&#8221;bash&#8221;]
  
telnet 192.168.0.1
  
[/code]

Following that, you will be prompted to enter the credentials you actually use to log in to the router via the browser.

### Step 3 &#8211; Information Gathering with the tools at hand

I have some experience, but not much in regards to gathering information about the hardware. The following commands will get you various types of information.

The following will get some information about the CPU/Arch

[code language=&#8221;bash&#8221;]
  
cat /proc/cpuinfo
  
[/code]

The following will get some information about System Memory

[code language=&#8221;bash&#8221;]
  
cat /proc/meminfo
  
[/code]

The following will get some information about the Kernel version (note uname was not available in this environment)

[code language=&#8221;bash&#8221;]
  
cat /proc/version
  
[/code]

To get some information about the version of busybox that is running and the commands it supports.
  
[code language=&#8221;bash&#8221;]
  
busybox
  
[/code]

To get some information about the processes that are running< [code language="bash"] ps aux [/code] To obtain some information about the network interfaces [code language="bash"] ifconfig -a [/code] 

###  Step 4 &#8211; Information Gathering with a newer and improved version of busybox

I found out that to get even more information such as controller vendors and so on (that I could further research),  I needed to use lspci and lsusb. These didn&#8217;t come with the embedded/shipped version of busybox. To obtain a newer one use the following commands.
  
[code language=&#8221;bash&#8221;]
  
cd /tmp
  
mkdir busy && cd busy
  
wget http://busybox.net/downloads/binaries/latest/busybox-mips
  
chmod +x ./busybox-mips
  
./busybox-mips
  
[/code]
  
You now see that the version of busy box is newer and all of the commands are now available.

To obtain pci vendor information
  
[code language=&#8221;bash&#8221;]
  
./busybox-mips lspci
  
[/code]
  
You will get some input similar to
  
[code language=&#8221;bash&#8221;]
  
05:00.0 0200: 14e4:1659
  
[/code]

For my purposes I&#8217;m only interested in the last two values on the right with a colon delimiter.

  * The first is the vendor.
  * The Second is the device ID.

Now you can use a service such as [PCI Database][1] to look up the vendor and device information.

To obtain USB vendor information
  
[code language=&#8221;bash&#8221;]
  
./busybox-mips lsusb
  
[/code]

You should also be able to lookup the vendor/device ID information using a similar service such as [PCI IDs][2]

Hopefully this little post has entertained somebody out there and provides them with a foundation to do their own investigation and diagnostics.

Enjoy 🙂

### References

  * http://prefetch.net/articles/linuxpci.html
  * http://forums.whirlpool.net.au/archive/1098853
  * https://forum.openwrt.org/viewtopic.php?id=32631

 [1]: http://www.pcidatabase.com/
 [2]: http://pci-ids.ucw.cz/read/PD