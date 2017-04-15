---
title: Batch conversion of media containers using FFMpeg on Windows
author: Brad McCormack
type: post
date: 2015-06-02T09:58:34+00:00
url: /uncategorized/batch-conversion-of-media-containers-using-ffmpeg-on-windows/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3833352451
categories:
  - Uncategorized

---
* This is an old post that I didn’t finish. I’m putting it up out of prosperity 

I happen to have a lot of MKV files on my NAS. I&#8217;d like to stream them around my home without transcoding. Unfortunately the MKV container is not suited to streaming in my environment. I use a mixture of SMB and DLNA streaming. Some of my DLNA clients are pretty abysmal when it comes to container/codec support.

I&#8217;m lucky enough that the majority of my media has supported codecs so I&#8217;d like to just fix up the incompatabilities with the containers.

FFMPEG to the rescue!

Usage &#8211; (FIX ME)
  
ffmpeg -i cool-song.flv \ # Specify the input file
  
> -acodec copy \ # I want to copy the audio track
  
> -vcodec copy \ # I want to copy the video track too
  
> cool-song.mkv # specify the output filename and container

However I don&#8217;t want to be doing this by hand for hundreds of files so I&#8217;ll be using Window&#8217;s equivalent of Xargs which I would use on Unix.

So in this case I&#8217;ll be using forfiles (provide link)

To get help on forfiles specify
  
forfiles /?

forfiles /S /p D:Video /M r*.mkv /C &#8220;cmd /c ffmpeg -i @file -acodec copy -vcodec copy -y @fname.m4v -loglevel panic&#8221;

You could also look at keeping the ffmpeg output and redirecting it to a log (append for all copy between containers via)

(confirm the above)
  
forfiles /S /p D:Video /M r*.mkv /C &#8220;cmd /c ffmpeg -i @file -acodec copy -vcodec copy -y @fname.m4v 2>&1 | something >> log.txt

To show the current file converting , time started converting , convert it and delete the original mkv file you could use
  
forfiles /S /p D:Video /M *.mkv /C &#8220;cmd /c echo converting @file &
  
& time /t && ffmpeg -i @file -acodec copy -vcodec copy -y @fname.m4v -loglevel panic && del
  
@file&#8221;