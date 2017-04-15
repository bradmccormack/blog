---
title: Not enough time in a day
author: Brad McCormack
type: post
date: 2012-04-29T13:15:51+00:00
url: /operating-systems/not-enough-time-in-a-day/
dsq_thread_id:
  - 5553446442
categories:
  - Development
  - Linux
  - Operating Systems
  - Random

---
So today I had many aspirations &#8230;

### Get grub updated to Grub 2 on Arch Linux (Done)

  * <pre>(Something went wrong here and I had to find a spare Arch Live CD laying around and Chroot into the existing install .... something along the lines of ..</pre>

  * <pre>mkdir /media/arch</pre>

  * <pre>mount /dev/sda1 /media/arch</pre>

  * <pre>mount -t proc /proc /media/arch/proc</pre>

  * <pre>mount -t sysfs /sys /media/arch/sys</pre>

  * <pre>mount -o bind /dev /media/arch/dev</pre>

  * <pre> chroot /media/arch /bin/bash</pre>
    
    <pre>(N.B Don't use a live cd of a different cpu architecture than the chrooted environment (a.ka X86 vs X86-64) You will have issues.

</pre>

### Get a new kernel supporting SFS or at least a module ready so that I could chain load Aros via Grub 2 (!Done)

  * It seems I&#8217;ll have to re-partition to get around Aros&#8217;s short-comings in regards to partitioning schemes and Grub. If you are wondering what Aros is take a look here [So today I had many aspirations &#8230;

### Get grub updated to Grub 2 on Arch Linux (Done)

  * <pre>(Something went wrong here and I had to find a spare Arch Live CD laying around and Chroot into the existing install .... something along the lines of ..</pre>

  * <pre>mkdir /media/arch</pre>

  * <pre>mount /dev/sda1 /media/arch</pre>

  * <pre>mount -t proc /proc /media/arch/proc</pre>

  * <pre>mount -t sysfs /sys /media/arch/sys</pre>

  * <pre>mount -o bind /dev /media/arch/dev</pre>

  * <pre> chroot /media/arch /bin/bash</pre>
    
    <pre>(N.B Don't use a live cd of a different cpu architecture than the chrooted environment (a.ka X86 vs X86-64) You will have issues.

</pre>

### Get a new kernel supporting SFS or at least a module ready so that I could chain load Aros via Grub 2 (!Done)

  * It seems I&#8217;ll have to re-partition to get around Aros&#8217;s short-comings in regards to partitioning schemes and Grub. If you are wondering what Aros is take a look here ][1] 
  * [I][1]&#8216;ll come back to you later Aros 🙁

### Put my first attempts at an Md5 OpenCL based hasher onto github.

  * I&#8217;m still going with this. I&#8217;m quite embarrassed with my current attempt. I&#8217;ve spent too long with C#, Java, Python and other higher level languages. It needs slightly more auditing. I&#8217;m aiming for the following phases
  * 1) Simple single core md5 hasher (I&#8217;ll minimize function calls with function based macro&#8217;s and inline functions where applicable ( and any other nieve optimization&#8217;s I can think of).
  * 2) Posix Threads hasher
  * 3) OpenCl hasher

I&#8217;m still quite a beginner and I don&#8217;t think step 2 will be necessary. Perhaps it will, but not in the respect of my initial implementation intentions. I garner that at least a dedicated thread will need to lay around awaiting requests to hash certain files and feed them to OpenCL kernels.

Perhaps I&#8217;ll try various compilers too and see how it goes. I&#8217;ve got a few in mind such as

  * GCC
  * Intel Compiler
  * Digital Mars
  * Microsoft Compiler

<div>
  I&#8217;m open to suggestion here.  Obviously I&#8217;ll try various compiler/linker settings and see how it goes. Does gcc -o3 really improve things over -o2 ? Probably not in the grand scheme of things due to binary size.
</div>

I&#8217;ve got a proclivity to keep heading towards my goals even though I don&#8217;t have enough time and today I found some useful sites to reinforce my knowledge. I good series of OpenCL articles I found on

  * <http://www.codeproject.com/Articles/110685/Part-1-OpenCL-Portable-Parallelism>

<div>
  I figure I&#8217;m going to have my work cut out for me in regards to workload size and transfers to the computing device. I&#8217;ll probably have to evaluate the size of the workload against the average time of transfer to the compute device as sometimes the transfer over the bus precludes any computation at all. The time to transfer over earlier buses to older OpenCL compatible devices negates any transfer at all and is best done by a posix thread or other.
</div>

<div>
</div>

<div>
  That&#8217;s enough for today. I&#8217;m quite burnt out. I&#8217;ve got a wife and two daughters. They challenge me more than all my IT and Development hobbies combined. Especially the my youngest whom is 2.5 years old 🙂
</div>

<div>
</div>

<div>
  Note &#8211; I&#8217;ve spent quite a while with the in-laws tonight and I consider myself a very lucky man. I&#8217;m exceedingly lucky to call them mum and dad !
</div>

<div>
</div>

<div>
</div>

&nbsp;

&nbsp;

 [1]: http://aros.sourceforge.net/