---
layout: post
date: 2026-04-02 12:00:00 +1200
title: "OTA Updates on nRF Devices"
description: "A guide for updating repeater or room server firware for nRF based devices."
image: "assets/images/2026/04/02/nRF-DFU.webp"
author: scottpowell
categories:
  - Firmware
tags:
  - Guide
---

Updating the firmware of your repeaters or room servers is super easy for nRF-based nodes.

<img alt class="mx-auto" src="/assets/images/2026/04/02/nRF-DFU.webp" />

**1. Download new .zip**

Go to the [webflasher](https://flasher.meshcore.io/) and find your device, then select the Repeater or Room Server role, then latest version. In the bottom-right, click the **Download** button, then select .zip file:

<img alt class="mx-auto" src="/assets/images/2026/04/02/nRF-zip-download.png" />

Alternatively, go to the [Github releases](https://github.com/meshcore-dev/MeshCore/releases) page, then find the related artifact, eg. [the T114 Repeater](https://github.com/meshcore-dev/MeshCore/releases/download/repeater-v1.14.1/Heltec_t114_repeater-v1.14.1-467959c.zip).

**2. Login to repeater/room server**

Using the mobile app, login to your device:

<img alt class="mx-auto" src="/assets/images/2026/04/01/app-repeater-login.png" />

Then switch to the Command Line, and enter `start ota` command:

<img alt class="mx-auto" src="/assets/images/2026/04/01/repeater-cli2.png" />

You should then see a reply, like:
```
OK - mac: FF:AA:BB ...
```
Alternatively, you can use a standalone device like the T-Deck with the Ripple firmware to enter the `start ota` command:

<img alt class="mx-auto" src="/assets/images/2026/04/01/xiao-start-ota.jpg" />

**3. Connect with your phone**

If you don't have it installed already, you will need to install the Nordic **nRF Device Firmware Update** app, which you can find on [Google Play](https://play.google.com/store/apps/details?id=no.nordicsemi.android.dfu) and the [App Store](https://apps.apple.com/us/app/nrf-device-firmware-update/id1624454660).

**3.1 DFU App Settings**

In the app, tap on **Settings** icon and use these recommended settings changes:
* Packet receipts notification - **ON**
* Number of packets - **8**
* Request high MTU (Android only) - **OFF**
* Disable resume - **ON**
* Prepare object delay - **0 ms**
* Force scanning - **ON**

**3.2 Start update**

In the app, select the .zip file you downloaded and select the Device, Then press the **start** button:

<img alt class="mx-auto" src="/assets/images/2026/04/02/dfu-app.png" />

And that's it! 

**4. Finishing Up**

Once completed, logout then log back in (either with the app or a standalone device), check that the clock is correct with the **clock** command. If it is incorrect, just issue the **clock sync** command.

Also, you might want to enter the **ver** command, to check the firmware version is now updated.

**Trouble Shooting**

If the update stalled or failed, try issuing this command from phone or standalone device:
```
reboot
```
And then try the process again.

