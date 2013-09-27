---
layout: post
title: "PhoneLib bug regarding cant find proto"
date: 2013-09-27 10:30
comments: true
categories: ["android", 'proto']
---
Do you make use of [PhoneNumberUtils][phoneutils] in your android project?

If so, have you ever encountered this exception before?

```java
java.lang.RuntimeException: cannot load/parse metadata: /com/google/i18n/phonenumbers/data/PhoneNumberMetadataProto_MY at com.google.a.a.d.a(PhoneNumberUtil.java:619)
```

If yes, chances are you are probably using [proguard][progaurd] to do the obfuscation of code. However, as [this][thread] points out, proguard automatically also obfuscates the library files. However, the PhoneNumberUtils has a hardcoded path to the proto files. Therefore, one cannot rely on obfuscating the library's code. 

To resolve you must add the following lines in your `proguard` configuration file:

```
-keep class com.google.i18n.phonenumbers.** { *; }
```

This problem occurs in Android version Honeycomb and below it, as versions after Honeycomb have their own phonenumber util included which has a namespace of `com.google.android.i18n.phonenumbers`. If you are supporting version below honeycomb this can be an overlooked disaster.

[phoneutils]:  https://code.google.com/p/libphonenumber/
[thread]: https://code.google.com/p/libphonenumber/issues/detail?id=259&can=1&q=metadata
[proguard]: http://proguard.sourceforge.net/