---
title: Tuning GCC on Arch
author: Brad McCormack
type: post
date: 2012-07-20T23:46:55+00:00
url: /operating-systems/tuning-gcc-on-arch/
dsq_thread_id:
  - 3363574697
spacious_page_layout:
  - default_layout
categories:
  - Linux
  - Operating Systems

---
I&#8217;ve recently got some new computer parts. I figured I should probably update the global compiler settings. In Arch linux you can do the following

[code language=&#8221;bash&#8221;]
  
sudo vim /etc/makepkg.conf
  
[/code]
  
For my particular CPU (Core i7 3770 (Non K as I wanted Vt-D functionality) use the following settings

[code language=&#8221;bash&#8221;]
  
CFLAGS=&#8221;-march=core-avx-i -mtune=core-avx-i -O2 -pipe -fstack-protector &#8211;param=ssp-buffer-size=4 -D\_FORTIFY\_SOURCE=2&#8243;
  
[/code]

You can also un-comment MAKEFLAGS and set it to use more/all cores such as

[code language=&#8221;bash&#8221;]
  
MAKEFLAGS=&#8221;-j8&#8243;
  
[/code]

For a list of supported options check out  [http://en.gentoo-wiki.com/wiki/Safe_Cflags/][1]  and <http://gcc.gnu.org/onlinedocs/gcc/i386-and-x86_002d64-Options.html>

* Note &#8211; modifying MAKEFLAGS may break building on some packages. Use with caution

 [1]: http://en.gentoo-wiki.com/wiki/Safe_Cflags/Intel#Core_i7_and_Core_i5.2C_Xeon_55xx