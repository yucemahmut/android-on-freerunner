dfu-util -a kernel -R -D kernel.img
dfu-util -a rootfs -R -D system.img
dfu-util -a u-boot -R -D qi.img
> mount -o rw,remount -t yaffs2 /dev/block/mtdblock3 /system
> cat /sdcard/DroidSans-Bold.ttf > /system/fonts/DroidSans-Bold.ttf
> cat /sdcard/DroidSansMono.ttf > /system/fonts/DroidSansMono.ttf
> cat /sdcard/DroidSans.ttf > /system/fonts/DroidSans.ttf
> cat /sdcard/DroidSerif-Bold.ttf > /system/fonts/DroidSerif-Bold.ttf
> cat /sdcard/DroidSerif-Regular.ttf > /system/fonts/DroidSerif-Regular.ttf
> reboot
> adb remount
> adb push libmuxgxm-ril.so /system/lib/libmuxgsm-ril.so
> adb push asound.conf /system/etc/asound.conf
> adb shell
> In shell:
> > sync
> > reboot
> > > $ pmount android
> > > $ cd /media/android/data/data/com.android.providers.settings/databases/
> > > $ adb shell
      1. cd /data/data/com.android.providers.settings/databases/
> > > $ echo "REPLACE INTO \"system\" VALUES(26,'window\_animation\_scale','0.0'); \
> > > > REPLACE INTO \"system\" VALUES(27,'transition\_animation\_scale','0.0');" | sqlite3 settings.db  }}}
  Be aware off configuration files left on the sdcard data partion that can in some circumstances give you problems.

  * *Why is Donut not available?* The developers have decided to focus on getting Cupcake to be a stable, usable distribution before moving on to Donut. You can help speed up both by getting involved in issue fixing and testing.

  * *I used to be able to tell it to stay on when USB is plugged in through (I think) either the Dev Tools or Spare Parts apps, but I can't find that option now - any clues?* `Settings/Applications/Development/Stay awake`

  * *Is it possible to tell it to not use GPRS ?  I don't have a data plan and I don't want it turned on unless I absolutely need it.* Go to `Settings/Wireless controls/Mobile networks/Access Point Names`. Select each APN that shows up there, when you get to the details page, do Menu button, Delete APN. Note that you may want to write down the APN info somewhere, in case you change your mind and want to re-add it. Unfortunately, I don't know of a "Disable GPRS" switch anywhere. 

  * *Which Videos formats are supported?* This project cannot use non-free codecs due to licensing issues. (We need to research this and find out what works)

  * *I am using Windows 7. How do I ssh/adb into the phone from Windows?* Check out the [http://wiki.openmoko.org/wiki/Neo1973_and_Windows dedicated Openmoko Wiki page] or [http://forum.xda-developers.com/showthread.php?t=502010 here is a guide to using adb with Vista.]

  * *How can I make sure my contacts are not lost?* You could use the tools [http://andappstore.com/AndroidApplications/apps/109392 vcardio] or [https://android-client.forge.funambol.org/ Funambo] or [http://www.dusystems.com/importContacts.html Android Contacts Import]. Check out their website to see what fits your purpose. To make sure the contacts are not lost you should backup /data/data/com.android.providers.contacts/databases/contacts.db where the contacts are stored.

  * *I get "No contacts on sim" even when I know there are contacts on the sim card.* To import contacts from sim card the gsm firmware on the freerunner needs to be updated. [http://wiki.openmoko.org/wiki/GSM/Flashing Here] is a guide on the openmoko wiki about how to update the gsm firmware. If it still doesn't work after updating please file a issuereport stating that you have updated.

  * *No hebrew chars are showed on web pages* This because the android release does not have a hebrew font. The following describes how to download and install a font that fixes this.
    # Download and unpack http://android-on-freerunner.googlecode.com/svn/wiki/hebdroid.zip onto SD
    # adb shell
    # Use following commands
    {{{
    }}}

  * *The list of GPRS settings does not include the APN details for my provider* The AoF project is taking the list of GPRS providers from the Cyanogenmod project. Requests for changes should be reported on the Cyanogenmod wiki [http://code.google.com/p/cyanogenmod/wiki/APNlist here]. The synchronisation towards AoF is currently not automated, so you can send a message to the AoF mailinglist to request a sync. In the meantime, you can always add your provider details manually yourself in your Freerunner.

  * *The volume is really low on phone calls and people have a hard time hearing me* To fix the audio on calls, follow these steps (only works on RC1 and cupcake daily images):
    # Download these two files [http://android-on-freerunner.googlecode.com/issues/attachment?aid=-5422763061150972986&name=libmuxgsm-ril.so&token=a594185745157afdc5ec572d4de23f41 libmuxgsm-ril.so] and [http://android-on-freerunner.googlecode.com/issues/attachment?aid=9112655529052061802&name=asound.conf&token=226abe8cbe9903fecc46f778781614f9 asound.conf]
    # Using adb, push the downloaded files to your FR:
{{{
}}}
    # If you are still experiencing lower than desired volume on calls, you can install Tone Picker and increase the call volume there.

  * *Froyo is very slow. What can I do to improve responsiveness?* First thing is: try to find the bottlenecks and provide feedback or patches ;-). As a quick hack, make sure you have sqlite3 installed and have the correct permissions to do the following:
   # navigate to settings directory corresponding to your install type
     # mount android SD card and navigate to settings directory (SD version)
{{{
}}}
     # connect to your Freerunner using [AndroidDebugBridge adb] (NAND version)
{{{
}}}
   # modify database
{{{
}}}
However, feel free to help us find the actual cause and cure instead of fighting the symptoms.
----
     If you have a question that is not answered above please send it to the [http://groups.google.com/group/android-on-freerunner Mailing List at our Google Groups site]
----```