---
layout: post
title: "Set Background that does not resize on Soft Keyboard"
date: 2014-05-12 14:29:45 +0800
comments: true
categories: ["Android"]
---

###Problem

Here is an interesting problem. Let's say you are in a particular view, and you want to be able to set a background. You simple use the `View.setBackground(Drawable)` or `View.setBackgroundDrawable(Drawable)` call to do this. However once you do this, you realize that whenever the soft keyboard is opened it pushes your view to the top/resizes it such that your background looks squished.
<!--more-->
I encountered this while working on a chatting application. The resulting image looks something like this:

![Image](/images/flat_background.jpg)

###Solution

The solution is quite simple:

* In your Activity Manifest, declare the activity which houses the view to `android:windowSoftInputMode="adjustResize|stateHidden"`
* Then follow this piece of code:
```java
	Bitmap bmImg;
	// Load a bitmap into bmImg
	BitmapDrawable background = new BitmapDrawable(bmImg);
	background.setGravity(Gravity.TOP);
	this.setBackgroundDrawable(background);
```

This will make sure that you set your background with the `Gravity.TOP` which will ensure it is always aligned with the top of the container view and is not affected by the Soft Keyboard display.