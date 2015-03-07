---
layout: post
title: "Styling Material Toolbar in Android"
date: 2015-03-07 21:30:25 +0800
comments: true
categories: ['Android']
---
[Android Lollipop](http://www.android.com/versions/lollipop-5-0/) was released back in Google I/O reflecting the new Material Design philosophy. It was followed soon by the AppCompat Library updating to [`v21`](http://android-developers.blogspot.sg/2014/10/appcompat-v21-material-design-for-pre.html). The Library brought in a new Action Bar pattern called `Toolbar` which behaves more like a custom view with a promise to customize the look and feel of it instead of the quirks of the [ActionBar](http://cyrilmottier.com/2013/05/24/pushing-the-actionbar-to-the-next-level/). The Action Bar was both less customizable and hard to style. With Toolbar some of it goes away as the Toolbar behaves more like a view group and you can initialize it with various different views which previously you could not.

<!--more-->

However, I still found a lot of inconsistent quirks when styling the Toolbar for use in [Pie](https://www.pie.co).

###The Action Button Styles
This is how you are supposed to style the Toolbar:
```xml
<android.support.v7.widget.Toolbar
    android:layout_height=”wrap_content”
    android:layout_width=”match_parent”
    android:minHeight=”@dimen/triple_height_toolbar”
    app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
```

However, you soon begin to realize that not everything works with this style, especially the ripple effect in the navigation button is inconsistent with the rounded ripple effect, it gets cut off at the navigation button boundaries. The problem happens because when Toolbar is used as a stand alone widget instead of the normal way of using it as an Action Bar styles start breaking up. So how do we fix it? The way to go is to basically make the theme that you apply on the `Toolbar` to inherit from `Widget.AppCompat.Toolbar` theme like this:

```xml
<style name="ActionBarTheme" parent="Widget.AppCompat.Toolbar">
        <!-- Define custom attributes here -->
</style>
```

However, that still does not properly fix it. In order to rectify, we need to provide additional properties, namely: the `actionButtonStyle` and the `selectableItemBackground`. So here's the complete fix:

```xml
<style name="ActionBarTheme" parent="Widget.AppCompat.Toolbar">
        <item name="android:textColorPrimary">@color/white</item>
        <item name="colorControlNormal">?actionBarIconColor</item>
        <item name="actionButtonStyle">@style/ActionBar.ActionButton</item>
        <item name="selectableItemBackground">?android:selectableItemBackground</item>
        <item name="selectableItemBackgroundBorderless">?android:selectableItemBackground</item>
        <item name="colorControlHighlight">@color/colorSecondary</item>
</style>
```

Note that for the Button Style we are using the ActionBar button style because the Toolbar does not define one, and if you are inheriting the theme from Toolbar then you will need to do this.

Also to customize the ripple colors, you can change the `colorControlHighlight` property. Also notice that none of the properties are using the `android:` prefix because these are not defined by the framework instead they are part of the `AppCompat` library.

This way you can continue to use the Toolbar as an independent view but still inherit some styles from the ActionBar action button styles which comes in really handy.

###Providing Drop Shadow for compatibility
Lollipop introduced really cool `elevation` properties which provide crispy, papery feel to the entire app UX. However, there is no compatible method for older devices to simulate the effect. Shadows seems to be the hardest thing to achieve on the Android platform. There are various ways to simulate the effect on pre-21 devices. However the most credible way to do it is to wrap the content layout in a [`DrawShadowFrameLayout`](http://goo.gl/ANIhgr). It is used by the Google Official I/O App, the source for which is available on [Github](https://github.com/google/iosched).

However, this still requires the resulting content layout to be wrapped inside another layout, which might be a bit too much for being against flattening the UI view hierarchy. I am still searching if this can be done without wrapping the content layout in another layout, perhaps by drawing the 9-patch drawable directly after all the views have been drawn, up & over them. Currently, it causes me to write code like this:

```java
@Override
protected View _createContentView(Context context) {
    View view = super._createContentView(context);
    if (!VersionUtils.isLollipopAndAbove()) {
        DrawShadowFrameLayout frameLayout = new DrawShadowFrameLayout(context);
        frameLayout.setLayoutParams(new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT,
                                                              ViewGroup.LayoutParams.MATCH_PARENT));
        frameLayout.addView(view, ViewGroup.LayoutParams.MATCH_PARENT,
                                  ViewGroup.LayoutParams.MATCH_PARENT);
        frameLayout.setDefaultShadowDrawable(AppResources.getDrawable(R.drawable.header_shadow), true);
        frameLayout.setShadowVisible(true, false);
        return frameLayout;
    }
    return view;
}
```

But the good thing, it works perfectly fine. But you can't really animate the shadow regions alongwith the Toolbar, for example if you wanted to gently bring down the action bar when the user launches the app. The shadow is drawn on the content layout therefore does not travel with the action bar. For example something like this effect

![Gif](https://www.dropbox.com/s/gag8nh7uubgw5km/ezgif.com-gif-maker.gif?dl=0&raw=1)

There were more quirks I had to put in while making [Pie][1] I can't seem to remember them right now. But I will write future posts on the various amount of small fixes in the coming weeks. Perhaps something about setting the status bar color correctly. It seems really small but finding the right way of doing things tests your Android knowledge to the limits. And in the end when Stack Overflow turns up nothing about the issue you have, you start to look and debug as to why this really happens. And that's when the fun really begins :)

[1]: https://www.pie.co
