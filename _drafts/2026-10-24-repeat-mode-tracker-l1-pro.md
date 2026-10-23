---
layout: post
date: 2026-10-23 12:00:00 +1200
title: "Companion Repeat Mode with the Wio Tracker L1 Pro"
description: "How to use companion repeat mode with your Wio Tracker L1 Pro"
image: "assets/images/2026/10/24/image.png"
author: seeedstudio
categories:
  - Guides
tags:
  - WioTrackerL1Pro
---

When a hiking group spreads beyond direct radio range, a device between the two ends may be able to help relay messages. This guide shows how to enable Companion Repeat Mode on a [Wio Tracker L1 Pro](https://www.seeedstudio.com/Wio-Tracker-L1-Pro-for-Meshcore-p-6717.html?sensecap_affiliate=lLZqP0c&referring_service=blog.meshcore.io), check that forwarding works, and use the results from a three-device field test to plan a temporary deployment.


## How Companion Repeat Mode Helps

On a hike, a group can spread out as some people move ahead and others fall behind. If the devices at the front and back can no longer exchange messages directly—and there’s no nearby repeater—how can the group stay connected?



MeshCore normally keeps Companion devices from forwarding packets. For certain off-grid situations, Companion Repeat Mode allows a device to take on a temporary relay role. We took three L1 Pro units into the field to see how that setup works—and what to consider before relying on it.

<img alt class="mx-auto" src="/assets/images/2026/10/24/image%205.png" />

Wio Tracker L1 Pro combines a built-in 2,000 mAh battery, an OLED display, an antenna and a protective enclosure in one portable device. It can be carried in a pocket or attached to a backpack with a lanyard or hook. The display is useful for checking device status, while the integrated battery makes the L1 Pro practical for a multi-hour field test. See the [Wio Tracker L1 Series overview](https://wiki.seeedstudio.com/wio_tracker_l1_node/?sensecap_affiliate=lLZqP0c&referring_service=blog.meshcore.io) for product details.

<img alt class="mx-auto" src="/assets/images/2026/10/24/image%202.png" />

## Enabling Companion Repeat Mode

For this setup, A and C are the two endpoints, and B is the device between them that will relay messages. To enable Repeat Mode on B, follow these steps:

1. Put all participating devices on the same supported off-grid radio setting.

2. Connect the MeshCore app to device B and open its radio settings.

3. Enable **Repeat Mode**

4. Save the setting and leave B powered on between A and C.

<img alt class="mx-auto" src="/assets/images/2026/10/24/image%203.png" />

If the app warns that the selected frequency does not support Repeat Mode, choose a supported off-grid setting that is permitted for your location. MeshCore restricts Companion Repeat Mode to specific off-grid configurations to help prevent portable clients from adding unnecessary forwarding traffic to a public mesh. 



**In our setup, B continued to use Companion firmware**; we enabled Repeat Mode through the app, without reflashing it as a dedicated repeater. After the setting was saved, the device handled forwarding itself. 



## What We Observed in the Field

We then tested this setup outdoors to see how the phone connection and relay placement affected message delivery. For the field test, all three devices ran the same Companion firmware version and used the radio settings described above. Before testing, they exchanged adverts and were added as contacts. We chose an off-grid setting supported by the app and firmware, and confirmed that the selected frequency was appropriate for the test location.



The straight-line distances were approximately **4 km from A to B** and **4 km from B to C**. In Tests 1–3, B remained at the same location, surrounded by trees. 

<img alt class="mx-auto" src="/assets/images/2026/10/24/image%204.png" />

For Test 4, A and C stayed in place while B moved roughly perpendicular to the A–C line to a higher, more open location. The A–B and B–C distances remained approximately unchanged.

<img alt class="mx-auto" src="/assets/images/2026/10/24/image%201.png" />

We sent ten short messages from A to C in each round:

The results show that B continued forwarding messages after its phone was disconnected. Moving B to a higher, more open location increased delivery from **4/10 to 8/10**, despite approximately unchanged distances. In this test, improving the relay’s position made a substantial difference to communication reliability.



## Practical Tips for a Temporary Deployment

The field results highlight how much relay placement can matter. Before using this setup on a trip, check the following.

1. **Agree on the radio setting before you lose coverage.** Every node in the group needs to use matching settings. Choose a supported off-grid configuration that is legal in your region, and test it before the trip.

2. **Put the relay where it can hear both sides.** A device inside a backpack may be shielded by the wearer or surrounding materials. Try clipping B outside the pack, keeping its antenna upright, or placing it higher and stationary. Test changes one at a time so you can tell which placement helped.

3. **Track battery use.** The L1 Pro has an integrated 2,000 mAh battery, but actual runtime depends on usage and conditions. Note the battery level before and after each test, and bring a power bank for longer outings. Do not assume that a companion device will match the endurance of a dedicated, power-optimized repeater.

## When to Use a Dedicated Repeater

Companion Repeat Mode is useful when a hiking group, field team or temporary outdoor event needs a quick relay and no fixed repeater is available. It can also help teams map coverage before deciding where dedicated repeaters should be installed.

<img alt class="mx-auto" src="/assets/images/2026/10/24/image.png" />

**It is not a replacement for a well-positioned, continuously powered repeater.** A portable node can move, lose battery or leave the route where other users need it. For a fixed coverage area or long-term public mesh, use infrastructure designed for that role. The L1 Pro can stay in your pocket as a Companion during normal use—and become a temporary relay when the group moves beyond existing MeshCore coverage.



