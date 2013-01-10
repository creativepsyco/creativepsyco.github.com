---
layout: post
title: "Essential Mozilla Command Line"
date: 2013-01-11 00:27
comments: true
categories: [Mozilla, Calendar]
---

*This is a reference of some of the very important mozilla commands that I use in daily Terminal Life*

###Requirements
Set up Hg, Autoconf using Brew. Look up the instructions at the official website.

###Setting up of Source Directory

Mozilla Code can be checked out from http://hg.mozilla.org. To set up a Thunderbird and Calendar clone on your local machine checkout the source code as

```
bash$ hg clone http://hg.mozilla.org/comm-central
```

This will create a `comm-central` directory in your local machine
Now `comm-central` depends on a number of submodules which can be fetched by the following commands
``` bash
bash$ cd comm-central
bash$ python client.py checkout
```
This will fetch the submodules. It will take a while

###Compiling Code
In order to build the calendar application create a `.mozconfig` file within `comm-central` and insert the following code:

```
mk_add_options MOZ_OBJDIR=@TOPSRCDIR@/obj-TB
mk_add_options AUTOCONF=/usr/local/Cellar/autoconf213/2.13/bin/autoconf213
ac_add_options --enable-calendar

# Turn off compiler optimization. This will make applications run slower,
# will allow you to debug the applications under a debugger, like GDB.
ac_add_options --disable-optimize
ac_add_options --disable-jemalloc

# -s makes builds quieter by default
# -j4 allows 4 tasks to run in parallel. Set the number to be the amount of
# cores in your machine. 4 is a good number.

mk_add_options MOZ_MAKE_FLAGS="-j4 -s --no-print-directory"
mk_add_options JS_READLINE=1
export JS_READLINE=1
ac_add_options --disable-tests
ac_add_options --disable-installer
```

Now fire away the compiler using the following command:
```
bash$ make -f client.mk
```

This will take quite a lot of time depending upon your machine's strength of course.

###Applying a patch
From time to time, you might need to apply a patch from someone else, or maybe your own patch. This is quite easy, but note that if you plan on applying the patch, making some changes and then want to create a patch that doesn't contain the patch you originally applied, it might be quite a bit harder.

{% codeblock lang:bash %}
cd $MOZILLA/calendar
# --dry-run tests the patching process to ensure that the patch will go
# ahead cleanly. Be sure to run --dry-run at least once before running
# the actual patching process.
patch -p1 -i ~/my_first_bug.diff --dry-run
# Now check if the patch applies cleanly, or you are willing to fix the 
# places it went wrong. When you are confident, you can call:
patch -p0 -i ~/my_first_bug.diff

# If the file to patch was not found, take a look at the patch headers. For
# example, if the header contains "+++ themes/winstripe/calendar-views.css",
# then you need to go into the base directory and call again. If the header
# contains "+++ mozilla/calendar/base/Makefile.in", you can use -p2 instead to
# strip the "mozilla/calendar" part.
patch -p2 -i ~/my_first_bug.diff
{% endcodeblock %}

###Creating a patch
Simply create a diff and store it somewhere
```
bash$ hg diff > ~/my_first_patch.diff
```

###Reverting a patch
```
bash$ hg revert .
```

For more information refer to [Mozilla Documentation on Build][ref1]

[ref1]: https://developer.mozilla.org/en-US/docs/Developer_Guide/Build_Instructions


