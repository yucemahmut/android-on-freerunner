In general, the setup necessary to build Android-on-Freerunner is the same as for the main Android project. See

  * [Initialise your system](http://source.android.com/source/initializing.html) for instructions on setting up your system

  * [Download the code](http://source.android.com/source/downloading.html) for instructions how to download the code

**One important difference:** The AoF-specific source code is hosted at our Gitorious repository, so to checkout AoF code use the following command (not the one on the source.android.com page):
```
 $ repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b froyo
```
or
```
 $ repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b cupcake
```

The `-b` parameter on the repo command specifies the branch to check out, currently either `cupcake` or `froyo`.

Next, just type the following to get the source code:

```
 $ repo sync
```


  * [Initialise your build](http://source.android.com/source/building.html) for the first paragraph on sourcing the build/envsetup.sh. You can forget about the rest of that page in the context of AoF.

# Setting up your system #
In general, for either Mac OS/X or Linux build machines, follow the instructions at pages mentioned above. Notes on specific situations are below.

To build Froyo, do:
```
    $ make -j 4 TARGET_PRODUCT=fr
```

To build Cupcake, do:
```
    $ make -j 4 TARGET_PRODUCT=freerunner
```


**NOTE:** The -j parameter will try to optimise the compilation process. The number should represent the double of the number of cores on your system. In this example, the source is compiled on a dual core system. If you are trying to diagnose a build error, not using the `-j` parameter may make the diagnostics easier to read.

## Ubuntu Linux ##

Install the packages recommended on the Android page for Ubuntu (32 or 64 bit). Ubuntu 9.10 (Karmic Koala) and later no longer have Java 5 (which is required) in the repositories. You can either manually download and install it from the [Oracle page](http://java.sun.com/javase/downloads/index_jdk5.jsp), or you can add the old repository lines to your sources.list. See [this page](http://blog.enea.com/Blog/bid/32050/Ubuntu-9-10-Java-5-and-the-Android-Open-Source-Project) for instructions on the two approaches.

Also in 9.10 and later, the default GCC version has changed, so gcc-4.3, g++-4.3, liblzo2-dev and uboot-mkimage must be installed:
```
    # sudo aptitude install gcc-4.3 g++-4.3 liblzo2-dev uboot-mkimage sun-java5-jdk
```

Whichever of the methods above you've used to install Java 5, you need to point the Android Java variables to the correct location.  Something like:
```
    $ export JAVA_HOME=<path-to-jdk1.5.0_22>
    $ export ANDROID_JAVA_HOME=$JAVA_HOME 
```

Additionally you must set the proper GCC version with environment variables:
```
    $ export CC=gcc-4.3 
    $ export CXX=g++-4.3 
```

In case you have multiple JREs installed, make sure to set the default to the version required for your build. To find out what JREs you have installed run the following:

```
    $ sudo java-update-alternatives --list
```

or

```
    $ sudo update-alternatives --list java
```

To set the corect JRE, type:

```
    $ sudo java-update-alternatives --set [the correct 1.5 JRE]
```

or

```
    $ sudo update-alternatives --set java [the correct 1.5 JRE]
```


Building with 64-bit OS seems to be more problematic. See the list of packages at the Android site, keeping in mind that you need GCC 4.3 versions of some of the listed packages (cc-4.3-multilib and g++-4.3-multilib). If you come up with a configuration that works, please post it to the mailing list. On the other hand, if you are having problems fell free to ask for help!

# The Installer branch #
The installer branch is important in case you want to build your own installer package like the daily builds or releases. The installer branch includes the qi bootloader and a kernel with busybox and mtd-utils.

  * Prepare your system by downloading and extracting the OpenMoko toolchain to the correct location as described in [this paragraph](http://wiki.openmoko.org/wiki/Toolchain#Downloading_and_installing). You will this toolchain for compiling Buysybox as part of the installer.

  * Install the xutils-dev package since you will be needing lndir as part of the make process

```
    $ sudo apt-get install xutils-dev
```

  * Create a separate dir and get the source code on your system:

```
    $ repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b installer
```

Now get the code by typing:
```
    $ repo sync
```

  * Once this process is done, compile the source by simply typing:

```
    $ make
```

Now look for the files called qi.img and uImage.img.

# Installation Package #

For those who wish to build their own AoF installation package from the local repository, we have created a little script that will do this for you. Pre-requisite is that you've successfully compiled your AoF branch (e.g. Cupcake or Froyo) and the installer branch.

The script, called PackageInstaller.sh, is part of the installer branch and can be found in the installer directory.

The script requires two parameters: the path to your local installer branch and the path to your AoF branch.

An example of how to use the script:

```
    $ ./aof_installer/installer/PackageInstaller.sh ./aof_installer ./aof_cupcake
```

The script will create the a file called aof-installpackage-YYYYMMDD.tar.gz in your current directory.

Copy your tar.gz to your SD card and you're ready to install your own AoF distro ;-)


---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_