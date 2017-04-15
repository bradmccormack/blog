---
title: WordPress Redirect error from Upgrade – DB Cache Reloaded Fix Plugin
author: Brad McCormack
type: post
date: 2012-06-24T06:09:39+00:00
url: /hosting/wordpress-redirect-error-from-upgrade-db-cache-reloaded-fix-plugin/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363574890
categories:
  - Hosting
  - Information Technology
  - Linux
  - Networking

---
I just found out that my wife&#8217;s website would no longer allow me to log and administer it. It was stuck in a weird redirect loop. Each time I attempted to log in it would show a bizzare url.

I tried the following process to resolve.

  * Rename the Plugins dir temporarily (It failed. Obviously a/some plugins were crucial to the site working)
  * Manually [upgrading][1] WordPress
  * Deleting one plugin at a time.

Eventually I found out that deleting the [DB Cache Reloaded Fix][2] resulted in a blank page whilst removal of the other plugins wasn&#8217;t doing anything dramatic.

I figured perhaps the new WordPress build wasn&#8217;t compatible with that version of the Plugin. I ended up using the process below to resolve it.

[code language=&#8221;bash&#8221;]
  
ssh useratdomain.com
  
wget http://downloads.wordpress.org/plugin/db-cache-reloaded-fix.zip
  
unzip db-cache-reloaded-fix.zip
  
rm db-cache-reloaded-fix.zip
  
[/code]

I was right ! Following this I could now login and administer her site (after Upgrading the MySQL database too)

A quick rsync of the website in a working condition was now in-order 🙂

Hopefully this helps somebody.

 [1]: http://codex.wordpress.org/Updating_WordPress
 [2]: http://wordpress.org/extend/plugins/db-cache-reloaded-fix/