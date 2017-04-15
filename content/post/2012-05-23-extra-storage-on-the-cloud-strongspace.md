---
title: Extra storage on the Cloud (Strongspace)
author: Brad McCormack
type: post
date: 2012-05-23T11:40:34+00:00
url: /operating-systems/extra-storage-on-the-cloud-strongspace/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363621242
categories:
  - Information Technology
  - Operating Systems
  - Random

---
I need to upload some more photos etc to the web for an extra layer of protection for my data.
  
My VPS is running out of space so I did a little research and I found [Strongspace][1] which happens to have some very cheap storage.

It&#8217;s $3.99 USD a month for 15GB (a cup of coffee) or $39.90 for a year, so I decided to give it a try.

Whilst they provide you with a nice web interface and SFTP access I opted to sync my files using rsync.

Do do this use

[code language=&#8221;bash&#8221;]
  
rsync -avz location/ usernameatusername.strongspace.com:/strongspace/username/home
  
[/code]

Note not specifying the trailing / on location will copy only the contents inside location not the location folder which might not be what you intended.
  
It&#8217;s still early stages but it seems very cheap and easy to use.

 [1]: http://www.strongspace.com