---
layout: post
title: "Fixing Android Studio startup on Mac: A valid JVM was not found on this machine"
date: 2014-12-26 02:25:02 +0800
comments: true
categories:
---

###Android Studio on Mac OSX Yosemite
I recently migrated to Mac OSX Yosemite and discovered that when I booted Android Studio I could not start it up since a JVM was not found on the machine. I was running 1.0RC3 version of Android Studio, and did not have this problem before, on Mavericks. I knew I had a JVM installed.

<!--more-->
I did some search and stumbled upon this [thread][1] on stackoverflow. From RC3 onwards Android Studio requires a `STUDIO_JDK` [environment variable][2] to be able to run correctly. I found the resulting applescript as discussed in the answer a bit of an overkill. Here's what I did:

First find out the correct version of Java installed in your machine. As provided in the comments, you can use this command to find it out

```
echo $(find /Library/Java/JavaVirtualMachines -depth 1|tail -n1)

```
It will print something like
```
/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk
```
depending on your Java version

Open your `/etc/launchd.conf` on your mac using the terminal and add the lines from the `echo` command (`launchd.conf` might not exist on your machine):
```
setenv STUDIO_JDK /Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk

```
Now open your `~/.bashrc` file and add the following lines:

```
export STUDIO_JDK=/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk
grep -E "^setenv" /etc/launchd.conf | xargs -t -L 1 launchctl
```
This will set the correct environment variable during startup. And you can continue to launch Android Studio properly. To see immediate effect, type `source ~/.bashrc` in your terminal to refresh the environment variables.

[1]: http://stackoverflow.com/questions/27369269/android-studio-was-unable-to-find-a-valid-jvm-related-to-mac-os
[2]: http://tools.android.com/recent/androidstudio1rc3_releasecandidate3released
