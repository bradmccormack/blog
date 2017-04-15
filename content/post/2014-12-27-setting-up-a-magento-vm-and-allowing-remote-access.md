---
title: Setting up a Magento VM and allowing remote access (Using KVM) for dev purposes (Draft)
author: Brad McCormack
type: post
date: 2014-12-27T11:29:57+00:00
url: /uncategorized/setting-up-a-magento-vm-and-allowing-remote-access/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3814379987
categories:
  - Uncategorized

---
* This is an old post that I didn&#8217;t finish. I&#8217;m putting it up out of prosperity 🙂

Rather than use a VPS provider I created a VM locally with the raft of spare computing power and low latency network facilities. This bypassed having to SCP/ Rsync / FTP send resources over the slower internet connection only to run on a constrained magento box.

Tools used
  
* Arch Linux
  
* Virt Manager + KVM

Grab the VMDK from here
  
https://bitnami.com/stack/magento/virtual-machine

Currently
  
https://bitnami.com/redirect/to/46487/bitnami-magento-1.9.1.0-0-ubuntu-14.04.zip

Extract it
  
&#8212; steps here

Convert to a qcow2 image 

qemu-img convert -f vmdk -O qcow2 bitnami-magento-1.9.1.0-0-ubuntu-14.04.vmdk magento.qcow2

Create a vm and set that is the hard disk image

Change all interfaces to virtio
  
14.04 has paravirtual drivers already installed so is good to go. Why ? Because it will speed up slow as Magento

Create a bridge
  
https://wiki.archlinux.org/index.php/Bridge\_with\_netctl Follow this
  
Add your interfaces as appropriate e.g

Description=&#8221;Example Bridge connection&#8221;
  
Interface=br0
  
Connection=bridge
  
BindsToInterfaces=(eth0 eth1 tap0 enp2s0)
  
IP=dhcp
  
\## Ignore (R)STP and immediately activate the bridge
  
#SkipForwardingDelay=yes

Start it and enable it at boot

sudo netctl start bridge

sudo netctl enable bridge
  
output (creates sym link)
  
ln -s &#8216;/etc/systemd/system/netctl@bridge.service&#8217; &#8216;/etc/systemd/system/multi-user.target.wants/netctl@bridge.service&#8217;

Change the Nic to use the new bridge (br0) as the Network source

* Start the VM

* Configure whatever you like on your vm https://wiki.bitnami.com/Virtual\_Appliances\_Quick\_Start\_Guide
  
for me starting ssh daemon
  
https://wiki.bitnami.com/Virtual\_Appliances\_Quick\_Start\_Guide#How\_to\_enable_sshd.3f

* Allow remote access
  
you could use port forwarding, port triggering, dmz , vpn etc&#8230;

As I don&#8217;t really care about this little vm (its just for dev purposes) I threw it in the DMZ zone (make sure it has a static IP &#8211; I just did a dhcp reservation and set the mac of the vnic)

* Adding a DNS A record so you don&#8217;t have to remember the ip
  
log in to your dns registrar (look up terminology) &#8211; I use cloudflare so I just added it in there for this blog as magento.nerdoncoffee.com)

* Modify your vm
  
in /opt/bitnami/apps/magento/conf/htaccess.conf php\_value memory\_limit 512M

* Update Apache2 on your vm

#<VirtualHost *:80>
  
\# ServerName magento.example.com
  
\# ServerAlias www.magento.example.com
  
\# DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
  
#
  
\# Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
#</VirtualHost>

<VirtualHost *:80>
    
ServerName magento.nerdoncoffee.com
    
ServerAlias magento.nerdoncoffee.com www.magento.nerdoncoffee.com
    
DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
    
Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
</VirtualHost>

#<VirtualHost *:443>
  
\# ServerName magento.example.com
  
\# ServerAlias www.magento.example.com
  
\# DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
  
\# SSLEngine on
  
\# SSLCertificateFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.crt&#8221;
  
\# SSLCertificateKeyFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.key&#8221;
  
#
  
\# Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
#</VirtualHost>

<VirtualHost *:443>
      
ServerName magento.nerdoncoffee.com
      
ServerAlias magento.nerdoncoffee.com www.magento.nerdoncoffee.com
      
DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
      
SSLEngine on
      
SSLCertificateFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.crt&#8221;
      
SSLCertificateKeyFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.key&#8221;

Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
</VirtualHost>

OR

<VirtualHost *:80>
    
ServerName magento.nerdoncoffee.com
    
ServerAlias magento.nerdoncoffee.com
    
DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
    
Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
</VirtualHost>

#<VirtualHost *:443>
  
\# ServerName magento.example.com
  
\# ServerAlias www.magento.example.com
  
\# DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
  
\# SSLEngine on
  
\# SSLCertificateFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.crt&#8221;
  
\# SSLCertificateKeyFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.key&#8221;
  
#
  
\# Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
#</VirtualHost>

<VirtualHost *:443>
      
ServerName magento.nerdoncoffee.com
      
ServerAlias magento.nerdoncoffee.com
      
DocumentRoot &#8220;/opt/bitnami/apps/magento/htdocs&#8221;
      
SSLEngine on
      
SSLCertificateFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.crt&#8221;
      
SSLCertificateKeyFile &#8220;/opt/bitnami/apps/magento/conf/certs/server.key&#8221;

Include &#8220;/opt/bitnami/apps/magento/conf/httpd-app.conf&#8221;
  
</VirtualHost>

vim /opt/bitnami/apps/magento/conf/htaccess.conf
  
at the bottom in the directories node
  
SetEnvIf Host www\.magento.nerdoncoffee\.com MAGE\_RUN\_CODE=magento.nerdoncoffee.com
  
SetEnvIf Host www\.magento.nerdoncoffee\.com MAGE\_RUN\_TYPE=website
  
SetEnvIf Host ^magento.nerdoncoffee\.com MAGE\_RUN\_CODE=magento.nerdoncoffee.com
  
SetEnvIf Host ^magento.nerdoncoffee\.com MAGE\_RUN\_TYPE=website

Now uncomment the following file in /opt/bitnami/apps/magento/conf/htaccess.conf 

\## you can put here your magento root folder
  
\## path relative to web root

RewriteBase /magento/

Restart apache in your vm
  
sudo /opt/bitnami/ctlscript.sh restart apache

References
  
* https://wiki.archlinux.org/index.php/Bridge\_with\_netctl