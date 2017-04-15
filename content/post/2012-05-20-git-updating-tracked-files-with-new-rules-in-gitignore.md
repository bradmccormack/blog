---
title: Git – Updating tracked files with new rules in .gitignore
author: Brad McCormack
type: post
date: 2012-05-20T22:26:04+00:00
url: /development/git-updating-tracked-files-with-new-rules-in-gitignore/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363626816
categories:
  - Development

---
This morning I inadvertently added the build directory from Xcode to the index for git. The following cleaned that up nicely

[code language=&#8221;bash&#8221;]
  
git rm &#8211;cached for files and git rm -r &#8211;cached //for the build/ directory
  
[/code]

If you want to delete particular files that exist in many places you can do the following
  
[code language=&#8221;bash&#8221;]
  
git rm &#8211;cached *[extension]
  
[/code]
  
such as

[code language=&#8221;bash&#8221;]
  
git rm &#8211;cached *.DS_Store
  
[/code]

Then just commit the changes
  
[code language=&#8221;bash&#8221;]
  
git add -A
  
git commit -m &#8220;- Cleanup index and remove tracked files that weren&#8217;t intended to be tracked.&#8221;
  
[/code]

Finally I feel It&#8217;s best to rebase the branch so that this little mess won&#8217;t be shown (It depends upon your team&#8217;s policies of course)
  
[code language=&#8221;bash&#8221;]
  
git rebase -i origin/samebranchname
  
[/code]

or perhaps you only want to rebase a commit or two back.
  
[code language=&#8221;bash&#8221;]
  
git rebase -i HEAD^^
  
[/code]

Done 🙂

Feel free to correct me if any of this is not ideal.