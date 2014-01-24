---
layout: post
title: "Android Activity: Problem with Posting Runnables"
date: 2014-01-05 23:44
comments: true
categories: ["memory-leak", "android"]
---

A lot of times in Android you end up writing code like this:

```java
//---- Normal Code above ---
Runnable runnable  = new Runnable() {
    @Override
    public void run() {
        // Do something stupid
    }
}
// Singleton Handler Manager -- not an instance one, am I smart?
HandlerManager.getInstance().postDelayed(runnable, 20000); // Run this after 20 seconds! Delay processing of data
finish();
```

That is, cooking up a lot of `Runnables` and posting them on the Handler to be run after a period of time. It happens.

As our role as developers trying to get a feature done in time, sometimes these Runnnables are the only hope. However, this `Runnable` introduces a potential memory leak.

<!--more-->

Android uses a Garbage Collection mechanism to reclaim allocated memory in which nodes are traversed from the root of the Collector and each node is checked for incoming references, if these references fall into a particular type of reference, `Final`, `Weak` or other types of finalized references, these nodes are freed and memory unallocated. However, if there any incoming references, then this node cannot be freed up. If you are developing a large app, you will definitely face this issue at some point or the other.

Now you must be asking why the above code will cause a memory leak.

It does as follows.

* When you created a Runnable, it ended up being an inner class, and all inner classes have outer class [references][1].
* Now your Runnable is queued in the Singleton Handler, which is always alive in Memory
* As such your Runnable is alive
* And so is your Activity
* When you call finish(), your activity is queued up to be destroyed, but the Garbage Collector finds that your Activity instance is referenced from your Runnable inner class instance and therefore does not collect its allocated memory.

So hopefully, now you get it?
>
The solution is to use static inner classes so as to avoid initialization of the instance with the outer class instance.
>
static inner classes use the class instance, not the instance reference and therefore are safe from these issues.

[1]: http://stackoverflow.com/questions/1816458/getting-hold-of-the-outer-class-object-from-the-inner-class-object