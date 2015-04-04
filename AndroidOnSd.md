## Introduction ##

With AoF on SD card it is **not** necessary to flash anything to NAND of your Freerunner, not even another bootloader. The Howto here assumes you have an SD card layout where your first (primary) partition is FAT32 and your second is ext2/3. For other setups have a closer look at the patches to modify them according to your needs. Anything beyond the 2nd partition does not matter. See also [SD partition layout](#SD_partition_layout.md) part of this page.

## Details ##

Before continuing, make sure to understand the general principles explained in [Improve it](ImproveIt.md) and [Build it](BuildIt.md).

The modifications and instructions below are different for Cupcake and Froyo. Please skip to the section relevant for you branch (Cupcake or Froyo).

### Cupcake ###

Do the following steps in a directory where you want to build Cupcake. All prerequisites should be set up.

  1. get a copy of Cupcake SD repo:
```
   $repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b cupcake-sd
```
  1. build Cupcake
  1. copy the necessary directories and files onto the 2nd partition (ext2/3) of your SD card using [this script](#Copy_to_/_Update_your_SD_installation.md).

### Froyo ###

Do the following steps in a directory where you want to build Froyo. All prerequisites should be set up.

  1. get a copy of Froyo SD repo
```
   $repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b froyo-sd
```
  1. build Froyo
  1. copy the necessary directories and files onto the 2nd partition (ext2/3) of your SD card using the [copy script](#Copy_to_/_Update_your_SD_installation.md).
  1. Boot Froyo once and replug the SD card into your PC again and modify the Froyo settings to increase responsiveness. See the FAQ section for detailed instructions.

## Scripts ##

### Copy to / Update your SD installation ###

Make sure you have the correct permissions to do the following and you have `rsync` installed.

```
#!/bin/bash

#replace by the directory where your build is
FROM="/mnt/android-dev/aof/out/target/product/freerunner"
#replace by mountpoint of SD card
TO="/media/android"
RSYNCOPTS=" -aP "

rsync $RSYNCOPTS $FROM/root/ $TO/
rsync $RSYNCOPTS $FROM/system/ $TO/system/
test -d "$TO/boot" || mkdir $TO/boot
rsync $RSYNCOPTS $FROM/kernel.img  $TO/boot/uImage-GTA02.bin
cp -v $TO/init $TO/sbin/init
chmod --changes -R 777 $TO
```

## SD partition layout ##

Use your favourite partitioning tool to create/modify the partitions on your SD card to have one FAT32 partition at the beginning, followed by one ext2/3 partition for the AoF system. Example layout:

| partition number | mmcblk0p1    | mmcblk0p2 | mmcblk0p3 | mmcblk0p5 |
|:-----------------|:-------------|:----------|:----------|:----------|
| filesystem       | vfat         | ext3      | ext3      | ext3      |
| used for         | AoF storage  | AoF system| system X  | storage X |

Note that for best SD usage mmcblk0p1 should be used for a third system and "AoF storage" placed in a logical partition. See [Qi boot order](http://wiki.openmoko.org/wiki/Qi#Boot_Order) to understand why. However, this has not yet been tested. Volunteers are welcome :)

Concerning size it depends on how much you want to put in your partition. The system partition should be big enough for system + some additional space for config and other applications. The original 512M card is fine for an AndroidOnSd installation (FAT + ext3).

## Prepared Builds ##

There are builds for both Cupcake and Froyo SD versions made by Ran and hosted by Serdar.  These are updated as and when there is some patch to be tested and should not be used as your daily phone. But feel free to provide feedback.
[http://serdar-dere.net/~ran/](http://serdar-dere.net/~ran/)


---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_