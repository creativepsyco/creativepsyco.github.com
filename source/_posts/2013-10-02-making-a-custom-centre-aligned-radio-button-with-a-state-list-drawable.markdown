---
layout: post
title: "Android Programming Tip#1: Making a Custom Centre Aligned Radio Button with a State List Drawable"
date: 2013-10-02 19:10
comments: true
categories: ["Android"]
---
###Custom Widgets
These days I have been working extensively on apps for the Android Platform. And one thing keeps coming back to me: Resuse of components and widgets. Although most of the time when you are doing simple apps, you don't really pay much attention to the type of design and navigation strategies (`SINGLE_TOP` activities vs `SINGLE_TASK` blah.. blah) but when you are faced with putting different kinds of components in a single view then you probably have shuddered to think whether you were better off accomplishing this as a separate widget of its own. As can be seen from Romain Guy's [Google IO Talk][googleio], developing custom views has its own significant advantages.

Some of the advantages are as follows:

* A piece of highly reusable component will be at your disposal
* you will have fine-grained control over the lifecycle of the control/widget that you are developing, (For example, you can decide whether to load images from the network or display a dummy error image in a custom `ImageView` see `volley` [`NetworkImageView`][netio] for details)
* Custom views prevent cluttering of code in a single view thereby making abstraction more powerful
* Other general advantages due to code reuse

So let's dive into how I managed to make a custom widget for one of the apps.

<!--more-->

The Problem
-----------
Develop a custom control that behaves like a radio button but has a custom icon instead of the ordinary radio button icon and also has two drawable states to show when in highlighted mode as well as normal mode

The Solution
------------
Once you know how to create custom widgets its very easy. 

First start by creating the attibutes your control will have in `attrs.xml`
```xml
 <declare-styleable name="HighlightedImageRadioButton">
        <attr name="focusedDrawable" format="reference"/>
        <attr name="normalDrawable" format="reference" />
 </declare-styleable>
```
Here we have 2 drawables that will function as our highlighted and normal state drawables respectively

Then, create your custom radio button by extending from `RadioButton`

{% gist 6821592 %}

There's is a trick though, you need to center align the drawable since the default `RadioButton` is left aligned and has a text on the right side. To do this you need to override the `onDraw` method:

```java
	/**
     * Fix for putting the drawable in the center
     * notice that we put the background color of the drawable to transparent
     *
     * @param canvas
     */
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
 
        if (buttonDrawable != null) {
            buttonDrawable.setState(getDrawableState());
            final int verticalGravity = getGravity() & Gravity.VERTICAL_GRAVITY_MASK;
            final int height = buttonDrawable.getIntrinsicHeight();
 
            int y = 0;
 
            switch (verticalGravity) {
                case Gravity.BOTTOM:
                    y = getHeight() - height;
                    break;
                case Gravity.CENTER_VERTICAL:
                    y = (getHeight() - height) / 2;
                    break;
            }
 
            int buttonWidth = buttonDrawable.getIntrinsicWidth();
            int buttonLeft = (getWidth() - buttonWidth) / 2;
            buttonDrawable.setBounds(buttonLeft, y, buttonLeft+buttonWidth, y + height);
            buttonDrawable.draw(canvas);
        }
    }
```
The above fix was found via [this post][stack] in Stackoverflow.

And this is how you use it:

```xml
<!--Use inside a RadioGroup control-->
<com.example.ui.control.HighlightedImageRadioButton
                android:id="@+id/sports_topic_icon"
                custom:focusedDrawable="@drawable/sports_topic_icon_highlighted"
                custom:normalDrawable="@drawable/sports_topic_icon"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:layout_width="wrap_content" />
```
Just like a simple radio button. 

[googleio]: http://www.youtube.com/watch?v=NYtB6mlu7vA
[netio]: https://android.googlesource.com/platform/frameworks/volley/+/d62a616ebca5bfa4f9ec5517208e13f2d501b69a/src/com/android/volley/toolbox/NetworkImageView.java
[stack]: http://stackoverflow.com/questions/4407553/android-radiobutton-button-drawable-gravity