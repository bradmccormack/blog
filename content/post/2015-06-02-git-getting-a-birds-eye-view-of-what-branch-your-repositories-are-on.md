---
title: Git – Getting a birds eye view of what branch your repositories are on
author: Brad McCormack
type: post
date: 2015-06-02T09:56:22+00:00
url: /uncategorized/git-getting-a-birds-eye-view-of-what-branch-your-repositories-are-on/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3833350993
categories:
  - Uncategorized

---
* This is an old post that I didn’t finish. I’m putting it up out of prosperity
  
A quick script to enable birdseye view of what branch various repositories are residing on.

cat listrepos.sh

#!/bin/bash

REPOS=&#8221; \`find $HOME/somepath* -maxdepth 1 -mindepth 1|xargs -l echo\`&#8221;
  
GIT_DIRS=${REPOS}
  
for d in $GIT_DIRS
  
do
         
if [ -d $d/.git ]; then
             
cd $d
             
REPO=${d##*/}
             
BRANCH=\`git rev-parse &#8211;abbrev-ref HEAD\`
             
echo $REPO $BRANCH
         
fi
  
done

example output

brad@ubuntu:~$ ./listrepos.sh
  
repoa development
  
repob development
  
repoc master
  
repod featurebranch