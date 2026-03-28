---
layout: post
date: 2025-12-12 12:00:00 +1200
title: "The Year In Review"
description: "Well, what a year! MeshCore is almost 1 year old now."
image: "assets/images/2025/12/12/year_in_review.jpg"
author: scottpowell
---
![](/assets/images/2025/12/12/year_in_review.jpg)

_Generated with Grok by xAI_

Well, what a year! MeshCore is almost 1 year old now, and I thought it a good time to reflect on this first mega year, and remember the milestones we've smashed.

**Just Do It**

Over the 2024/2025 silly season, I decided to hunker down, take the phone off the hook (so to speak) and force myself to write the first draft version of the MeshCore C++ library. I had scribblings on paper from prior months and had what I thought was a workable idea. I hadn't worked everything out, but I've found that just trying to code an idea sometimes reveals a solution (and/or flaws in an idea). I'm not sure if this phenomenon has a name, but I've always found this process to have an element of magic to it. But, as I wrote the initial code, it was surprising how many things just fell into place.

**Bob and Alice**

The very first MeshCore messages were sent between Bob and Alice, in early January:
![](/assets/images/2025/12/12/bob_and_alice.png)

These were two Heltec V3 boards running the 'Simple Secure Chat' (terminal) app, with two Serial Monitors running. None of the contact management code had been written, so Bob and Alice were two hard-coded key-pair identities.

**That UK Chap, Andy Kirby**

I had been talking on and off with Andy over 2024 about various mesh topics, and was hinting at a new system that might materialise, and thankfully Andy really jumped at the prospect of MeshCore, and was the very first real-world tester:
![](/assets/images/2025/12/12/andy_photos.jpg)

**Teamwork Makes the Dream Work**

Andy published a number of early YouTube videos even when the system was getting its legs, and it made a splash. Discussions were all on his Discord server, and he already had quite a large following of mesh nerds discussing things, so there was an almost instant cross over, of peeps willing to give MeshCore a try.

I don't remember the dates, but pretty soon a core team ended up materialising:
* Andy Kirby, OG tester, YouTube promoter, community engagement, UK website.
* Rastislav Vysoky, who wrote the first web client, the meshcore map, the webflasher, and almost single handedly manned tech support
* Liam Cottle, who was already very involved in the Reticulum project, jumped over and wrote an even better web client, then smashed out a native mobile client in Flutter in seemingly no time.

For about six months this was the core team, until Florent de Lamotte was asked to join after his tireless work with his Python-based mc-cli integrations.

**Milestones**

There's a crazy amount of stuff to try to recollect, but I thought just some broad stats could be interesting:
* GitHub releases: **23** (v1.0.0 to v1.11.0)
* Git commits (non-merge): **1453**
* Number of boards supported: **60+**
* Repo contributors: **69**
  *	Top 8: fdlamotte, recrof, oltaco, liamcottle, jquatier, jbrazio, 446564 (ded), cod3doomy
* MeshCore map nodes uploaded: **10,625**  (as at 11 Dec)

Since the creation of the [Lets Mesh Analyzer site](https://analyzer.letsme.sh/stats) and observer nodes, we now have insights into actual usage of MeshCore meshes around the world. Some current stats:
* Unique packets per day: **~80K**
* Unique active nodes per day: **~4.5K**
* Unique public messages per day: **~4.4K**
* Total bytes of traffic per day: **~52 million**

**The Pain Points**

Over this first year there were a number of painful issues which took a lot of effort to resolve:
* Deaf-gate:  This is the name we assigned to the mysterious habit of repeaters to just go deaf, seemingly at random times. This one evaded explanation for many months. Eventually it was discovered to be an AGC (automatic gain controller) glitch, which needed a force reset in the LoRa modules. A transmit would temporarily clear this problem, but because MeshCore is frugal on transmits, sometimes a repeater would stay deaf for hours. The "set agc.reset.interval ..." was the eventual workaround.
* Packet corruptions: only in the very early months when I errantly assumed we didn't need to use hardware CRC's.
* iOS and BLE disconnects:  Forever the thorn in Liam's side throughout this year.
* Little-(F)FS corruptions:  The plague on all of the nRF52 users. Mysterious low power situations, BLE disconnections, and borked LFS blocks. Thanks to the amazing work from Taco, and the ExtraFS system, this problem has been reduced. (still rears its head, tho :-(  )
* Room server glitches:  It took waay too long to figure out about three really bad bugs that were in the Room server firmware. Is sad as this really hampered adoption of rooms, and it still stings a bit, as usage is relatively rare.
* The T1000e:  Was plagued with issues for most of the year, from LR11xx issues, deafness, lockups, LFS corruptions, etc. Thankfully the issues are pretty much ironed out now.
* CAD and packet collisions:  This one took a number of attempts to get right. I think there were something like 3 major implementations of CAD tried, and (surprise, surprise) the simplest was the best.

**Timeline**

![](/assets/images/2025/12/12/timeline.png)

A picture paints a thousand code commits. :-) Here are most of the major events of 2025. Tiring just looking at it. phew.  March was a pretty wild month. Liam and I were cranking out features/updates at pretty wild pace. (apologies if something you worked on/contributed isn't in this chart. Took hours of going over git logs and release notes to come up with the subset above)

I have no idea what 2026 is going to look like! Will be interesting to compare charts at the end of next year.

**The MeshCore Spirit**

Something that really gets me _in the feels_ is how many individuals have taken up the challenge in their locales to either spread the word, or actually spread the network by building and deploying repeaters. In many key geographies, this has sometimes rested on the shoulders of a small handful of people, who have scaled mountains, trees and towers to get backbones built out.

In my own state of Victoria, Australia, there has been a huge increase in the mesh in just the past two months, and we now have messages reaching many hundreds of kms, across about half the state. This is largely down to a handful of brave souls (eg. @silentlightning) finding key mountain tops, and linking it all out. I must admit, I never anticipated that a mesh could go any further than just metro Melbourne. Was happy to be wrong about that one. :-)

There is also a very resourceful spirit of repurposing affordable gear in DIY repeater builds. There are a lot of repurposed solar flood lights out there that do remarkably well, and there's a bit of a running gag here in Melbourne that you're not a real MeshCorer unless you have a Bunnings pool pole as a mast. Pool pole sales have skyrocketed this year.

![](/assets/images/2025/12/12/montage2.png)

So, to all you good, freedom-loving folk out there, I tip my hat to you. Well done!

**Onwards...**

In just a year we did this:
![](/assets/images/2025/12/12/worldmap.png)

Let's keep lighting up the world.

_---sincerely, Scott Powell_
