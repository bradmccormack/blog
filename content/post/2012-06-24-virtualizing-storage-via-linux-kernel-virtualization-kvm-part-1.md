---
title: Virtualizing Storage via Linux Kernel Virtualization (KVM) – Part 1
author: Brad McCormack
type: post
date: 2012-06-24T01:04:50+00:00
url: /operating-systems/virtualizing-storage-via-linux-kernel-virtualization-kvm-part-1/
dsq_thread_id:
  - 3363604118
categories:
  - Information Technology
  - Linux
  - Operating Systems
  - Virtualization

---
Disclamer &#8211; I&#8217;m don&#8217;t have an enterprise budget to play with really cool fiber SAN&#8217;s and so on. This series is an expirment on a budget.

It probably seems strange that storage should be virtualized. It can, but should it? This post won&#8217;t go into if it should or shouldn&#8217;t but begin to analyse how it can be virtualized efficiently using the virtualization technologies built into the Linux kernel. It depends on your requirements and resources if it should be virtualized which is outside of the scope of this post.

For the purposes of this post [KVM][1] will be considered. There are competing technologies that can (and should) be considered available for Linux such as full virtualization (Xen) and OS Level virtualization (LXC &#8211; Linux Containers &#8211; Akin to Zones in Solaris and Jails in BSD&#8217;s as far as I understand with my limited exposure) and of course proprietary solutions such as VMWares Workstation, Oracle&#8217;s Virtualbox and others. Not to be confused with some emulation products such as Bochs, KEmu and similar (Please correct me If I am wrong).

For my purposes, I want a solution that offers tangible benefits such as simplicity, redundance , performance and my favourite &#8211; tweakability (to achieve the previous facets). I&#8217;ve performed some preliminary research and have investigated the following solutons.

  * [Nexenta Stor (Open Solaris Based)][2]   
  * [FreeNas (FreeBSD Based)][3]  
  *  [Disclamer &#8211; I&#8217;m don&#8217;t have an enterprise budget to play with really cool fiber SAN&#8217;s and so on. This series is an expirment on a budget.

It probably seems strange that storage should be virtualized. It can, but should it? This post won&#8217;t go into if it should or shouldn&#8217;t but begin to analyse how it can be virtualized efficiently using the virtualization technologies built into the Linux kernel. It depends on your requirements and resources if it should be virtualized which is outside of the scope of this post.

For the purposes of this post [KVM][1] will be considered. There are competing technologies that can (and should) be considered available for Linux such as full virtualization (Xen) and OS Level virtualization (LXC &#8211; Linux Containers &#8211; Akin to Zones in Solaris and Jails in BSD&#8217;s as far as I understand with my limited exposure) and of course proprietary solutions such as VMWares Workstation, Oracle&#8217;s Virtualbox and others. Not to be confused with some emulation products such as Bochs, KEmu and similar (Please correct me If I am wrong).

For my purposes, I want a solution that offers tangible benefits such as simplicity, redundance , performance and my favourite &#8211; tweakability (to achieve the previous facets). I&#8217;ve performed some preliminary research and have investigated the following solutons.

  * [Nexenta Stor (Open Solaris Based)][2]   
  * [FreeNas (FreeBSD Based)][3]  
  *][4] 

Whilst I&#8217;ve only ever had Nexenta Stor used in production systems I decided it was worth investigating alternatives  for my use-case.

The first one to be eliminated was OpenMediaValult. Whilst it looks excellent, one of the mandatory requirements is ZFS. I can go on a spiel why I chose ZFS over others but I&#8217;ll cut-to-the-chase and admit I&#8217;m happy with its performance, flexiblity and I down right want to learn more about it and I don&#8217;t think the Linux alternative BTRFS <a />is ready for prime time yet.</p> 

That leaves me with Nexenta Stor and FreeNas.

As anyone who is envolved with virtualized storage is aware one of the paramount requirements is performance (along with Security, Scalabity and so-on) and to achieve this in a virtualized environment it is pretty obvious that the guest operating system must have para-virtualized drivers for I/O related tasks or the whole tasks of Storage (I/O) will become non-applicable due to abysmal performance.

I guess my first stop is investigating the state of paravirtualization for KVM and guests. A nice start is  [IBM&#8217;S Page][5] to help me/you understand some preliminaries about KVM 

The next appropriate stop is attaining the current state of guest support for VirtIO.  The best place for this is [here][6]. This gives me some helpful advice on where to start.

Stay tuned for further parts in this series of investigation and generally playing around and having fun!

_Some notes for me and TODO&#8217;s for the future._

  * _It appears there is a virtIO driver for BSD  http://people.freebsd.org/~kuriyama/virtio/  (use wget (comes with freenas) to install the pkg. Edit fstab etc.)_

  * _do benchmarks against emulated hardware (eg Intel e1000) versus paravirtualized IO driver. Also investigate passing through Sata controllers directly using Intel Vt-d._

  * _Provide some references to why virtualize storage ( This is for my own sanity too)_

&nbsp;

&nbsp;

&nbsp;

 [1]: http://www.linux-kvm.org/page/Main_Page
 [2]: http://nexentastor.org/
 [3]: http://www.freenas.org/
 [4]: http://www.openmediavault.org/
 [5]: http://www.ibm.com/developerworks/linux/library/l-virtio/index.html?ca=dgr-lnxw07Viriodth-LX&S_TACT=105AGX59&S_CMP=grlnxw07.
 [6]: http://www.linux-kvm.org/page/Guest_Support_Status