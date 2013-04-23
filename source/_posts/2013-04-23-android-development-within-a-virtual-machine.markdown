---
layout: post
title: "Android Development within a Virtual Machine"
date: 2013-04-23 17:30
comments: true
categories: ["Android", "VM"]
---
Sometimes it can be [really fustrating][vmware-issue] to work within a Virtual Machine (VM) which might not have support for a number of removable devices. I am talking of VMWare workstation which is a big part of the problem.

Below I describe a working way to install a reliable working environment in VMWare Workstation 9.x running Ubuntu 12.10 

<!--more-->

For the instructions below I am installing everything under: `~/android`

###Pre-Requisite

* Install Sun (Oracle) JDK for IntelliJ Idea
```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
```

###Install Intelli-J
* Donwload Intell-J from [this][intellij-download] and extract to a directory in your computer
* Unpack the directory
* Add the Idea bin folder to your PATH
```bash
export PATH=$PATH:/usr/lib/<YOUR_IDEA_FOLDER>/bin
```

###Install Android SDK Bundle
* Download the ADT Bundle from [this][and-link] 
* Extract it and move to a location in your 
* Add these lines to your `.bashrc` file
```bash
export PATH=${PATH}:~/android/android-sdk-linux/tools
export PATH=${PATH}:~/android/android-sdk-linux/platform-tools
export PATH=${PATH}:~/android/idea-IC/bin
```
* Source your `.bashrc`
```bash
source ~/.bashrc
```

###Test a Device
Now go ahead and test your shiny new Android Development device by connecting it to the USB port on your machine.

* In VMWare you need to make sure that it connects to the Android Device. You can do this, by navigating to **VM > Removable Devices > Samsung Android Device > Connect (Disconnect fro host)**. This will disconnect it from your host machine (Windows 7 in my case) and connect it to the VMWare instance.

* In VMWare Ubuntu:
```bash
adb devices #lists all the devices attached to the computer
```
* If the above command displays nothing, do this:
```bash
adb wait-for-device
adb start-server
adb devices # display connected devices
```
This will make it wait for the device to link up

###Android Device Issues with VMWare
Some of the issues that I encountered while connecting my Samsung Galaxy S-III to VMWare on a Windows 7 Enterprise workstation are as follows:

* If you have an error message that says that the device cannot be plugged out, then make sure no process is accessing the device. This includes the Kies software from Samsung. Use the Task Manager to kill them

* If you have an error message that says there is a driver error then perhaps you have a USB 3.0 issue. As of now, VMWare does not support Intel-based USB 3.0 controllers. So you need to suck it up for the moment and find a USB 2.0 port on your machine. Otherwise you need to look up another [manual][vmware-issue]

###Conclusion
Hopefully by the end of this guide, there is a working android development setup on your machine. 

[vmware-issue]: http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1027964
[intellij-download]: http://www.jetbrains.com/idea/download/index.html
[and-link]: http://developer.android.com/sdk/index.html
