---
title: Whats a good use for a WDTV Live box?
author: Brad McCormack
type: post
date: 2012-03-08T10:00:19+00:00
url: /operating-systems/whats-a-good-use-for-a-wdtv-live-box/
dsq_thread_id:
  - 4747669337
categories:
  - Information Technology
  - Operating Systems
  - Random

---
Well Rsyncing approximately 16GB of Family photos onto my VPS  of course.

Why not use your desktop you say? Probably because it uses >100Watts (PC or the Imac) and the WDTVLive uses <7watts 🙂

<pre>rsync -avPe ssh /local/volume/ <a href="https://webmail.exetel.com.au/src/compose.php?send_to=brad%40www.nerdoncoffee.com">user@remote.domain.com</a>:~/Location</pre>

Will sync all files in /local/volume to the Location directory inside /home for user at remote.domain.com In the grand scheme of things will probably only save $2 electricity but its still nice! Thanks WDLXTV