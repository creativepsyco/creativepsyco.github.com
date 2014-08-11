---
layout: post
title: "Facebook does it again. Cheating Dalvik"
date: 2014-08-11 10:48:33 +0800
comments: true
categories: ["Android", "Hacks", "Facebook"]
---

Recently I found this crash trace in my phone:

```
08-08 12:26:17.816    2756-2756/? E/DalvikReplaceBuffer﹕ Failed to replace LinearAlloc buffer (at size 33554432). Continuing with standard buffer.
    java.io.IOException: Could not find LinearAlloc memory mapping.
            at com.facebook.dalvik.DalvikInternals.b(DalvikInternals.java:132)
            at com.facebook.dalvik.DalvikReplaceBuffer.b(DalvikReplaceBuffer.java:88)
            at com.facebook.dalvik.DalvikReplaceBuffer.a(DalvikReplaceBuffer.java:60)
            at com.facebook.katana.app.FacebookApplication.a(FacebookApplication.java:202)
            at com.facebook.base.app.DelegatingApplication.e(DelegatingApplication.java:32)
            at com.facebook.base.app.DelegatingApplication.attachBaseContext(DelegatingApplication.java:59)
            at android.app.Application.attach(Application.java:181)
            at android.app.Instrumentation.newApplication(Instrumentation.java:991)
            at android.app.Instrumentation.newApplication(Instrumentation.java:975)
            at android.app.LoadedApk.makeApplication(LoadedApk.java:502)
            at android.app.ActivityThread.handleBindApplication(ActivityThread.java:4301)
            at android.app.ActivityThread.access$1500(ActivityThread.java:135)
            at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1256)
            at android.os.Handler.dispatchMessage(Handler.java:102)
            at android.os.Looper.loop(Looper.java:136)
            at android.app.ActivityThread.main(ActivityThread.java:5001)
            at java.lang.reflect.Method.invoke(Native Method)
            at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:785)
            at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:601)
```

I immediately realized that this is Facebook's version of trying to [cheat Dalvik VM][link1] for its limit of 65K methods in a single DEX file.

Quoting from the article:

> As it stood, the release of the much-anticipated Facebook for Android 2.0 was at risk. It seemed like we would have to choose between cutting significant features from the app or only shipping our new version to the newest Android phones (ICS and up). Neither seemed acceptable. We needed a better solution.
> Once again, we looked to the Android source code. Looking at the definition of the LinearAlloc buffer, we realized that if we could only increase that buffer from 5 MB to 8 MB, we would be safe!

I maintain that this is a horrible hack and one that is beset with many problems. On a really non-technical note, this fix is a very dirty which can cause other apps to misbehave on your phone. Also, it means you as a developer don't really believe in the existence of well-defined APIs. Recently, Facebook did the same by launching App Links for Mobile SDK, another hacky product, without a proper involvement of the community, in a standards enforced way. You can read all other really technical jot-downs [here](https://news.ycombinator.com/item?id=5321634)

While the official standard way to to fix this problem is described in the offical [Android Blog][custom].

*EDIT*

~~Seems like Facebook missed it. What a waste.~~

Facebook did not miss it, however the reasons they have mentioned don't seem to be very convincing. Android apps load up in a sequence with different entry points being called at different stages of the application, not everything is a core part of the app, it can be argued to some extent. 

[Here][link3] is a nifty way to split DEX files if you are facing a similar situation.

[link1]: https://www.facebook.com/notes/facebook-engineering/under-the-hood-dalvik-patch-for-facebook-for-android/10151345597798920
[custom]: http://android-developers.blogspot.sg/2011/07/custom-class-loading-in-dalvik.html
[link3]: https://github.com/creativepsyco/secondary-dex-gradle