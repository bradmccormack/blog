---
title: Learning JQuery – Selectors
author: Brad McCormack
type: post
date: 2012-08-28T09:41:23+00:00
url: /development/learning-jquery-selectors/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3363525624
categories:
  - Development

---
I&#8217;ve only just begun to have exposure to JQuery/Javascript as a primary language. I&#8217;ve been progressively going through [tutorials][1] and other sites such as  [I&#8217;ve only just begun to have exposure to JQuery/Javascript as a primary language. I&#8217;ve been progressively going through [tutorials][1] and other sites such as][2] and  [w3schools.][3]

From what I&#8217;ve found, JS quite enjoyable and JQuery even more so as it reduces the code I need to write to get things done (at the expense of the initial learning).

I&#8217;ve got a long way to go but here is a minimalistic list I&#8217;ve made for myself of various Selectors and filtering on those selectors.

#### Selectors 

[code language=&#8221;javascript&#8221;]
  
$allListItems = $(&#8220;li&#8221;);
  
$alldivs = $(&#8220;div&#8221;);
  
$whodom = $(&#8220;*&#8221;);
  
$byclass = $(&#8220;.someclass&#8221;);
  
$byId = $(&#8220;#Id&#8221;);
  
[/code]

#### Descendant Selector

[code language=&#8221;javascript&#8221;]
  
$all\_li\_in_fancylist = $(&#8220;li&#8221; .fancylist&#8221;);
  
[/code]

&#8211; Descendant of class/id etc constraint
  
= All items that are a descendants of list items that have a class of fancylist

#### Child Selector

[code language=&#8221;javascript&#8221;]
  
$(&#8220;.shiny > a&#8221;)
  
[/code]

-Ancestor > child type
  
= All links that are a child of an element with a class of shiny

#### Pseudo-Classes

[code language=&#8221;javascript&#8221;]
  
$(&#8220;li:first-child&#8221;)
  
[/code]

= Will select all list-elements that are the first child of their parent.

#### Filtering Selections

[code language=&#8221;javascript&#8221;]
  
$alldivs = $(&#8220;div&#8221;);
  
$lastdiv = $alldivs.last();
  
$2nddiv = $alldivs.eq(1);
  
[/code]

 [1]: http://www.codecademy.com/tracks/jquery
 [2]: http://docs.jquery.com/Tutorials
 [3]: http://www.w3schools.com/jquery/default.asp