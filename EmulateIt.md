### What is the emulator? ###

You don't need to do application development and testing directly on your Freerunner.  To make development easier Android provides a full Software Development Kit (SDK) which allows you to emulate different platforms on your desktop system.

Arie Skliarouk has provided the files and instructions below for emulating your Freerunner running in 640x480 mode using the Android SDK.

### Installing ###

For downloading of the SDK and full information on installation go the [The Android Developers SDK homepage.](http://developer.android.com/sdk/index.html)

**Important** The Android SDK is under heavy development with new versions released for each Android platform. Make sure you select the one that cooresponds to Adroid platform installed on your Freerunner. The information below is based on android-sdk-linux\_x86-1.5\_r2.

### Configuring for the Freerunner ###

**Note** The location of the Android SDK directory and files  may be different in your installation.

1) Create the Android Virtual Device (AVD) with custom profile:

```
$ ~/tests/android_qa/android-sdk-linux_x86-1.5_r2/tools
$ android create avd -n FreeRunner -t 2
Android 1.5 is a basic Android platform.
Do you wish to create a custom hardware profile [no]yes

Device ram size: The amount of physical RAM on the device, in megabytes.
hw.ramSize [96]:128

Touch-screen support: Whether there is a touch screen or not on the device.
hw.touchScreen [yes]:

Track-ball support: Whether there is a trackball on the device.
hw.trackBall [yes]:no

Keyboard support: Whether the device has a QWERTY keyboard.
hw.keyboard [yes]:no

DPad support: Whether the device has DPad keys
hw.dPad [yes]:no

GSM modem support: Whether there is a GSM modem in the device.
hw.gsmModem [yes]:

Camera support: Whether the device has a camera.
hw.camera [no]:

Maximum horizontal camera pixels
hw.camera.maxHorizontalPixels [640]:

Maximum vertical camera pixels
hw.camera.maxVerticalPixels [480]:

GPS support: Whether there is a GPS in the device.
hw.gps [yes]:

Battery support: Whether the device can run on a battery.
hw.battery [yes]:

Accelerometer: Whether there is an accelerometer in the device.
hw.accelerometer [yes]:

Audio recording support: Whether the device can record audio
hw.audioInput [yes]:

Audio playback support: Whether the device can play audio
hw.audioOutput [yes]:

SD Card support: Whether the device supports insertion/removal of virtual SD Cards.
hw.sdCard [yes]:

Cache partition support: Whether we use a /cache partition on the device.
disk.cachePartition [yes]:no

Cache partition size
disk.cachePartition.size [66MB]:

Created AVD 'FreeRunner' based on Android 1.5
$
```

2) Edit the configuration file for the AVD and change HVGA to VGA (IMHO it is more proper to name the themes by the device they represent, but never mind):
```
$ vi ~/.android/avd/FreeRunner.avd/config.ini
```

3) Unpack the freerunner theme [(you can download it here)](http://android-on-freerunner.googlecode.com/files/freerunner_emulator_theme2.zip) into the skins following directory:

android-sdk-linux\_x86-1.5\_r2/platforms/android-1.5/skins

4) Start the emulator with the AVD
```
$ emulator -avd FreeRunner
```



---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_