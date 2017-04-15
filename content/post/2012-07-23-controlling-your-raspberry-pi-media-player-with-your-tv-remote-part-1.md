---
title: Controlling your Raspberry Pi media player with your TV Remote – Part 1
author: Brad McCormack
type: post
date: 2012-07-23T09:22:02+00:00
url: /operating-systems/controlling-your-raspberry-pi-media-player-with-your-tv-remote-part-1/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363538042
categories:
  - Development
  - Information Technology
  - Linux
  - Operating Systems
  - Raspberry Pi

---
I&#8217;ve recently been using my [Raspberry pi][1] for watching movies using [XBMC][2] via the excellent &#8220;distro&#8221; [RaspBMC][3].

At first it wasn&#8217;t ideal to have to use a keyboard to navigate XBMC.I then found out about some XBMC remote apps for our Iphone and Android smart phones. However I still wanted something more natural to use (especially for my wife).

## 1. Understand the technology

I performed some research and found out about a cool technology that was defined in the [HDMI Specification 1.0 and updated in HDMI 1.2, HDMI 1.2a and HDMI 1.3a][4]. Following more research I then found out that HDMI CEC is supported in the [VideoCore IV GPU][5]. See the [Datasheet here][6] and the [Vendor&#8217;s (Broadcom) Page][7] here.

Now that I knew I was right to go I hunted around to see if there was anything already out there that I could work off. In my opinion standing on the shoulders of giants is generally the right approach to take. It saves you time and heartache.

## 2. Look for existing Source

To my satisfaction, there was already [Source][8] that was in a working state (Tested OK) , that I could learn off. After testing the code and seeing that it worked (My Sony Bravia remote controlled XBMC via the Pi using HDMI CEC) the next stage was to [fork the repo][9].

To build the source, perform the following steps ([according to the instructions on the intial repo][10] (Note I normally use [Arch Linux][11] but this build was conducted on [Ubuntu 12.04 LTS][12] to appeal to the widest audience &#8211; and be more likely to work as existing documentation is more likely.

  1. [Get the Raspberry Pi firmware and point to it for later][13] 
      * Clone the code
  
        [code language=&#8221;bash&#8221;]
	  
        git clone https://github.com/raspberrypi/firmware.git
	  
        export $SDKSTAGE= {full absolute) location you cloned to above
  
        [/code] 
      * [Build Open Elec to get the toolchain][14] 
        [code language=&#8221;bash&#8221;]
  
        git clone git://github.com/OpenELEC/OpenELEC.tv.git
  
        sudo apt-get install g++ nasm flex bison gawk gperf autoconf automake m4 cvs libtool byacc texinfo gettext zlib1g-dev libncurses5-dev curl libcurl4-gnutls-dev
  
        git-core build-essential xsltproc libexpat1-dev autopoint
  
        sudo perl -e shell -MCPAN install XML::Parser
  
        PROJECT=RPi ARCH=arm make
  
        [/code]
        
          * (Wait a few hours depending on your hardware)</ul> 
      * Find the location of the cross-compiler and set to use it 
        [code language=&#8221;bash&#8221;]
  
        find ./ -name armv6zk-openelec-linux-gnueabi-gcc./build.OpenELEC-RPi.arm-devel/toolchain/bin/armv6zk-openelec-linux-gnueabi-gcc
  
        export CC={path found above such as} /OpenELEC.tv/build.OpenELEC-RPi.arm-devel/toolchain/bin/armv6zk-openelec-linux-gnueabi-gcc
  
        echo $CC (to make sure the path is set correctly
  
        [/code]</ol> 
    
    ## 3. Building the source
    
    Now that the build environment is set-up the first thing we should do is make sure the stock code builds. Simply make sure you are in the location that you cloned for instance
    
      * https://github.com/bradmccormack/rpi-cecd
    
    &nbsp;
    
    #### Stay tuned for more info !
    
    &nbsp;

 [1]: http://www.raspberrypi.org/
 [2]: http://xbmc.org/
 [3]: http://www.raspbmc.com/
 [4]: http://en.wikipedia.org/wiki/HDMI#CEC
 [5]: http://www.broadcom.com/products/technology/mobmm_videocore.php
 [6]: http://www.element14.com/community/docs/DOC-43016/l/broadcom-datasheet-for-bcm2835-soc-used-in-raspberry-pi
 [7]: http://www.broadcom.com/products/BCM2835
 [8]: https://github.com/olajep/rpi-cecd
 [9]: https://github.com/bradmccormack/rpi-cecd/
 [10]: https://github.com/bradmccormack/rpi-cecd/blob/master/README.md
 [11]: http://www.archlinux.org/
 [12]: http://releases.ubuntu.com/precise/
 [13]: https://github.com/raspberrypi/firmware
 [14]: http://wiki.openelec.tv/index.php?title=Building_and_Installing_OpenELEC_for_Raspberry_Pi