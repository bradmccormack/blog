---
title: http vs www site prefix.
author: Brad McCormack
type: post
date: 2012-01-30T13:29:03+00:00
url: /hosting/web-hosting-pitfalls-1-http-vs-www-site-prefix/
dsq_thread_id:
  - 3549599192
categories:
  - Hosting
  - Information Technology

---
Web Hosting Pitfalls #1

So here I am all eager and tenacious, trying to learn how to host my own sites.

In the past I used shared hosting. Sure it was easy and the wonderful chaps over at http://wphosting.com.au were very helpful. However, I found I wanted to do some learning by myself and have some more control. Perhaps even squeeze some performance out of the sites 🙂

I happen to use an inexpensive (but flexible and relatively powerful for the price) VPS from http://www.linode.com . Perhaps one day I&#8217;ll write up a little tutorial on how I setup my Linode (Linux node) and setup the remainder of the serving stack (Nginx, Mysql , PHP). But for now I&#8217;ll actually get to the point and note a little tidbit from my learning.

I was in the progress of setting up hosted email with Google, entering all the information and entering my domain details www.somedomain.com. After this you have to verify your domain via one of the many ways listed on the Google Apps setup process. I uploaded a file and figured it would be a piece of cake.

FAIL.

I was quite perplexed. It was trying to verify against http://somedomain.com which is actually different.

The Fix:
  
Login to the DNS Manager
  
Add an A Record that points to somedomain.com and make it point to the same location as the www prefix.

Problem solved. I could now verify my site and proceed to setup MX information.

Whats an A Record?
  
A static IP address that is pointed to via a DNS Record. The A means (address)

There probably are ramifications but for now it works great!

P.S don&#8217;t forget it can take about 48 hours for MX records to propagate !