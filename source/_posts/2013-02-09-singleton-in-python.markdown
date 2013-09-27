---
layout: post
title: "Singleton in Python"
date: 2013-02-09 19:27
comments: true
categories: ["Python", "Django"] 
---
In my current Final Year Project (FYP) for my undergraduate degree I needed to spawn processes and be able to store their state in the memory so that they can be started/re-started or stopped. I had previously gone about doing the same kind of operation in Java with a Singleton `ProcessManager` Class so I was pretty sure that such a pattern would exist in Python as well. Turns out, it is a [bad practice][link1] to do so. 

Anyways, I could not figure out a better way to do this, so I stuck with Singletons. Moreover it got me the job done.
<!--more-->
Here is how the final product looks like:
<script src="https://gist.github.com/creativepsyco/4744901.js"></script>

Cheers
mohit


[link1]: http://lucumr.pocoo.org/2009/7/24/singletons-and-their-problems-in-python/