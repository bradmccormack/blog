---
title: Setting up a File Server and Hypervisor using Proxmox and ZFS
author: Brad McCormack
type: post
date: 2014-01-07T02:13:56+00:00
url: /operating-systems/setting-up-a-file-server-and-hypervisor-using-proxmox-and-zfs/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363450736
categories:
  - Hosting
  - Information Technology
  - Linux
  - Networking
  - Operating Systems

---
<img class="aligncenter" src="https://www.proxmox.com/images/proxmox/proxmox-logo.png" alt="proxmox-logo" />
  
<img class="aligncenter" src="http://www.nerdoncoffee.com/wp-content/uploads/2012/09/zfs_logo.jpg" alt="ZFS Logo" width="400" height="320" />

* * *

<div id="zfs">
  <h4>
    About ZFS
  </h4>
  
  <p>
    ZFS is a modern filesystem designed to be extremely flexible, powerful, easy to use and administer. There are already many great articles that go into depth which I will leave you to explore. Please see the <a href="#references">References</a>
  </p>
</div>

#### About Proxmox

Proxmox is a great set of packages to help you manage your virtual machines. Proxmox simplifies a lot of management for virtual machines and can handle High availability, Clusters and much more. Behind the scenes it is using [KVM][1] for full virtualization and [OpenVZ][2] containers for high performance, lightweight OS-Level virtualization.

Check the following for more detail

<https://www.proxmox.com/proxmox-ve/features>

##### Notes

I came from ESXI and HyperV. I&#8217;m finding that I&#8217;m continuing to adopt open source solutions primarily as I don&#8217;t have any vendor lock in and to increase my skill-set.

<div id="installingproxmox">
  <h4>
    Installing Proxmox
  </h4>
  
  <p>
    Install Proxmox onto your desired device. I used a spare USB flash drive. Most of the time, Proxmox will be running from memory so I don&#8217;t mind a slow install if it frees up a much faster disk for storage.
  </p>
  
  <p>
    The only problem I came across is Proxmox installs the GRUB bootloader onto the first available disk rather than the selected disk. If anything it should not install a bootloader at all unless you ask it to, and if so only where you ask it to. It wiped my existing bootloader on the MBR so I had to chroot into the system and restore the bootloader. <a href="https://bugzilla.proxmox.com/show_bug.cgi?id=464">This bug has been reported</a>. I suggest for now if possible to disable all drives in the bios except for the drive that you want Proxmox installed onto temporarily.
  </p>
</div>

<div id="updateproxmox">
  <h4>
    Update Proxmox
  </h4>
  
  <p>
    <a href="http://pve.proxmox.com/wiki/Package_repositories">http://pve.proxmox.com/wiki/Package_repositories</a>
  </p>
  
  <p>
    Follow the instructions from <a href="http://pve.proxmox.com/wiki/Package_repositories#Proxmox_VE_No-Subscription_Repository">http://pve.proxmox.com/wiki/Package_repositories#Proxmox_VE_No-Subscription_Repository</a> if you have not purchased a subscription from Proxmox.
  </p>
</div>

