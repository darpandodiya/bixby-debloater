# bixby-debloater
Remove Bixby assistant from Samsung phones using adb commands, works without root. 

I used to have these steps over at my blogpost - https://darpan.blog/code/guide-remove-bixby-bloatware-from-samsung-galaxy-phones/

The blogpost has received many comments and I get frequent emails mentioning that the steps were easy to follow and worked for them to remove Bixby from their phones. I thought it would be good idea to have it here on GitHub so that anyone can suggest feedback on the steps if they become outdated. 

# Steps
Below method should work for all phones in general.

## 1. Install device drivers
[Install OEM USB drivers](https://developer.android.com/studio/run/oem-usb) this link by Google should help setting up.

Since we are talking about Samsung here, here’s direct link to their driver download page:

https://developer.samsung.com/galaxy/others/android-usb-driver-for-windows

<img src="https://user-images.githubusercontent.com/6646375/147863947-c81f3462-db13-46e2-8510-6e0a46b27947.png" height="300" width="600">


## 2. Install ADB
[[TOOL]Minimal ADB and Fastboot](https://forum.xda-developers.com/showthread.php?t=2317790) by xda-developers.com will guide you how to do it.

ADB stands for Android Debug Bridge and is used to debug applications from computer.

In essence, download the minimal adb installer and run the setup.
<img src="https://user-images.githubusercontent.com/6646375/147863964-44ec2857-f2c6-40bb-8b70-caf4f0760677.png" height="400" width="600">
<img src="https://user-images.githubusercontent.com/6646375/147863968-5d8a795e-d05b-4ffe-89b3-8dcf41dfb3c3.png" height="400" width="600">

## 3. Enable Developer Options and USB Debugging on phone
- Go to Settings
- About Phone
- Software information
- Tap on Build number 7 times
- Go back to Settings
- Developer options
- Turn on
- Scroll down at USB debugging
- Turn on
- Accept prompt of Allow USB Debugging

<img src="https://user-images.githubusercontent.com/6646375/147863974-d719e751-0cdf-4cd6-bbf1-7aafd2eb2b54.png" height="600" width="300">


## 4. Connect device to computer
Open command prompt on the computer, and execute command,

```
adb devices
```

If you’re running adb first time then you’ll see a prompt on the device asking to allow USB debugging. Set it to allow always.

Run `adb devices` again and you’ll see your device listed there.

<img src="https://user-images.githubusercontent.com/6646375/147863992-e8738067-d047-47d9-84af-1234e9da8e15.png" height="400" width="600">

<img src="https://user-images.githubusercontent.com/6646375/147863993-d00f7bac-58a5-4b7e-b9e7-20f2ee04f95a.png" height="600" width="300">


## 5. Run series of commands
Copy-paste below commands one by one on command line. Or you can combine them all into one .bat/.sh file and execute simultaneously.

```
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.agent
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.es.globalaction
adb shell pm uninstall -k --user 0 com.samsung.android.bixbyvision.framework
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.wakeup
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.plmsync
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.voiceinput
adb shell pm uninstall -k --user 0 com.samsung.systemui.bixby
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.agent.dummy
adb shell pm uninstall -k --user 0 com.samsung.android.app.settings.bixby
adb shell pm uninstall -k --user 0 com.samsung.systemui.bixby2
adb shell pm uninstall -k --user 0 com.samsung.android.bixby.service
adb shell pm uninstall -k --user 0 com.samsung.android.app.routines
adb shell pm uninstall -k --user 0 com.samsung.android.visionintelligence
adb shell pm uninstall -k --user 0 com.samsung.android.app.spage
```
<img src="https://user-images.githubusercontent.com/6646375/147864005-87544e72-90e1-464c-a594-eb27f71eba35.png" height="400" width="600">

## 6. Done!
Now if you open your launcher and try to search for Bixby, you’ll find no trace of it. Enjoy your Bixby free device! 🙂

## 7. Disable USB Debugging
Once done, please go to Developer Options and turn off USB debugging. Leaving the debugging option open is extremely dangerous.

---

## How it works

`adb shell pm uninstall -k --user 0 com.samsung.android.bixby.agent`

Let’s break them down and understand what they are doing.

`shell pm uninstall` – package manager command to uninstall given package name

`-k --user 0` – uninstall the app for current user/default user of the phone that is user 0.

`com.samsung.android.bixby.agent` – this is package name of the application

`--user 0` – implies that the application is being uninstalled for the current user – not system wide. Root access it needed to do system wide un-installation.

>This could also mean that whenever you’ll upgrade or factory reset your device, the bloatware would kick in again. The advantage being even if you uninstall a system application using this method, you can still receive official OTA updates from your carrier or OEM.  

## Bixby related packages
Below are the packages being uninstalled using the adb commands in the bat file.

```
com.samsung.android.bixby.agent
com.samsung.android.bixby.es.globalaction
com.samsung.android.bixbyvision.framework
com.samsung.android.bixby.wakeup
com.samsung.android.bixby.plmsync
com.samsung.android.bixby.voiceinput
com.samsung.systemui.bixby
com.samsung.android.bixby.agent.dummy
```

How did I list them down? Well, pretty simple, using another adb command

`adb shell pm list packages | findstr "bixby"`
