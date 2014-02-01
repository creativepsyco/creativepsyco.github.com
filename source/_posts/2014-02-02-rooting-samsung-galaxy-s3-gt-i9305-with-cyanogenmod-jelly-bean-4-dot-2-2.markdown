---
layout: post
title: "Rooting Samsung Galaxy S3 GT-I9305 with CyanogenMod Jelly Bean 4.2.2"
date: 2014-02-02 03:36
comments: true
categories: ['android', 'hacks']
---

After getting the 4.3 update my Samsung Galaxy S3 started having [weird battery problems][1], it was a high I moved to a custom ROM since they were a lot stable than a few years back and offered a much better stock Android experience without any bloated software than device OEMs put inside.

In this post, I shall roughly tell you how I installed a custom on my Samsung Phone.

>
Warning: Rooting voids the warranty of your device. We and the developers of these files are not responsible if anything happens to your device. Use this rooting guide at your own risk!

<!--more-->

## Prerequisites

* Need a Windows XP/Windows 7/Windows 8 PC
* Ample of charge on your phone battery
* The Galaxy S3 I9305 should be factory unlocked.
* Have a model number *I9305* phone, the international model of Samsung Galaxy 3 LTE. 
* Do make a backup of the phone date, as everything will be erased.

## Downloads

* [ODIN + Auto Root File][2]
* [TWRP File][3], only version 2.5.* works properly with the CyanogenMod ROM
* [CyanogenMod ROM for 4.2.2][4]
* Corresponding [Google Apps package][7]

## Steps

* Plug the S3 into your computer and make sure all drivers are installed correctly. If you run into problems, try a different USB cable or port. Then turn your device off.
* Put the S3 into Download Mode by holding in the Power, Home, and Volume Down buttons at the same time. Then push the Volume Up button once this screen pops up.
* Unzip and open the CF-Auto-Root folder and right click on Odin.exe, then click on “Run as Administrator”.
* Make sure there is a yellow box in the screen, if not then the USB drivers did not install correctly and you should try a different USB cable/port.
* Once you have the yellow box, click on PDA and then browse for the file that was downloaded with Odin, (CF-Auto-Root-i9305.tar.md5). Then click Start.
* Once you get a "green" box, means you have succeeded.

Next we need to install TWRP:

* Put you device in Download mode, make sure its connected via USB
* Go back to Odin, and click PDA
* This time, get the openrecoveryXXX-twrp-2.5.x.tar file and flash it
* If you get a "green" box at the end, means you passed

Now we need to transfer the Custom ROM + Google Apps package zip files to the Phone either via `adb` or via manual file transfer through Windows Explorer

Once that's done we can launch the recovery mode to commence installing the custom ROM.

* Turn off your phone
* Hold in the Power, Volume Up, and Home buttons until “Recovery Booting” appears in the top left corner. Then release the power button but keep holding the Volume Up and the Home button until “TeamWin” appears on the screen.
* Click on Wipe – Advanced Wipe and select “Dalvik Cache”, “System”, and “Cache”. Then move the slider to complete the wipe.
* Go back and click on Install and scroll down to the location where you transfered the ROM zip file. Click on the zip file and then slide to install the ROM.
* After that do the same step to install Google Apps
* After move to Home and do a factory reset to delete useless App Data

And you are done, Congratulations!

## Known Issues
* If your [Google Apps don't work][6], like the Keyboard crashes or something, make sure that you have the correct Google Apps package
* If you get assert failed error, you will need to try with different nightlies until you hit the one that succeeds.
* For some of the common problems, visit the [Wiki][5]



[1]: http://au.ibtimes.com/articles/536399/20140129/galaxy-s4-s3-note-2-update.htm#.Uu1M9HeSxqs
[2]: http://download.chainfire.eu/232/CF-Root/CF-Auto-Root/CF-Auto-Root-m3-m3zh-gti9305.zip
[3]: http://techerrata.com/file/twrp2/i9305/openrecovery-twrp-2.5.0.0-i9305.tar
[4]: http://download.cyanogenmod.org/?device=i9305&type=stable
[5]: http://wiki.cyanogenmod.org/w/Wiki_Problems
[6]: http://forum.cyanogenmod.com/topic/78601-unfortunately-android-keyboard-aosp-has-stopped-problem/
[7]: http://goo.im/gapps/gapps-jb-20130812-signed.zip