<div id="installzfsonlinux">
  <h4>
    Install ZFS on Linux
  </h4>
  
  <p>
    <a href="http://ispire.me/native-zfs-for-linux-on-proxmox/">http://ispire.me/native-zfs-for-linux-on-proxmox/</a> is an excellent to-the-point article that will get you going. You may also look at the official article on <a href="http://pve.proxmox.com/wiki/ZFS">http://pve.proxmox.com/wiki/ZFS</a>.
  </p>
  
  <p>
    I chose to go with a raidz mode with 4 spare 400GB (approx) hard drives I had laying around. This gives me a good balance between redundancy and performance. I also decided to leave the pool name as tank.
  </p>
  
  <h5>
    Note
  </h5>
  
  <ul>
    <li>
      Make sure you have the correct repositories enabled before you install the SPL and ZFS modules as versions can get out of sync with the running kernel.
    </li>
    <li>
      I strongly suggest you use the /dev/disk/by-id prefix to your devices to avoid confusion when remapping devices in your bios e.g the following in my case<br /> [code language=&#8221;bash&#8221;]<br /> zpool create -f tank raidz /dev/disk/by-id/ata-Hitachi_HDP725032GLA380_GEK033RG2Z160C /dev/disk/by-id/ata-SAMSUNG_HD321KJ_S0ZEJ1MPB79243 /dev/disk/by-id/ata-SAMSUNG_HD322HJ_S17AJ9AS701687 /dev/disk/by-id/ata-SAMSUNG_HD403LJ_S0NFJQSS200987<br /> [/code]
    </li>
    <li>
      I left checksumming on to improve reliability of writes. This is up to you to decide.
    </li>
  </ul>
</div>

<div id="configurezfsonlinux">
  <h4>
    Configuring ZFS on Linux
  </h4>
  
  <p>
    First I made some changes to my pool to increase performance
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> zfs set compression=on tank<br /> zfs set primarycache=all tank<br /> zfs set atime=off tank<br /> [/code]
  </p>
  
  <p>
    Now that we have finished we can check the status of our pool by doing the following<br /> [code language=&#8221;bash&#8221;]<br /> root@proxmox:~# zpool status<br /> pool: tank<br /> state: ONLINE<br /> scan: none requested<br /> config:<br /> NAME STATE READ WRITE CKSUM<br /> tank ONLINE 0 0 0<br /> raidz1-0 ONLINE 0 0 0<br /> ata-Hitachi_HDP725032GLA380_GEK033RG2Z160C ONLINE 0 0 0<br /> ata-SAMSUNG_HD321KJ_S0ZEJ1MPB79243 ONLINE 0 0 0<br /> ata-SAMSUNG_HD322HJ_S17AJ9AS701687 ONLINE 0 0 0<br /> ata-SAMSUNG_HD403LJ_S0NFJQSS200987 ONLINE 0 0 0
  </p>
  
  <p>
    errors: No known data errors<br /> [/code]
  </p>
  
  <p>
    It&#8217;s also a good idea to add a write and/or read cache for ZFS. The write cache is called the ZFS Intent Log(<a href="http://nex7.blogspot.com.au/2013/04/zfs-intent-log.html">ZIL</a>). The read cache is called the <a>L2ARC</a>. I confirmed that it is a good idea to separate caches onto different devices for the purpose of data integrity and performance. I only have 1 SSD available here of 120GB , and as most of my use-case is read I&#8217;ll dedicate it all to L2ARC and add ZIL later when my budget allows it.
  </p>
  
  <p>
    Create a partition on the device using fdisk/parted or your preferred partitioning utility.
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]
  </p>
  
  <p>
    fdisk /dev/sdb<br /> command (m for help): print
  </p>
  
  <p>
    Disk /dev/sdb: 120.0 GB, 120034123776 bytes<br /> 255 heads, 63 sectors/track, 14593 cylinders, total 234441648 sectors<br /> Units = sectors of 1 * 512 = 512 bytes<br /> Sector size (logical/physical): 512 bytes / 512 bytes<br /> I/O size (minimum/optimal): 512 bytes / 512 bytes<br /> Disk identifier: 0x00073527
  </p>
  
  <p>
    Device Boot Start End Blocks Id System<br /> Command (m for help): n<br /> Partition type:<br /> p primary (0 primary, 0 extended, 4 free)<br /> e extended<br /> Select (default p): p<br /> Partition number (1-4, default 1): 1<br /> First sector (2048-234441647, default 2048): 2048<br /> Last sector, +sectors or +size{K,M,G} (2048-234441647, default 234441647):<br /> Using default value 234441647
  </p>
  
  <p>
    Command (m for help): w<br /> The partition table has been altered!
  </p>
  
  <p>
    Calling ioctl() to re-read partition table.<br /> Syncing disks.<br /> [/code]
  </p>
  
  <p>
    Then to add the cache simply add it to the previously created pool such as the following.
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> zpool add tank cache /dev/disk/by-id/ata-OCZ-AGILITY3_OCZ-CP2JK78KJ8T96IIN-part1<br /> [/code]
  </p>
  
  <p>
    To check that the L2ARC cache was successfully added issue the zpool status command again and you should see something similar to the following
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> root@proxmox:~# zpool status<br /> pool: tank<br /> state: ONLINE<br /> scan: none requested<br /> config:<br /> NAME STATE READ WRITE CKSUM<br /> tank ONLINE 0 0 0<br /> raidz1-0 ONLINE 0 0 0<br /> ata-Hitachi_HDP725032GLA380_GEK033RG2Z160C ONLINE 0 0 0<br /> ata-SAMSUNG_HD321KJ_S0ZEJ1MPB79243 ONLINE 0 0 0<br /> ata-SAMSUNG_HD322HJ_S17AJ9AS701687 ONLINE 0 0 0<br /> ata-SAMSUNG_HD403LJ_S0NFJQSS200987 ONLINE 0 0 0<br /> cache<br /> ata-OCZ-AGILITY3_OCZ-CP2JK78KJ8T96IIN-part1 ONLINE 0 0 0<br /> errors: No known data errors<br /> [/code]
  </p>
  
  <h5>
    Notes
  </h5>
  
  <ul>
    <li>
      When I/you do adopt a ZFS intent Log I suggest referring to some excellent advice on <a href="http://nex7.blogspot.com.au/2013/04/zfs-intent-log.html">http://nex7.blogspot.com.au/2013/04/zfs-intent-log.html</a>
    </li>
  </ul>
</div>

<div id="proxmoxconfig">
  <h4>
    Further configuration for Proxmox
  </h4>
  
  <p>
    So now we have a nice zpool ready to create some file-systems to dump all of our images, ISOs and containers onto. Before we start we are going to create further volumes in our pool for these different types of storage requirements.
  </p>
  
  <p>
    We will create the following file-systems (ISO, IMAGES, CONTAINERS, BACKUPS, STORAGE). Simply do the following to create them
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> zfs create tank/STORAGE<br /> zfs create tank/ISO<br /> zfs create tank/IMAGES<br /> zfs create tank/CONTAINERS<br /> zfs create tank/BACKUPS<br /> [/code]
  </p>
  
  <p>
    When you have finished creating you will be able to see these by doing zfs list.
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> root@proxmox:~# zfs list<br /> NAME USED AVAIL REFER MOUNTPOINT<br /> tank 384K 878G 47.9K /tank<br /> tank/BACKUPS 41.9K 878G 41.9K /tank/BACKUPS<br /> tank/CONTAINERS 41.9K 878G 41.9K /tank/CONTAINERS<br /> tank/IMAGES 41.9K 878G 41.9K /tank/IMAGES<br /> tank/ISO 41.9K 878G 41.9K /tank/ISO<br /> [/code]
  </p>
  
  <p>
    Now that we have the filesystems created we can add them to the storage pools in Proxmox. A quick check of mount shows that the wondeful little ZFS daemon already mounts the zfs filesystems for us.
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> root@proxmox:~# mount<br /> tank on /tank type zfs (rw,noatime,xattr,noacl)<br /> tank/ISO on /tank/ISO type zfs (rw,noatime,xattr,noacl)<br /> tank/IMAGES on /tank/IMAGES type zfs (rw,noatime,xattr,noacl)<br /> tank/CONTAINERS on /tank/CONTAINERS type zfs (rw,noatime,xattr,noacl)<br /> tank/BACKUPS on /tank/BACKUPS type zfs (rw,noatime,xattr,noacl)<br /> [/code]
  </p>
  
  <p>
    Now it&#8217;s time to add these mounts to Proxmox. Simply add them using the Proxmox UI. It will look similar to the following.
  </p>
  
  <p>
    <img class="aligncenter" src="https://www.nerdoncoffee.com/wp-content/uploads/2014/01/Share.png" alt="Proxmox share ui" width="609" height="207" /></img>
  </p>
</div>

<div id="zfssharing">
  <h4>
    Sharing your ZFS datasets
  </h4>
  
  <p>
    To share these datasets on the network we will do it at the ZFS level and not using the Proxmox UI. ZFS simplifies setting up shares and exporting them which I am a big fan of.
  </p>
  
  <p>
    1) Install NFS daemon and Samba<br /> [code language=&#8221;bash&#8221;]<br /> apt-get install nfs-kernel-server samba samba-common-bin<br /> [/code]
  </p>
  
  <p>
    2) Set up the NFS exports via ZFS
  </p>
  
  <p>
    [code language=&#8221;bash&#8221;]<br /> zfs set sharenfs=on /tank/BACKUPS<br /> zfs set sharenfs=on /tank/IMAGES &#8230;<br /> [/code]
  </p>
  
  <p>
    3) Update the NFS daemon to ignore an empty export file<br /> By default the nfs-kernel-server init script checks for exported filesystems and will not start the daemon if it finds none. Please use one of the methods in the <a href="http://ubuntuforums.org/showthread.php?t=2195620&p=12892616#post12892616">linked post</a>. I opted with my own solution of modifying the init script.
  </p>
  
  <p>
    4) Restart the nfs daemon<br /> [code language=&#8221;bash&#8221;]<br /> /etc/init.d/nfs-kernel-server start<br /> [/code]
  </p>
  
  <p>
    5) Check on a client machine<br /> [code language=&#8221;bash&#8221;]<br /> sudo showmount -e 192.168.1.2<br /> [/code]<br /> Note &#8211; You should specify NFS version 3 when you try to mount the NFS share otherwise it may take a LONG time to mount the shares.
  </p>
  
  <p>
    e.g for manually mounting at the shell<br /> [code language=&#8221;bash&#8221;]<br /> sudo mount -v -t nfs 192.168.1.2:/tank/STORAGE /media/ZFS/ -onfsvers=3<br /> [/code]
  </p>
  
  <p>
    e.g for mounting at boot via an /etc/fstab entry<br /> [code language=&#8221;bash&#8221;]<br /> 192.168.1.2:/tank/STORAGE /media/ZFS nfs rsize=8191,wsize=8192,noatime,auto,rw,exec,nfsvers=3 0 0<br /> [/code]
  </p>
  
  <p>
    6) Setup SAMBA/SMB shares<br /> [code language=&#8221;bash&#8221;]<br /> zfs set sharesmb=on /tank/STORAGE<br /> zfs set sharesmb=on /tank/IMAGES &#8230;<br /> [/code]
  </p>
  
  <p>
    7) restart samba daemon<br /> [code language=&#8221;bash&#8221;]<br /> root@proxmox:~# service samba restart<br /> Stopping Samba daemons: nmbd smbd.<br /> Starting Samba daemons: nmbd smbd.<br /> [/code]
  </p>
  
  <p>
    8) Check on a client<br /> [code language=&#8221;bash&#8221;]<br /> smbclient -L 192.168.1.2<br /> Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.6.6]
  </p>
  
  <p>
    Sharename Type Comment<br /> &#8212;&#8212;&#8212; &#8212;- &#8212;&#8212;-<br /> print$ Disk Printer Drivers<br /> IPC$ IPC IPC Service (proxmox server)<br /> tank_CONTAINERS Disk Comment: /tank/CONTAINERS<br /> tank_STORAGE Disk Comment: /tank/STORAGE<br /> tank_ISO Disk Comment: /tank/ISO<br /> tank_BACKUPS Disk Comment: /tank/BACKUPS<br /> tank_IMAGES Disk Comment: /tank/IMAGES<br /> tank Disk Comment: /tank<br /> Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.6.6]
  </p>
  
  <p>
    Server Comment<br /> &#8212;&#8212;&#8212; &#8212;&#8212;-<br /> PROXMOX proxmox server
  </p>
  
  <p>
    Workgroup Master<br /> &#8212;&#8212;&#8212; &#8212;&#8212;-<br /> WORKGROUP PROXMOX<br /> [/code]
  </p>
  
  <h5>
    Notes
  </h5>
  
  <ul>
    <li>
      Datasets in a pool all share the same resources in the pool. They will all have access to the same storage capacity and inherit the rules of the parent unless you manually constrain them with quotas and provide explicit settings for a dataset.
    </li>
  </ul>
</div>

<div id="summary">
  <h4>
    Summary
  </h4>
  
  <p>
    I&#8217;m very happy with Proxmox , ZFS and this whole inexpensive setup in general. I can comfortably run numerous virtual machines and serve files directly should I need to. I&#8217;m serving files at a constant speed of greater than 100MB a second with a single Gigabit connection. I&#8217;d like to see what I could achieve with a higher budget and aggregated 10Gb links.
  </p>
  
  <p>
    I strongly suggest that you don&#8217;t skimp on ram if you are setting up something like this as ZFS loves a lot of ram and then you have to account for the memory of guest VMs.
  </p>
  
  <p>
    Soon I will be bench-marking this setup directly on the server to find out what sort of performance is achieved with lowly SATA drives and the L2ARC and 32GB of ram.
  </p>
  
  <h5>
    Notes
  </h5>
  
  <ul>
    <li>
      Don&#8217;t forget to move SWAP to the appropriate drive. By default I had swap mounted on the slow flash disk. You could create another data-set such as tank/SWAP[code language=&#8221;bash&#8221;]<br /> zfs create -V 32G tank/SWAP<br /> mkswap /dev/zvol/tank/SWAP<br /> swapon /dev/zvol/tank/SWAP<br /> free</p> <p>
        total used free shared buffers cached<br /> Mem: 32634688 16905980 15728708 0 60260 1985236<br /> -/+ buffers/cache: 14860484 17774204<br /> Swap: 33554424 0 33554424<br /> [/code]</li> 
        
        <li>
          Don&#8217;t forget to also mount temp in the appropriate place. By default I had swap mounted on the slow flash disk. I suggest you use something like tempfs (see below for my example.[code language=&#8221;bash&#8221;]<br /> cat /etc/fstab</p> <p>
            # /dev/pve/root / ext3 errors=remount-ro 0 1<br /> /dev/pve/data /var/lib/vz ext3 defaults 0 1<br /> UUID=b5bd555f-bac8-473f-9357-74a79d015bba /boot ext3 defaults 0 1<br /> /tank/SWAP none swap sw 0 0<br /> proc /proc proc defaults 0 0<br /> tmpfs /tmp tmpfs nodev,nosuid 0 0<br /> [/code]</li> </ul> </div> 
            
            <div id="references">
              <h4 id="references">
                References
              </h4>
              
              <ul>
                <li>
                  <a href="http://nex7.blogspot.com.au/2013/03/readme1st.html">http://nex7.blogspot.com.au/2013/03/readme1st.html</a>
                </li>
                <li>
                  <a href="http://docs.oracle.com/cd/E19253-01/819-5461/zfsover-2/">The Oracle docs</a>
                </li>
                <li>
                  <a href="http://pve.proxmox.com/wiki/ZFS">http://pve.proxmox.com/wiki/ZFS</a>
                </li>
                <li>
                  <a href="http://ispire.me/native-zfs-for-linux-on-proxmox/">http://ispire.me/native-zfs-for-linux-on-proxmox/</a>
                </li>
                <li>
                  <a href="https://flux.org.uk/tech/2007/03/zfs_tutorial_1.html">https://flux.org.uk/tech/2007/03/zfs_tutorial_1.html</a>
                </li>
                <li>
                  <a href="https://flux.org.uk/tech/2007/03/zfs_tutorial_2.html">https://flux.org.uk/tech/2007/03/zfs_tutorial_2.html</a>
                </li>
                <li>
                  <a href="http://blog.ociru.net/2013/04/05/zfs-ssd-usage">http://blog.ociru.net/2013/04/05/zfs-ssd-usage</a>
                </li>
                <li>
                  <a href="http://serverfault.com/questions/238675/zfs-how-to-partition-ssd-for-zil-or-l2arc-use">http://serverfault.com/questions/238675/zfs-how-to-partition-ssd-for-zil-or-l2arc-use</a>
                </li>
                <li>
                  <a href="https://www.nickebo.net/zfs-zil-l2arc-what-that-about/">https://www.nickebo.net/zfs-zil-l2arc-what-that-about/</a>
                </li>
                <li>
                  <a href="http://nex7.blogspot.com.au/2013/04/zfs-intent-log.html">http://nex7.blogspot.com.au/2013/04/zfs-intent-log.html</a>
                </li>
              </ul>
            </div>

 [1]: http://www.linux-kvm.org/page/Main_Page
 [2]: http://openvz.org/Main_Page