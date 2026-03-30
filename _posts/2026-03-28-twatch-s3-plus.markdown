---
layout: post
date: 2026-03-28 12:00:00 +1200
title: "LilyGo T-Watch S3 Plus"
description: "Info about the new Ripple firmware release the for the T-Watch S3 Plus."
image: "assets/images/2026/03/28/twatchs3plus.jpg"
author: scottpowell
categories:
  - Ripple
tags:
  - Release
---
I've now added support for the [T-Watch S3 Plus](https://www.lilygo.cc/products/t-watch-s3-plus?bg_ref=cq3pUU7cD3). This is a feature packed programmable watch from LilyGo, with integrated GPS, sx1262 LoRa radio, and 1.5" colour touch screen.

![](/assets/images/2026/03/28/twatchs3plus.jpg)

**Download**

You can download + flash in one easy step using this [direct link](https://flasher.meshcore.io/ripple-lilygo-t-watch-s3-plus/ripple-gui/v9.3) to the MeshCore Flasher target.

**UI Navigation**

The watch has 3 physical buttons, one on left/top shoulder, one on right-top shoulder, and a power button along the right edge:

![](/assets/images/2026/03/28/twatchhelp.png)

NOTE: the top-right shoulder button is the RESET button, which will reboot the device.

The UI navigation is cursor based, where you move the dark-gray selection up/down by tapping in the top and bottom areas of the screen, per the picture above. You then tap on the right side to select, and tap on the left side to navigate back to previous screen.

Some screens require ENTER key to submit/save/send, and this is done with a short press of the power key. A long press (approx 4 seconds) powers off the device.

Press the top-left shoulder button to turn the screen on/off.

**Watch Face/Lock Screen**

The screen automatically turns off after inactivity, but there is a special lock screen which comes on when the device is in standby and when you tilt your wrist to look at the watch face:

![](/assets/images/2026/03/28/twatchlock.png)

**General Guide**

The T-Watch S3 has a UI which mostly like the other Ultra devices, like the T-Deck, so refer to the [general user guide](https://buymeacoffee.com/ripplebiz/ultra-v7-7-guide-meshcore-users) here for details.

**Kid Mode**

The T-Watch makes for a great kid-tracker with the Ripple UI's Kid Mode. The device can be locked down so that only kid-friendly contacts are shown and no contact from strangers is possible. See [this article](https://blog.meshcore.io/2025/10/02/kid-mode) on how it works.

![](/assets/images/2026/03/28/twatchkidmode.png)

**NEW: Custom Watchfaces**

The 9.3 version of the firmware has some nice customisations for the watch-face/lock screen.

You can now set a **Display** preference of **Digital** or **Analog**. With both, you can upload a custom PNG background image! For the Analog option, you need to find an image _without hands_ as the hour/minute hands are rendered on top of this background:

![](/assets/images/2026/03/28/custom-watchfaces.jpg)

To customise the digital watchface background, just drop a 240x240 PNG file called **default-face.png** to root directory.

To customise the analog watchface background, just drop a 240x240 PNG file called **analog-face.png** to root directory.

**NEW: Maps**

The v9.3 can now handle _some_ map tiles, and rendered in the map view:

![](/assets/images/2026/03/28/twatch-maps.png)

**The 'LittleFS' Firmware**

The v9.3 firmware now has two variants, the legacy one for those who just want to upgrade and _not_ use the watchfaces or maps, and the "-LFS" variant.

The -LFS variant uses a different partition scheme, so if you flash this variant over the current one you _will lose your settings_. But, once you have this variant it will be safe to upgrade future versions without losing data.

This new variant, it carves out a LittleFS partition about 14Mb in size. So, you can drop map tiles up to about that size. This is fairly limited, but you should be able to drop some subset of your local geography for a few zoom levels.

Please see the [FAQ about how to get map tiles](https://docs.meshcore.io/faq/?h=maps#47-q-how-do-i-get-maps-on-t-deck).

**Copying Files to LittleFS**

There is a handy online tool called [ESPConnect](https://thelastoutpostworkshop.github.io/ESPConnect/) which makes it easy to connect the T-Watch via USB to your computer, then add/modify/delete the files in the LittleFS partition.

Just click on the **CONNECT** button at the top, select the port for your USB connection, then click on the **LittleFS Tools** tab:

![](/assets/images/2026/03/28/espconnect-littlefs.png)

You'll need to be patient while it does a _full read_ of the partition, but once done, it will ask you to click **BACKUP** (which is very handy), that saves the entire partition to a .bin file.

If you look below, you should then see the various files/folders in the file system. Just drag-n-drop files, like 'analog-face.png' to add to the watch. You can also do this for the map tiles, just drag/drop the whole 'tiles' folder you have prepared on your computer. NOTE: you will see this warning if you've blown the size limit:

![](/assets/images/2026/03/28/espconnect-full.png)

You should see the orange warning **Unsaved changes** after adding/removing files/folder, and once you have made all the changes need, just click the blue **SAVE TO FLASH** button:

![](/assets/images/2026/03/28/espconnect-save.png)

Again, you'll need to be patient as it writes the entire partition back to the watch. But, once it's done just disconnect the watch, then press the Reset button on the top-right shoulder of the watch.

