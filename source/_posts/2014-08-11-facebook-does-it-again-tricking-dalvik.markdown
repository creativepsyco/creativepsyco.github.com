---
layout: post
title: "Facebook does it again. Tricking Dalvik"
date: 2014-08-11 10:48:33 +0800
comments: false
published: false
categories: ["Android", "Hacks", "Facebook"]
---

**Small Clarification**

Since this post went live it triggered quite a discussion [HN][hn].

*Just to clarify before you start bashing me in the comments. I knew about FB doing the LinearAlloc buffer thing when they had released the patch. However, there are other solutions [out there][dex] to fix this issue, and Facebook did not try adopting any of them, their reasons for not adopting it are not that convincing, AFAIK from a software development perspective.*
<!--more-->
**Also LinearAlloc!=dex method count**

*I should have been more explicit about this, however having too many methods and deep interface hierarchies within a single DEX file does amount to [exceeding the Linear allocation buffer](https://code.google.com/p/android/issues/detail?id=22586). Note that you can still exceed the Linear allocation buffer even if you have fewer methods in your DEX file, if you can deep complicated interface hierarchy, its fairly easy to create one. Exceeding the methods in a DEX file is just one of the ways why this can happen*

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

I immediately realized that this is Facebook's version of trying to [tricking the Dalvik VM][link1] for its limit of only 5MB on the linear allocation buffer. One of the very prominent ways of exceeding this is having too many inter-related in a single DEX file. If you have more than 64K (the exact number is 65536) methods in your DEX file, you will definitely fail during the DexOpt stage (which is when the methods are loaded into the buffer).

Quoting from the article:

> As it stood, the release of the much-anticipated Facebook for Android 2.0 was at risk. It seemed like we would have to choose between cutting significant features from the app or only shipping our new version to the newest Android phones (ICS and up). Neither seemed acceptable. We needed a better solution.

> Once again, we looked to the Android source code. Looking at the definition of the LinearAlloc buffer, we realized that if we could only increase that buffer from 5 MB to 8 MB, we would be safe!

I maintain that this is not a great way to fix this issue and seems a quick hack and one that is beset with many problems. On a really non-technical note, this fix is a very dirty which can cause other apps to misbehave on your phone. Also, it means you as a developer don't really believe in the existence of well-defined APIs. Recently, Facebook did the same by launching App Links for Mobile SDK, another much needed product(on mobile) but with its own set of protocols, versioning info etc., without a proper involvement of the community, in a standards enforced way. You can read all other really technical jot-downs [here](https://news.ycombinator.com/item?id=5321634)

While the official standard way to to fix this problem is described in the offical [Android Blog][custom]. 

**Why I say that it's a problem of too many methods in a single DEX file**

Quoting from FB:
> However, there was no way we could break our app up this way--too many of our classes are accessed directly by the Android framework. Instead, we needed to inject our secondary dex files directly into the system class loader. This isn't normally possible, but we examined the Android source code and used Java reflection to directly modify some of its internal structures. We were certainly glad and grateful that Android is open source—otherwise, this change wouldn’t have been possible. 

I am assuming that when they say **too many of our classes are accessed directly by the Android Framework**, they mean that a lot of Activities, Views, Receivers etc. need to registered which can only happen at the DexOpt stage of the app. This supports my assumption that the reason why they can't break the app is because of a lot of direct views and class hierarchies that need to be injected into the 
system classloader before the start of the app.

However, that said, FB is using the secondary dex file loading mechanism to some extent on their app. You can decompile the APK and take a look. But why then they would need to do a Linear Alloc even now is the main question, which was the motivation behind this post.

**EDIT**

~~Seems like Facebook missed it. What a waste.~~

Facebook did not miss it, however the reasons they have mentioned don't seem to be very convincing. Android apps load up in a sequence with different entry points being called at different stages of the application, not everything is a core part of the app, it can be argued to some extent. 

[Here][link3] is another very hacky way to split DEX files if you are facing a similar situation.

[link1]: https://www.facebook.com/notes/facebook-engineering/under-the-hood-dalvik-patch-for-facebook-for-android/10151345597798920
[custom]: http://android-developers.blogspot.sg/2011/07/custom-class-loading-in-dalvik.html
[link3]: https://github.com/creativepsyco/secondary-dex-gradle
[hn]: https://news.ycombinator.com/item?id=8162342
[dex]: https://medium.com/@rotxed/dex-skys-the-limit-no-65k-methods-is-28e6cb40cf71