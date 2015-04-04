## Before you start ##

**You need:**
  * A fully charged Openmoko Freerunner GTA02 telephone
  * An SD memory card supported on the Freerunner (non-exhaustive list available [here](http://wiki.openmoko.org/wiki/Supported_microSD_cards))
  * Some way of connecting your SD card to your computer.
  * Root priveliges on your Linux system

If required, you can find very comprehensive **additional information**  on most of the points covered below at the [Openmoko Wiki site](http://wiki.openmoko.org/wiki/Main_Page).

**Update GSM firmware**

Some of the issues with reading SIM card contacts, wakeup interrupts, etc. can be fixed by updating the GSM firmware on your phone to the latest version - currently Moko11. See [the relevant OpenMoko wiki page](http://wiki.openmoko.org/wiki/GSM/Flashing) for full instructions on checking your version and how to update.

### Backup your data - Manually ###

**Be warned!**  Loading a new build means clearing everything from your SD card and your telephone's memory, including the bootloader. Backup any personal files or modified config files you want to keep from your Freerunner now!

To back up your contacts and SMS data, create a local directory to store the data and pull the data from your Android phone using ADB.

```
$ mkdir backup
$ mkdir backup/data
$ adb pull /data/data/com.android.providers.contacts/databases/contacts.db backup/data/contacts.db
$ adb pull /data/data/com.android.providers.telephony/databases/mmssms.db backup/data/mmssms.db
$ adb pull /data/data/com.android.alarmclock/databases/alarms.db backup/data/alarms.db
$ adb pull /data/data/com.android.providers.settings/databases/settings.db backup/data/settings.db
$ adb pull /data/data/com.android.providers.calendar/databases/calendar.db backup/data/calendar.db
$ adb pull /data/data/com.android.settings/ backup/data/com.android.settings/
$ adb pull /data/data/com.android.settings/shared_prefs/com.android.settings_preferences.xml backup/data/com.android.settings_preferences.xml
$ adb pull /data/data/org.andnav2/ backup/data/org.andnav2/
```


### Backup your data - Automated ###

We've adapted the script by Teodor Robas who created the initial script to automatically backup contacts, sms, preferences, wallpaper, calendar and so on. It creates a restore script for pushing these back to the Freerunner after flashing a new image.  This restore script will also install busybox, a file explorer, and sets the date and time correctly.

You can [check out the adapted script here.](http://code.google.com/p/android-on-freerunner/downloads/detail?name=backup-android.sh&can=2&q=)

### Partition and format the SD card ###

For Android to work properly the Micro SD Card in your FreeRunner needs to have 2 primary partitions. The first needs to be a VFAT/MSDOS (16 or 32) partition which is used is mounted as '/sdcard', this is used as a  storage area (for pictures, movies, music, etc). The second is an ext3 partition which Android mounts as '/data', this is where it stores settings, caches, etc.

Experience has shown that sizing these at a ratio of 3 to 1 works best. So if you have a 4GB card make a 3GB vfat partition and a 1GB ext3 partion.

Experience has also shown that it is best to delete all existing partitions and create new when installing a new build. _(This greatly helps the testing and bug fixing process)._

Comprehensive information on partitioning and formatting your SD card can be found on [the Openmoko Wiki.](http://wiki.openmoko.org/wiki/Android_on_Freerunner#Preparing_the_SD_Card)

**Note** Users have reported that it is also possible to install Android by making a single partition formatted as VFAT/MSDOS.

## Choose your build ##

**First step is to decide which version of Android you want to install.**

One of the big plus points with Android is that Google push regular updates. The downside is that this project doesn't have the capacity to maintain several branches.

The current maintained options are **Cupcake (Android 1.5)** - as stable as it gets on the Freerunner with most functions working or **Froyo (Android 2.2)** - so bleeding edge as it hurts.

There is also a Master branch which is following the ["Android Open Source Project"](http://source.android.com/) and will form the basis for future development.  Allowing us to piggy-back on code releases from Google without going back to the start every time.

**Next decide if you will create your own build:**  [See detailed instructions here](http://code.google.com/p/android-on-freerunner/wiki/BuildIt)

**or if you want to use one of the prepared builds available:**
  * Download the latest "stable" Cupcake build version [featured on the Downloads tab on this site.](http://code.google.com/p/android-on-freerunner/downloads/list)
  * Download the current Froyo build [from the Downloads tab on this site.](http://code.google.com/p/android-on-freerunner/downloads/list) Be aware that these include the latest commits and are experimental.

NB: To install the SD builds on the Download tab [see here for details.](http://code.google.com/p/android-on-freerunner/wiki/AndroidOnSd)

We are grateful to Ran and Cley Faye for providing the prepared builds.

## Installing ##

Start by opening a terminal and change to the directory you downloaded the build file to.  Next you need to attach your squeaky clean, newly formatted SD card to your system.

To mount it you need to know what device name your system has allocated to it. This can vary due to your distribution's rules and your hardware setup.  If you are not sure then plug in your card then try running -
```
$ dmesg | tail
```

You should see something like -
```
scsi 5:0:0:0: Direct-Access     Multi    Flash Reader     1.00 PQ: 0 ANSI: 0
sd 5:0:0:0: Attached scsi generic sg2 type 0
usb-storage: device scan complete
sd 5:0:0:0: [sdb] Attached SCSI removable disk
sd 5:0:0:0: [sdb] 3862528 512-byte logical blocks: (1.97 GB/1.84 GiB)
sd 5:0:0:0: [sdb] Assuming drive cache: write through
sd 5:0:0:0: [sdb] Assuming drive cache: write through
 sdb: sdb1 sdb2
```

If your system doesn't automatically mount the new partitions then you need to mount the vfat (first) partition as root. Create a directory under /mnt or use an existing one there.  In this case /tmp1  -
```
# mount /dev/sdb1 /mnt/tmp1
```

Now you need to unpack the files from the build tar that you downloaded. First change to the directory you just mounted -
```
$ cd /mnt/tmp1
```

Now run the unpack commend as root -
```
# tar -xvzf /path/to/build/file/build.tar.gz
```

Don't worry if you see some messages about not being able to change the ownership of files.

First move out of the mounted directory -
```
$ cd ~
```

Now unmount the SD card as root -
```
# umount /mnt/tmp1
```

**Note:** Failing to unmount or sync the SD card before using it can lead to problems with the  boot process below.

Remove the SD card from your system and insert it into the Micro SD card in the Freerunner.  This is located below the SIM card.

## Go! ##

When you have reassembled your Freerunner, hold the 'Aux button' (top left) in while you push the 'Power button' (bottom right).  You should now see a menu with several choices, keep pushing the Aux button until you highlight the Option to "Boot from microSD card (FAT+ext2)".  While this option is highlighted push the 'Power button' again.

**Now sit back and watch the fun!** Here's a short listing of what you should see during a normal installation:
  * Several boot messages
  * A message that the kernel is starting
  * A scary white screen for what seems forever but is only a few seconds
  * Lots of scrolling text
  * That white screen again (don't panic)
  * An andorid-on-freerunner splash screen! (exactly what depends on how creative Damir's been lately).  This will be there for quite a while on the first boot. **NB:** "quite a while" = several minutes, if it's still there after 10 minutes something has gone wrong.  Go back to the start and double check all the preparation steps!
  * Very fancy Android letters with rippling light effect.
  * **Android start screen - locked!**

**Now you can use it!** Go to [Use it](UseIt.md) page to find out how.

**Note** The first boot takes **much** longer than subsequent ones, so don't worry.

### Restore your data - Manually ###

To restore your contacts and SMS data, push your backup to your Android phone using ADB.

```
$ cd backup
$ adb push backup/data/contacts.db /data/data/com.android.providers.contacts/databases/contacts.db
$ adb push backup/data/mmssms.db /data/data/com.android.providers.telephony/databases/mmssms.db
$ adb push backup/data/alarms.db /data/data/com.android.alarmclock/databases/alarms.db
$ adb push backup/data/settings.db /data/data/com.android.providers.settings/databases/settings.db 
$ adb push backup/data/calendar.db /data/data/com.android.providers.calendar/databases/calendar.db 
$ adb push backup/data/com.android.settings/ /data/data/com.android.settings/
$ adb push backup/data/com.android.settings_preferences.xml /data/data/com.android.settings/shared_prefs/com.android.settings_preferences.xml
$ adb push backup/data/org.andnav2/ /data/data/org.andnav2/
$ adb shell sync
$ adb shell reboot
```

### Restore your data - Automated ###

To restore your data, run the restore script generated by the backup script. This restore script will also install busybox, a file explorer, and sets the date and time correctly.


<a href='Hidden comment:  Removed until Serdar is back in business :-)
=Daily Builds=

There are daily builds made by serdar, these are build from the repo every night European time, or more often if there is some patch to be tested. These are for testing purposes and should be avoided if you aren"t interested in testing some specific path.
http://serdar-dere.net/~serdar/daily/
'></a>


---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_