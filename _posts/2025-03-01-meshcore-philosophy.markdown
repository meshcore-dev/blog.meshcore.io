---
layout: post
date: 2025-03-01 12:00:00 +1200
title: "MeshCore Philosophy"
description: "Some guiding principles behind the project."
image: "assets/images/2025/03/01/techphilosophybyGrok.jpg"
author: scottpowell
---
![](/assets/images/2025/03/01/techphilosophybyGrok.jpg)
_Generated with Grok by xAI_

The rapid rise and development of MeshCore is really great to see, and I hope it continues, but there is a risk of things like fragmentation and splintering, among other things, if some of the principles behind the design are lost or ignored. So, a summary on what lead to its design, and where I see it going hopefully will help.

**Background**

Most probably know of the existing mesh system, and the frustrations fairly universally experienced. My main issue was the desire for an open ecosystem, with an open protocol, where _individuals_ could develop their own components (be it software, hardware, add-ons, etc) where interoperability was maximised. This just simply was not possible.

Reticulum is also in the mix here. It is _far_ more ambitious than MeshCore, and can do a lot more, but it does suffer from a number of issues, like it being almost entirely Python-based, and not designed for microcontrollers. It does include this via things like the RNode firmware, but it's still very heavy compared to MeshCore.

**How does MeshCore try to answer these issues?**

So, in light of these background limitations, I went about trying to design something new, and these were the emerging principles:
* Light weight. Can run on tiny hardware.
* LoRa-first design, but not limited to LoRa. (any packet radio) ie. frugal packet sizes, very conservative with transmit air time.
* Portable core C++ library, not restricted to Arduino. Extensible. Many execution environments.
* Privacy as a core principle. (no mandatory identity exposure, encryption by default)
* Decentralised. No one central admin body/corporation who sets policy or permissions.
* Non-monolithic firmware. Firmwares are role-specific, they do a specific thing. No more, no less.
* An evolving protocol, extensible, but with features like messaging as core, being interoperable with as many firmwares as possible. Adaptable to things like sensor networks, for instance. Or even completely private systems with custom protocols on top of the core protocol.

**The Division of Labour, Rights and Responsibilities**

Like Reticulum, MeshCore clearly shifts the responsibility of packet routing to repeaters, and repeaters only. Edge nodes, like chat clients or sensor nodes, don't pollute the airwaves. Repeaters are mounted typically in good, elevated locations, and have a _fixed location_. Edge nodes can roam (* however, protocol extensions to aid/optimise roaming nodes is still being developed).

As repeaters do 'the work' of the mesh, it is the right of a repeater admin to refuse or allow traffic. This might be a little controversial, or could be seen as an avenue for discrimination by repeater admins, but the privacy built into the core protocol intentionally makes this difficult. Currently, there are no restrictions imposed in the standard repeater firmware, but there could be many repeater firmwares, some with more admin controls, with things like rate-limiting, custom priority boosts (or penalties) for packet types, or patterns of usage observed.

A more _organic_ strategy to weed-out bad nodes is, I believe, a better approach vs top-down policy dictates. With more options, choice of firmwares, individual areas/meshes will find their own solutions to things like channel saturation, etc. It's unlikely there is one solution for every mesh, in every jurisdiction. (However, good/sensible defaults go a long way to limiting the chaos)

**Paths**

The general strategy is: 'direct paths when possible, flood as fallback'. Every packet goes in either Flood mode or Direct mode. To stay frugal, the path discovery piggy-backs onto normal packets, like messaging. So, when A doesn't have a path to B, sending a message goes Flood mode, and if B is found, they get the message + path in one. In the context of messaging, the Ack + path is then returned to A.

Paths are asymmetrical. B also has to discover a path to A, and the Ack example also does a two-for one, in returning the Ack + out path, it also results in A receiving: Ack, out path, and return path. The only packet overhead in this exchange is a 3rd, direct-only packet that sends return path back to B. After this sequence, both A and B store direct paths (to the other) in their storage.

One side-effect of this, is that senders can store paths and actually choose which one to use. Liam's app actually takes this a step further and lets you manually construct paths yourself. If you are admin to a set of repeaters and know a particular route is optimal, this can be beneficial, and result in less flooding.

**Flood will become 'Expensive'**

The future scaling of MeshCore is still very much a question mark. I am quietly optimistic that it can grow, but I think it's safe to say that Flood mode will become what I call 'expensive'. The most basic form if this will be time. It will just take longer for Flood to reach a destination. (this is actually the current behaviour--repeater firmware gives lower and lower priority to flood packets that are from distant senders). To most users and use cases, this will be enough to look for better options.

Using a reasonably-local Room Server is one such option. These are intended for highly-local groups, as a simple bulletin board system. (No, they're not meant to scale to the whole mesh!) But, they generally try to insist on Direct-mode traffic, so keep the traffic local.

**Reward and Motivation**

I also think it's a fundamental right of a developer to be financially rewarded for their work. Vague promises of 'rewarding contributors', from some opaque corporate structure, is bullshit, in my opinion. This is why I strongly recommend the 'Freemium' model. It's direct reward to the person who put in the work. It also means the development is far more sustainable, and that a highly polished experience is the norm--instead of floundering in forum hell for weeks on end.

[Liam Cottle's](https://liamcottle.com/) amazing work with the native Android and iOS app is a great example of this. He has put in a lot of work, and it really should be rewarded. Also, if you place it in light of the alternative--forum hell and wrestling with unknown config and tools--isn't a few dollars worth being free of that!? :-)

It is highly rewarding getting direct praise and financial support from the people using your products. It's a tough road, though! It's extremely hard to make a living like this.

There is also potential for partnerships with hardware manufacturers. This gets a lot more murky, though. It is beneficial to potentially work on consumer-like bundles that are highly optimised or polished for a particular piece of hardware, and if these do eventuate, it will be up to the participants to work out an equitable share of the spoils. Anything consumer-focussed is extremely hard, and it takes a lot of QA and polish to get things like this to market. I hope MeshCore-compatible consumer products do eventuate, but it will be a difficult path.

**Future Stuff**

So, to wrap up, just some notes on future directions:
* Repeater admin and Room server interaction coming to Liam's apps.
* Hopefully more diagnostic tools for network planners, like Trace packets with SNR.
* Generally broadening device support: efforts to refine the T1000e support, more T1114 roles, etc.
* Efforts to standardise telemetry of sensor data.