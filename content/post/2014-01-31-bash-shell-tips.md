---
title: Bash Shell tips
author: Brad McCormack
type: post
date: 2014-01-31T02:46:56+00:00
url: /operating-systems/bash-shell-tips/
dsq_thread_id:
  - 3363355847
spacious_page_layout:
  - default_layout
categories:
  - Linux
  - Operating Systems
  - Random

---
### Running a command against a selection of files 

Lets say you want to pull out some data from a few SQL Lite databases. It would be painful to execute your SQL statement against each DB one at a time. Here are two ways to do that assuming you know that each DB is called &#8220;some.db&#8221;.

#### Using exec 

[code language=&#8221;bash&#8221;]
  
find . -name &#8220;some.db&#8221; -exec sqlite3 {} &#8220;SELECT Some\_Data from Some\_Table&#8221; \;
  
[/code]

##### Notes

  * The {} represents the placeholder that will be substituted with each path/filename that is found.
  * The \; implies that you want to run the command for each path/filename found.
  * Swapping out \; with +; signifies that you instead want to concatenate all path/filenames and then use that as the argument to the command.
  * You may also use a regular expression in the find command to control the granularity of matches </ul> 
    #### Using xargs 
    
    [code language=&#8221;bash&#8221;]
  
    find . -name &#8220;some.db&#8221; | xargs -n1 -iDB sqlite3 DB &#8220;SELECT Some\_Data from Some\_Table&#8221;
  
    [/code]