# Introduction #

To install ADB, download the latest version of the Android SDK by following the instructions at http://developer.android.com/sdk. You will find `adb` in the `tools` directory.

# Enable ADB #
Due to security issues with ADB, the shell has been disabled by default in AoF. You can enable ADB again by navigating to Settings -> Application -> Development -> USB Debugging.

# Connecting #

First connect your freerunner running android with usb to your host. The following command configure usb networking on some Linux systems.

```
   ifconfig usb0 192.168.0.200 netmask 255.255.255.0
```
Usb networking is required to use adb if these commands doesn't work find out how to do the same thing on your system.  The connection port might be different on your system i.e. eth0, eth1.

Detailed documentation on how to get this workig is available in the [USB Networking](http://wiki.openmoko.org/wiki/USB_Networking) section on the Openmoko Wiki.

When usb networking has been configured and confirmed working use the following command to connect.

```
   adb kill-server
   ADBHOST=192.168.0.202 adb devices
```

# Commands #
Here is a list of commands that works with adb. Please see http://developer.android.com/guide/developing/tools/adb.html for further details.

## logcat ##
The logcat command prints the logs from the android phone.

```
   adb logcat
   adb logcat -b radio
```

## shell ##
Gives you a shell on the android phone.

```
   adb shell
```

## kill-server ##
Kills the server on the host, good if the connection to the phone has been lost and needs to be restarted.

```
   adb kill-server
```

## install ##
Installs a android application package on the phone.

```
   adb install app.apk  - install app.apk on the phone
```

## devices ##
Lists all available adb devices on the host.

```
   adb devices
   ADBHOST=192.168.0.202 adb devices
```
ADBHOST sets the ip address for the network where the android phone is. This address must match the address configured with the usb network.

## push ##
Moves a file on the host to the phone.

```
   adb push HOST_FILE PHONE_PATH PHONE_FILE HOST_PATH
```

## pull ##
Moves a file on the phone to the host

```
   adb pull PHONE_FILE HOST_PATH HOST_FILE PHONE_PATH
```

**NB:** Leaving out the final path will write to the current directory



---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_