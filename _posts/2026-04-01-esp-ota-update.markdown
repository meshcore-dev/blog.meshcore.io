---
layout: post
date: 2026-04-01 12:00:00 +1200
title: "OTA Updates on ESP32 Devices"
description: "A guide for updating repeater or room server firware for ESP32 based devices."
image: "assets/images/2026/04/01/repeater-ota.png"
author: scottpowell
categories:
  - Firmware
tags:
  - Guide
---
Updating the firmware of your repeaters or room servers is super easy for ESP32-based nodes.

**1. Download new .bin**

Go to the [Github releases](https://github.com/meshcore-dev/MeshCore/releases) page, then find the related artifact, eg. [the Heltec V3 Repeater](https://github.com/meshcore-dev/MeshCore/releases/download/repeater-v1.14.1/Heltec_v3_repeater-v1.14.1-467959c.bin).
**NOTE: do _not_ download the -merged.bin!**

**2. Login to repeater/room server**

Using the mobile app, login to your device:

![](/assets/images/2026/04/01/app-repeater-login.png)

Then switch to the Command Line, and enter **start ota** command:

![](/assets/images/2026/04/01/repeater-cli.png)

You should then see a reply, like:
```
Started: http://192.168.4.1/update
```
Alternatively, you can use a standalone device like the T-Deck with the Ripple firmware to enter the 'start ota' command:

![](/assets/images/2026/04/01/xiao-start-ota.jpg)

**3. Connect with your laptop/phone**

The ESP32 device should have created a WiFi access point called **MeshCore-OTA**. Just connect to this from your laptop (or phone), and navigate to the URL that was shown in the command line response (above). You should see a page like this:

![](/assets/images/2026/04/01/repeater-ota.png)
Just take note of the ID displayed, eg the "MC Prime (Xiao C3)", as this should be the name (and MCU type) of the node you are about to update. 

Click the **Choose file** button, and select the .bin file you previously downloaded. You should then see a progress bar:

![](/assets/images/2026/04/01/ota-progress.png)

**4. Finishing Up**

Once completed, logout then log back in (either with the app or a standalone device), check that the clock is correct with the **clock** command. If it is incorrect, just issue the **clock sync** command.

Also, you might want to enter the **ver** command, to check the firmware version is now updated.

**Trouble Shooting**

If the progress bar stalled, or something went wrong, try issuing this command from phone or standalone device:
```
reboot
```
And then try the process again.

