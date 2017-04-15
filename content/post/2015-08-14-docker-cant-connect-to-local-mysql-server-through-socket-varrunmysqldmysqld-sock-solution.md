---
title: '“docker” Can’t connect to local MySQL server through socket ‘/var/run/mysqld/mysqld.sock’  SOLUTION'
author: Brad McCormack
type: post
date: 2015-08-14T04:35:10+00:00
url: /operating-systems/docker-cant-connect-to-local-mysql-server-through-socket-varrunmysqldmysqld-sock-solution/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 4031106385
categories:
  - Database
  - Docker
  - Hosting
  - Information Technology
  - Linux
  - Networking
  - Operating Systems
  - Virtualization

---
TLDR &#8211; Make sure whatever is building your docker image has enough memory.

Read on for the amusing story !

Today I was busy building a docker image for work. To start with I was doing it all locally on a pretty powerful laptop. However, I was pipe constrained and each failure would result in re-downloading files, cloning Github repo&#8217;s and so on. 

I have done my best to keep it granular so each expensive download operation was contained within a unique file-system layer (aufs). This would enable successive builds to use cache and fly along. However, some times this just wasn&#8217;t possible (with my current knowledge of docker). 

I figured why not start up a VPS with a big fat pipe so I could speed through this trial and error approach. No problem, I fired up a cheap VPS on digital ocean. First I tried CoreOS and had many failures. I thought perhaps I didn&#8217;t have enough knowledge with CoreOS so I just spawned an Ubuntu 14.04 instance and proceeded to start up docker and attempt builds again.

Part way through the build I kept getting multiple failures again. It was always based around trying to start the MySQL daemon. I wanted to deploy various DB&#8217;s to it at BUILD TIME so it was happening inside a RUN operation. I proceed to examine various logs, pipe out anything I could to try and ascertain the problem. Its more challenging to diagnose issues at BUILD TIME as it starts a new container for each RUN operation and state changes. You can&#8217;t just SSH into an an environment and peek around.

I tried a plethora of tweaks, waiting for the mysql socket to open etc. However, MySQL would always die. I couldn&#8217;t figure it out at first. Same version of docker as my local host. Same tools, same files. Yet, its failing . GRRRR. So I thought about it at a much higher level. What&#8217;s different about my local build environment and this VPS. My laptop has 16GB of ram. The VPS had 512MB &#8211; various amounts used by the OS itself which left less available for intermediary containers, all of the processes running at that point in the container which in turn resulted in catastrophic failure which in turn resulted in subsequent build steps failing.

I thought this was humorous and some other poor sap out there might find it amusing too 🙂