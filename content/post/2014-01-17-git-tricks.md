---
title: Git Tricks
author: Brad McCormack
type: post
date: 2014-01-17T01:13:51+00:00
url: /development/git-tricks/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363511133
categories:
  - Development

---
Here is a collection of Git tips that you may find useful (I did). This list will be updated as I find them (when I remember to as well)

##### Deleting local branches that have already been merged 

[code language=&#8221;bash&#8221;]
  
git branch &#8211;merged | grep -v &#8220;\*&#8221; | xargs -n 1 git branch -d
  
[/code]

##### Finding particular commits that have certain content 

e.g I knew at some point in time the repository contained a file (somefile) that started with returning and executing an anonymous JavaScript function that had the signature.

[code language=&#8221;javascript&#8221;]
  
(function(myvar)
  
[/code]

Now to find commits that had that code (its easier to infer which one was first) do the following.

[code language=&#8221;bash&#8221;]
  
git grep -e &#8216;(function(myvar)&#8217; $(git rev-list &#8211;all) somedirectory/somefile

someshahashhere:somedirectory/somefile: return (function(myvar) {
  
someshahashhere2:somedirectory/somefile: return (function(myvar) {
  
[/code]

Then its a matter of checking out the file from those commits and inspecting.

[code language=&#8221;bash&#8221;]
  
git checkout someshahashhere somedirectory/somefile
  
[/code]

The above could would check out the first commit with a sha hash of {someshahashhere}. You now have that file as it was originally with that anonymous JavaScript function.