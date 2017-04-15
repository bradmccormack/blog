---
title: Installing VMware Tools in a Linux guest for Vsphere/esx(i)
author: Brad McCormack
type: post
date: 2012-06-24T00:40:11+00:00
url: /operating-systems/installing-vmware-tools-in-linux-guest-for-vsphereesxi-deb-based-distro/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363604838
categories:
  - Information Technology
  - Linux
  - Operating Systems
  - Virtualization

---
I noticed that a Linux VM at my work wasn&#8217;t running as smoothly as it could be. I confirmed that it was not using VMWare tools.

To install follow these steps (Please note step 13 is only applicable to Debian based distros&#8217;)

  1. Login to the VMware Infrastrcture Client (Vsphere Client)
  2. Select the VM under nodes listed under the host by left clicking
  3. Click Inventory -> Virtual Machine -> Install/Upgrade Vmware Tools
  4. SSH/Telnet into the guest
  5. Mount the cdrom that was placed into the &#8220;drive&#8221; in step 3 [code language=&#8221;bash&#8221;] mount /dev/cdrom /media/cdrom [/code]
  6. Copy from the cd and extract the file by doing the following
  
    [code language=&#8221;bash&#8221;]
  
    cd /media/cdrom
  
    cp VMwareTools\*.tar.gz /tmp && cd /tmp && tar -xzvf VMwareTools\*.tar.gz
  
    umount /media/cdrom
  
    cd vmware-tools-distrib
  
    ./vmware-install.pl
  
    //Select yes and default locations to all the questions (unless you require specific answers)
  
    [/code]</p> 
      * It should eventually install the drivers or complain about missing headers. If that is the case do the following
  
        [code language=&#8221;bash&#8221;] apt-get install linux-headers-$(uname -r) [/code] </ol> 
    For all you IT/Network Admins don&#8217;t forget to give the Linux/Unix Vm&#8217;s some virtualization love too 🙂