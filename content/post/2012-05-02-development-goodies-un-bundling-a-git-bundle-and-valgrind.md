---
title: Development Goodies – Un-bundling a git bundle and Valgrind
author: Brad McCormack
type: post
date: 2012-05-02T10:37:50+00:00
url: /development/development-goodies-un-bundling-a-git-bundle-and-valgrind/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363635044
categories:
  - Development

---
### Git Bundles

So I&#8217;ve inherited a git bundle. I was trying to unbundle it incorrectly, and after a bit of enlightenment from a collegue I thought it&#8217;d be best to note it here for the future and impart some wisdom upon any reader.

The correct syntax to fetch from the bundle for instance is
  
[code language=&#8221;bash&#8221;]
  
git fetch name.bundle remotebranchname:localbranchname
  
[/code]
  
This will fetch remotebranchname into the local branch &#8220;localbranchname&#8221; from the bundle aptly named name.bundle.

### Valgrind

If you are a little rusty with lower level languages and managing memory yourself, a tool like Valgrind is invaluable. For a quick overview of how healthy your program is in terms of releasing memory use the following syntax.
  
[code language=&#8221;bash&#8221;]
  
valgrind program args
  
//If you want more detailed information use the following syntax
  
valgrind &#8211;leak&#8211;check=full program args
  
[/code]

Finally, if you&#8217;d like to see some information about other errors use the syntax
  
[code language=&#8221;bash&#8221;]
  
valgrind -v program args
  
[/code]
  
Enjoy 🙂

&nbsp;