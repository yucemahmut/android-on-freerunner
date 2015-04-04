**Under Construction (10 Oct 09)**

## Post install set-up ##

How you set up and use your system is up to you, but here are a few pointers that might help you out the first time you load Android onto your Freerunner. _It might even help all the other times too!_

The initial screen is locked and tells you to `Press the Menu key` to unlock. Push the Power button on your Freerunner to unlock the screen.

Here is a short listing of the button options:
| Menu | Power - short |
|:-----|:--------------|
| Back | Aux - short |
| Suspend | Power = medium (~2 sec) |
| Home | Aux - medium |
| Shutdown | Power - long (> 3 sec) |
| Switch apps | Aux - long |

The medium-length functions can be hard to control. An alternative way to invoke suspend is to hold Aux and then press Power. If you set home button behaviour to `Home, then sleep` under spare parts, this works to get the home screen also.

**Configuration**

You do most configuration using the `Settings` menu.  Either pull up the bottom tab on the screen or give the Power button a short press and tap `Settings`

Some of the things you should do here are:
  * Under `Sound & display` scroll all the way down to `Screen timeout` and set the screen timeout to something more than the default 1 minute (at least while you are ~~playing about~~ getting to know Android on your phone).
  * While your there you might want to adjust the brightness to make reading all these menu options easier.
  * Now backup the menu (using the Aux button) and choose `Applications` then `Development`.  Under here you want to switch on `Stay awake` so that your screen stays visible while connected to your main system.
  * Also under `Applications` you might want to activate the `Unknown sources` option as you will only be able to install non\_Market applications on your Freerunner!

Now that your phone isn't going to keep blanking out on you it's time to customize it to your liking. You will want to:
  * Choose your phones language under `Locale & text`
  * Set your timezone and time/date formats under `Date & time settings`
  * You may have to switch off `Automatic`.  This doesn't work with my provider and I need to set time and date manually.

**Mail Application**

When you register gmail account in the email app you will get an error regarding untrusted certificate, just edit manually and use these settings:
```
Incoming:
username@gmail.com
imap.gmail.com
SSL (If available)

Outgoing:
username@gmail.com
smtp.gmail.com
SSL (If available)
```
Now it should work.

## Setting up Wifi and GPRS ##

**Wifi**

You should be able to connect to open networks out of the box.  WEP & WAP networks have all been reported to work but there are many variables as to how they are set up and it may need you to modify the wpa\_supplicant.conf file and copy it to the phone.

```
adb pull /etc/wifi/wpa_supplicant.conf
```
Edit it locally using your favourite editor, then:
```
adb push wpa_supplicant.conf /data/misc/wifi/
```

**GPRS**

Yet another area which works but has a whole lot of variables which are nothing to do with android-on-freerunner! There may also be a problem getting GPRS to work with certain SIM cards.

A comprehensive list of APN details is included and this should be filtered depending on your Country (MCC) and Carrier (MNC) codes reported by your SIM card.  If they are already in the list then you should see it under `Settings, Wireless controls, Mobile networks, Access Point Names`.  Select by tapping the button next to the one you want to use.

(**Note** There are still some stability issues around GPRS.)

If your Carrier is not automatically found then you will have to contact them for the correct details and add them manually. Push the `Power` button to bring up the menu and select `New APN`.  Don't forget to pass on the details so that it can be added for future releases.

We are currently using the control list maintained by cyanogenmod - see the link to the APN control list under [the Supported Feature page](FeatureStatus.md).

## Using Dropbear SSH ##

To start using the Dropbear SSH server built in AoF Cupcake and Eclair, follow these steps:

Create the required subdirectory and copy your rsa\_host\_key using ADB:

```
ADBHOST=192.168.0.202 ./adb shell mkdir /data/dropbear
ADBHOST=192.168.0.202 ./adb push dropbear_rsa_host_key /data/dropbear
```

Enable wireless and check the IP address:
```
ifconfig eth0
```
This should provide you the IP address.

Now run the Dropbear server:
```
dropbear -v -p 22
```

and connect from your desktop to your phone (the root password is empty by default):
```
ssh root@<your phone's IP address>
```

Next, set your environment parameters:
```
export PATH=$PATH:/system/bin:/system/xbin
```

And of you go.

You no longer need the USB connection, so you could drop it using:
```
ifconfig usb0 down
```

## Setting up Busybox SSH ##

**Download**

Download the Busybox for Android [here](http://diy.zjip.com/busybox) or [here](http://verdebreuk.googlecode.com/files/busybox). Copy the files to your bin directory using ADB.

**Configure**

Go to the correct directory and create your symlinks.
On Cupcake:
```
cd ../android/_200912180730_VERSION/out/target/product/freerunner/system/bin
```
On Eclair:
```
cd ../android/_200912181145_VERSION/out/target/product/fr/system/bin
```
And execute following commands:
```
ln -s ./busybox ./find
ln -s ./busybox ./grep
ln -s ./busybox ./gzip
ln -s ./busybox ./gunzip
ln -s ./busybox ./vi
ln -s ./busybox ./watch
ln -s ./busybox ./wget
cd ..
ln -s ./bin/busybox ./sh
```

## Daily use ##

> [On the applications page](Applications.md) you can see what other users have recommended and get some links to where you can find more. Pass on your favourites.

## Support ##
> Check out the options on our [Support page.](Support.md)

## FAQ ##
> The answer to your question may already be on our [Frequently Asked Questions page.](FrequentlyAskedQuestions.md)

## Connecting to your Freerunner ##
> To connect to your Android powered phone using a usb cable you issue shell commands using ADB (Android Debug Bridge).  [You can find the details here.](AndroidDebugBridge.md)

## Supported Features ##
> Android on Freerunner is under heavy development. You can find an updated list of [Supported Features here](FeatureStatus.md).



---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_