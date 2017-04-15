---
title: Setting up a caching proxy on home router running OpenWRT – Part 1
author: Brad McCormack
type: post
date: 2012-05-13T03:03:13+00:00
url: /operating-systems/setting-up-a-caching-proxy-on-home-router-running-openwrt-part-1/
dsq_thread_id:
  - 3364004373
categories:
  - Information Technology
  - Linux
  - Networking
  - Operating Systems

---
### Why do this?

&nbsp;

  * An academic exercise. I really like to learn and see how I can push my hardware. Hopefully my education will assist me elsewhere.
  * Fun factor &#8211; I&#8217;m a computer nerd. I find it challenging and rewarding.
  * Save on bandwidth used , increase performance and do it all for minimal cost

&nbsp;

### Software/Hardware used

&nbsp;

  * OpenWrt Attitude Adjustment r31576 [ (GET IT HERE) ][1]
  * Polipo Caching proxy <http://www.pps.jussieu.fr/~jch/software/polipo/>
  * Netgear WNDR3700 <http://www.netgear.com.au/home/products/wirelessrouters/high-performance/wndr3700.aspx>

&nbsp;

### Starting off

&nbsp;

  * Check the compatibility of your hardware with those listed as recommendations by OpenWRT. For the purposes of this article you can be assured of comptability as I&#8217;ve already done that for you 🙂

&nbsp;

### Guide

&nbsp;

#### Install OpenWRT

  1. Download the latest build of OpenWRT for your device. For the Netgear 3700 you can [GET IT HERE][1]. Please use a search engine to determine the correct version to download for your Netgear WNDR3700. For my purposes and region I have elected to use openwrt-ar71xx-generic-wndr3700-squashfs-factory-NA.img
  2. Use the default Netgear interface to flash the firmware (below).  Please click the Browse button and navigate to the file you have selected. 
    [<img class="aligncenter size-full wp-image-121" title="netgear-WNDR3700-Firmware" src="http://www.nerdoncoffee.com/wp-content/uploads/2012/05/netgear-WNDR3700-Firmware-2.jpg" alt="netgear-WNDR3700-Firmware Upgrade Picture" />][2]</li> </ol> 
    
    &nbsp;
    
    #### Mount a flash drive for extra storage on your router
    
    &nbsp;
    
    #### Setup your modem in bridge mode and configure the wan port on your router to use PPOE/PPOA authentication
    
    &nbsp;
    
    #### Install Polipo (optware)
    
    &#8211; Test Polipo by setting the proxy in your web browser.
    
    #### Add rewrite rules for iptables so the proxying is transparent
    
    &nbsp;
    
    ### Applicable pages on your network
    
      * Polipo Configuration at runtime <http://192.168.0.70:8123/polipo/config>
      * Polipo Status <http://192.168.0.70:8123/polipo/status>
    
    &nbsp;
    
    &nbsp;
    
    #

 [1]: http://enduser.subsignal.org/~trondah/latest-release-is-r31602/
 [2]: http://www.nerdoncoffee.com/wp-content/uploads/2012/05/netgear-WNDR3700-Firmware-2.jpg