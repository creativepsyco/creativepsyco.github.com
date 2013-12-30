---
layout: post
title: "Optimizing Google Protocol Buffers with Wire"
date: 2013-12-31 02:10
comments: true
categories: ["Android", "Protocol Buffers"]
---

The concept of [Protocol Buffers][protobuf] by Google is amazing. 

###Why should you use it

Protocol Buffers is a serialization strategy, much like XML or JSON. A lot of mobile apps and web services make use of JSON for passing information. However, protocol buffers are inherently compressed formats and not intented for the human eye (readability wise). You should definitely take a look at it if you feel that you are using a lot of the mobile data or if the primary target group of your app is the domain of mobile. It however, makes sense to use them for internal RPC related APIs and communication channels.

###Why should you not use it

Having worked with protocol buffers extensively during the development of one of my company's Android apps, I can say that Protocol Buffers definitely can have their own problems. In an agile environment where requirements can change extensively during development, it makes no sense to actually use protocol buffers without breaking the schema. And according to Google you might not wanna change the structure of Enums. I will just summarize what [Blake Smith][smith] has to say about the anti-pattern:

Don't use Protocol Buffers **IF**:
>
* You need **dynamic message** schemas
* You’re sending a lot of data directly to the browser
* You need wide-spread language support as the ability to generate protocol buffer code files is only for a limited set of languages
* You require direct human readability

###Google Protocol Buffers on Android

On Android there is an option to use the pure [Java version][protoj] or a lite version (included within the original jar) or a [Java ME version][javame] which reduces the generated code size. However, there is a big issue with the usage of Protocol Buffer on Android. The Android Dalvik VM only allows 60K public methods to be exposed during runtime due to the limitation of the size of the DEX class file that it cam store in memory, for Android Gingerbread, this is about 7MBs. This limit is strictly enforced on Gingerbread (Android 2.3.3) and if it is the minimum version of Android that you want to support then you are in pretty bad luck. For each attribute on the protocol buffer message file there are about 9 public methods generated and this increases for optional/repeated fields. As such a lot of code is generated which won't be used by large anyways in your app. On the other hand, due to the amount of public methods exposed, you run the risk of not being able to ship your app on Gingerbread.

###With Wire

I used the Java Me version and the lite version of the protocol buffers but without any significant gains in application public method size. Then came [Wire][wire] by Square (the company is obsessed with Android.. [check out their open source contributions][ops]). Wire allowed me to reduce the number of public methods exposed in the application. The generated code definition files became so sleek that for once I could actually read them (with Google protobuf implementation, there were simply and quite literally thousands and thousands of lines to go over). The rest of the code did not need much change, just a few rewiring here and there and it was done.

###A note about proguard

In addition you need the following proguard rules:
```
-keep public class * extends com.squareup.wire.**{*;}

# Keep methods with Wire annotations (e.g. @ProtoField)
-keepclassmembers class ** {
    @com.squareup.wire.ProtoField public *;
    @com.squareup.wire.ProtoEnum public *;
}
```

Cheers

[protobuf]: https://code.google.com/p/protobuf/
[smith]: http://blakesmith.me/2012/09/05/a-primer-on-protocol-buffers.html
[protoj]: https://code.google.com/p/protobuf/downloads/list
[javame]: https://code.google.com/p/protobuf-j2me/
[wire]: https://github.com/square/wire
[ops]: http://square.github.io/