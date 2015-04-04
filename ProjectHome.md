Project to support the port of the Android OS to the [Openmoko Freerunner](http://wiki.openmoko.org/wiki/Main_Page) mobile phone. This project is continuing the work started by Koolu and includes additional code contributed by the community. The actual code is hosted at [Gitorious](http://gitorious.org/android-on-freerunner), because Google Code doesn't support Git. Most discussions about the project is on our email list that is hosted by [GoogleGroups](http://groups.google.com/group/android-on-freerunner).
# News #
<wiki:gadget url="http://google-code-feed-gadget.googlecode.com/svn/trunk/gadget.xml" up\_feeds="http://android-on-freerunner.blogspot.com/feeds/posts/default" width="600px" height="500px" border="1"/>

# Installation Instructions #

  * Unpack the files on to a FAT formatted SD card.
  * Insert card into the Freerunner, and boot from NOR menu (hold AUX key, then power)
  * Choose `Boot from SD Card (FAT and ext2)`

The automated install process should begin. It installs the Qi bootloader, reboots, the kernel, reboots, then installs the system image.

**NOTE:** This install process overwrites **everything** on the NAND in the phone, including the bootloader. If this is not what you would like to do, please don't install this release!

There also is an SD card installer, if someone would like to avoid installing on NAND.

If you need more detailed help then check out the [wiki page on installation](GetIt.md).

# Support #

If you need help or have questions about anything please look where to get answers from on our support [page](Support.md).