---
layout: post
date: 2026-04-06 12:00:00 +1200
title: "Installing the OTAFix Bootloader"
description: "A guide on how to install the OTAFix bootloader on nRF devices."
image: "assets/images/2026/04/06/otafix-banner.jpg"
author: taco
categories:
  - Firmware
tags:
  - Guide
---

![](/assets/images/2026/04/06/otafix-banner.jpg)

## Background
When I was first getting into MeshCore I realised that nRF52 devices had the ability to do OTA updates via BLE, which is obviously a great feature when you need to update a repeater which is on a roof.

Unfortunately I never had much luck getting this to work. If you were lucky and used the right settings it might work with RAK4631 but particularly with ProMicro devices it never worked at all. This led me to dig around in github where I found some ancient forgotten PRs to fix some of these issues.

Since they weren't getting any attention on the official Adafruit github I took it upon myself to fork the bootloader and include these fixes myself.

## Installing the OTAFIX Bootloader for nRF52 devices

For the best nRF52 OTA update experience, it’s recommended to upgrade your device to the **OTAFIX bootloader**. This version includes:

- Automatic fallback to OTA DFU mode if an update fails
- Significantly faster OTA update speeds
- Additional quality-of-life improvements for certain devices, like to the ability to enter OTA DFU mode by holding a button while resetting.

---

## 1. Enter UF2 DFU Mode

First, put your device into **UF2 DFU mode**.

For most nRF52 devices:
- Plug the device into your computer
- Quickly double-press the reset button (twice within 0.5 seconds)
- A UF2 USB drive should appear, the name of the drive will differ depending on the device.

Some devices use different methods, see the [notes](#device-specific-notes) at the end of this guide.

---

## 2. Check Your Current Bootloader (Optional but Recommended)

This step is important for devices like the **Seeed Studio XIAO nRF52840**, which often ship with the **SENSE** bootloader variant.

To check:
1. Enter UF2 DFU mode
2. Open the `INFO_UF2.TXT` file on the mounted drive

This file contains details about your current bootloader.

**Example:**
If the file shows: `Board-ID: nRF52840-SeeedXiaoSense-v1` then you will need to install the ***SENSE*** variant.


## 3. Download the OTAFIX Bootloader

You can now download the OTAFIX bootloader directly from the MeshCore flasher site!
Go to [https://flasher.meshcore.io](https://flasher.meshcore.io)

- Select your device
- Choose **Repeater**
- A download link for the OTAFIX bootloader will appear in a highlighted banner, the file will be titled `update-xxxx.uf2`

![](/assets/images/2026/04/06/flasher_banner_grab.webp)

Alternatively the OTAFIX bootloader is also available from the official site.
Go to:  
[https://github.com/oltaco/Adafruit_nRF52_Bootloader_OTAFIX/releases](https://github.com/oltaco/Adafruit_nRF52_Bootloader_OTAFIX/releases)

Download the correct UF2 file for your device from the **Assets** section.

- Files are named like: `update-xxxx.uf2`
- You may need to click **“Show more”** to see all available files

**Example:**
For the Seeed Xiao Sense variant as discussed above you will want to download `update-xiao_nrf52840_ble_sense_bootloader-0.9.2-OTAFIX2.1-BP1.2_nosd.uf2`

---

## 4. Install the Bootloader

- Drag and drop the `update-xxxx.uf2` file onto the UF2 drive
- The device will automatically:
    - Update the bootloader
    - Reboot when complete

---

## 5. Verify the Update

1. Re-enter UF2 DFU mode
2. Open `INFO_UF2.TXT` again

The version line should now include **OTAFIX**.

![](/assets/images/2026/04/06/otafix_installed.webp)

---

## Device-Specific Notes

### T1000-e DFU Mode
To enter DFU mode:
1. Attach the magnetic USB cable
2. Hold the button
3. Quickly disconnect and reconnect the cable twice

Video reference: [https://www.youtube.com/shorts/D6uo93-RcaY](https://www.youtube.com/shorts/D6uo93-RcaY)

---

### ThinkNode M3 DFU Mode
To enter DFU mode:
1. Attach the magnetic USB cable
2. Hold the button for ~25–30 seconds

