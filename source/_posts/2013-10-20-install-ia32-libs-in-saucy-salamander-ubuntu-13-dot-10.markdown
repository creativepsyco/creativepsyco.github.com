---
layout: post
title: "Install ia32-libs in Saucy Salamander Ubuntu 13.10"
date: 2013-10-20 23:55
comments: true
categories: ['Android', 'Ubuntu']
---
With the official release of Saucy Salamander 13.10 now available, I found myself installing it cleanly on my machine.

However, when I went ahead to install the `ia32-libs` package for the Android 32-bit shared libraries supplied with the SDK. I had a shock. I could not install and the terminal returned this error message: 

```bash
output$ sudo apt-get install ia32-libs
Reading package lists... Done
Building dependency tree
Reading state information... Done
Package ia32-libs is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source
However the following packages replace it:
  lib32z1 lib32ncurses5 lib32bz2-1.0

E: Package 'ia32-libs' has no installation candidate
```

Fortunately there is a solution.

According to this [thread][ask-ubuntu] all you gotta do is use the `i386` code behind the package name

So you can install the package as follow:

```bash
sudo apt-get install ia32-libs:i386
```

####Update
According to Rick below, the above method may not work. In that case, perhaps check out this [Link][link]. This should definitely resolve the issue, although it might end up screwing up your package management. But you gotta try what you gotta try, right?

Cheers

[ask-ubuntu]: http://askubuntu.com/questions/107230/what-happened-to-the-ia32-libs-package
[link]: http://wiki.phoenixviewer.com/ia32-libs-in-ubuntu-13-10