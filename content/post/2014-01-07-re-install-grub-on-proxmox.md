---
title: Re-install GRUB on Proxmox
author: Brad McCormack
type: post
date: 2014-01-07T02:23:42+00:00
url: /operating-systems/re-install-grub-on-proxmox/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363440183
categories:
  - Information Technology
  - Linux
  - Operating Systems

---
#### Foreword

I have [another article on setting up Proxmox and ZFS][1]. Please see it for an introduction.

There is currently a bug (in my opinion) where Proxmox always installs GRUB on the MBR of the first active disk. In my case it was the SSD which I in turn, had made the l2ARC for my ZFS pool (which happened to destroy the boot loader).

#### Reinstall Grub

Boot into a Linux distribution that is 64 bit (We need the same architecture as Proxmox so we can [chroot][2] into it)

Proxmox sets up two Primary partitions, one primary partition with an ext filesystem for /boot and the other, an LVM primary partition on the install medium.

First We need to activate LVM and mount the the root partition that is inside the LVM container.

[code language=&#8221;bash&#8221;]
  
sudo vgscan
  
sudo vgchange -ay
  
[/code]

Now create some mount points that are applicable for you (it could be off /mnt or /media in your distro or wherever you would like) and mount all file-systems (virtual too).

##### Note

The LVM device nodes will be located under /dev/pve.

Please check the location of the device you want to restore grub onto and mount /boot from. In my case it is /dev/sdc which is a spare USB flash disk. You can also reference it via /dev/disk/by-id or /dev/disk/by-uuid.
  
[code language=&#8221;bash&#8221;]
	  
sudo mkdir /media/USB
	  
sudo mount /dev/pve/root /media/USB/
	  
sudo mount /dev/sdc1 /media/USB/boot
	  
sudo mount -t proc proc /media/USB/proc
	  
sudo mount -t sysfs sys /media/USB/sys
	  
sudo mount -o bind /dev /media/USB/dev
	  
sudo mount -t devpts pts /media/USB/pts/
  
[/code]

#### Now its time to chroot into the system

[code language=&#8221;bash&#8221;]
	  
chroot /media/USB
  
[/code]

#### Finally, re-install Grub

[code language=&#8221;bash&#8221;]
  
/sbin/grub-setup /dev/sdc
  
[/code]

You should receive no error messages. Now it&#8217;s time to reboot that flash disk and you are back in business 🙂

 [1]: http://www.nerdoncoffee.com/?p=495
 [2]: http://www.tuxation.com/chrooting-into-a-linux-environment.html