---
layout: post
title: "Hide Keyboard for multiple Edit Boxes in a page"
date: 2013-11-04 11:54
comments: true
categories: ['android']
---

How to hide Keyboard in the case of multiple Edit Text
======================================================
One common problem faced by people working on Android input field forms is that many a times there is a need to show/hide the keyboard when the person touches on some other part of the screen, other than the edit box itself. However, since the focus is retained by the edit box, even if the keyboard is hidden, its hard to actually detect when and where should you be hiding the keyboard.

The Solution
============
One way to accomplish this to use the `dispatchTouchEvent` on the parent container. This is the first event that is called whenever there is a touch encountered on the surface. Only after this the rest of the touch listeners are invoked. 

We can first dispatch the touch and then check the `getCurrentFocus` view to see if the focus is retained by the `EditText`. If it is, then we simply hide the soft keyboard from the screen.

Another thing to make sure is that the parent container view groups are labelled as `focusableInTouchMode`, `clickable` and `focusable`

Here is the gist that does this job from a custom view group that contains multiple `EditText` instances.

{% gist 7229277 %}