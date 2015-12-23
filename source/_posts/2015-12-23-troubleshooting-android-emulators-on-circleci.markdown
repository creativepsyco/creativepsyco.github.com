---
layout: post
title: "Troubleshooting Android emulators on CircleCI"
date: 2015-12-23 12:12:01 +0800
comments: true
categories: ["CircleCI", "Android", "Testing", "Espresso"]
---
[CircleCI][1] is a very nifty cloud-hosted continuous integration tool. It allows you to write your code with the peace of mind that you have. You can setup testing for every push, every pull request, every git tag and so on. You can upload to [HockeyApp][2] or [Fabric][3]. Everything gets tested, leaving you to write the best code possible.

For device testing on CircleCI you will need to spin up an emulator, however, you can't really spin up `x86` images on the CircleCI containers because they cannot provide [KVM](https://discuss.circleci.com/t/provide-kvm-in-container/1179). As such you are left with ARM emulators which are notoriously extremely slow and end up causing flakiness in testing. Here are some tools which I regularly use to troubleshoot Android Emulators on CircleCI.

<!--more-->

### Getting screenshots via ADB

You definitely should get screenshots just after the emulator is started, to check if it is not unlocked.

Here is a part of my `.circleci.yml` file:

``` bash
# Other depedencies etc.
- emulator -avd circleci-android22 -no-audio -no-window:
    background: true
    parallel: true
- circle-android wait-for-boot
- sleep 30
# Unlock the screen
- adb shell input keyevent 82
# Run espresso tests
- ./gradlew :app:connectedDebugAndroidTest -PdisablePreDex;
```

And my espresso tests were failing because of:
```
org.example.SignupTests > testCheckSignupPageDisplayed[circleci-android22(AVD) - 5.1] FAILED
	java.lang.RuntimeException: Waited for the root of the view hierarchy to have window focus and not be requesting
  layout for over 10 seconds. If you specified a non default root matcher,
  it may be picking a root that never takes focus. Otherwise, something is seriously wrong. Selected Root:
```

Clearly something was wrong.

So I decided to start taking some screenshots before my emulator ran tests
```
adb shell screencap -p | sed 's/\r$//' > screen.png
```

Upload to [imgur](http://askubuntu.com/a/544450) for immediate viewing.
``` bash
# Unlock emulator
- adb shell input keyevent 82
# First download the bash script
- wget http://imgur.com/tools/imgurbash.sh
- chmod a+x imgurbash.sh
- adb shell screencap -p | sed 's/\r$//' > bootscreen.png
- ./imgurbash.sh bootscreen.png
# Continue running some more tests
```

Here's what I obtained when this ran:

![warning]

**So more often than not the cause of failure was the system process ANR blocking the espresso tests from getting anywhere.**

Essentially whenever the Espresso test fail you must check what the UI looked like at that point of time, the view hierarchy dumped by Espresso does not give a clear indication of what happens on the emulator.

Hope this helps someone who struggled with running espresso tests on CircleCI.

[1]: https://circleci.com
[2]: http://hockeyapp.net
[3]: https://fabric.io
[warning]: http://i.imgur.com/TzPDvyj.png
