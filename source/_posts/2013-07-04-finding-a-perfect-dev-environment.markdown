---
layout: post
title: "Finding a perfect dev environment"
date: 2013-07-04 13:51
comments: true
categories: ["android", "development", "ubuntu"]
---

Setting up a unobstrusive development environment has its advantages. If you are one of those people who probably spend about 14 hours a day sitting in front of a monitor doing what-not, from testing to deployment and development, then read on. Otherwise you can spend some time reading an [IBM][1] justification of how and why a solid dev environment pays off. This post is about setting up a solid development environment on Ubuntu Linux 12.04 and contains bits and pieces of information of how a lot of things can be automated.

I assume some familiarity with Linux command-line for the below.

###Prepare your home directory
This would be where all your patches come in handy. Spice it with some names that you can remember in the long-run. Keep a separate directory for SDKs, sources and the common shared libraries that you use.

```bash
mkdir /home/creativepsyco
cd /home
chown msk creativepsyco
chgrp msk creativepsyco 
cd /home/creativepsyco
mkdir android
```

###Install ZSH and Oh-My-ZSH
[ZSH][2] is a fanstastic tool and better than your average joe-bash-shell. It has advanced modules for autocompletion and lots of plugins which, after you use, trust me, are going to be your goto tools for specific tasks.

You can install zsh by using these commands

```bash
sudo apt-get update && sudo apt-get install zsh
```

Also, install [oh-my-zsh][3] which is another cake layer on top of zsh. It includes tons of automatic zsh scripts and plugins to aid you in development.

```bash
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

Set ZSH as your default shell

```bash
chsh -s /bin/zsh
```
> A tip: To get more out of your oh-my-zsh installation you need to enable some or a lot of the plugins in your `~/.zshrc` file which replaces your `~/.bashrc` file

###Install Git
```bash
sudo apt-get install git-core
```

###Make yourself feel at home with solarized
I absolutely love the [Solarized dark theme][4]. I use it everywhere: At work, in my MacBook Pro (running from 2010 without a scratch so far [touch wood]). Here's how it looks like:

![Solarized Dark Image][5]

Isn't it absolutely delicious. Unfortunately some of the ways to install on ubuntu terminal or gnome-terminal have been rather futile. Here's one that works so far:

1. Go to your home directory
2. Blindly follow the commands below
```bash
$ git clone git://github.com/sigurdga/gnome-terminal-colors-solarized.git
$ cd gnome-terminal-colors-solarized
$ ./solarize
```
3. And Voila experiment between the two themes

###Start installing Android Libraries
Now that the eyes are set, start installing the necessary SDKs from Android. You can get the SDK from Android's site. Just need to unzip it and place it in the home folder. Eventually your folder configuration should look something like this:
```bash
msk@ubuntu:/home/creativepsyco/android$ ls
adt-bundle-linux            ----- ADT bundle
idea-IC                     ----- IDE IntelliJ stack (Or Eclipse)
shared_android_lib          ----- Shared library 
project_android             ----- Project Git directory
anotherapp_android          ----- Project Git directory
```

> A Tip: If you are using a 64-bit OS then to make your life easy, you should install these compatible libs to help you: `sudo apt-get install ia32-libs`

###SQLite
```bash
sudo apt-get install sqlite3
```

###Ending Note
Now you have everything for Android development and please follow [Google's tutorials][6].


[1]: http://www.ibm.com/developerworks/rational/library/define-scope-development-environment/
[2]: http://www.zsh.org/
[3]: https://github.com/robbyrussell/oh-my-zsh
[4]: http://ethanschoonover.com/solarized
[5]: http://d.pr/i/xmRI.jpg
[6]: http://developer.android.com/training/index.html
