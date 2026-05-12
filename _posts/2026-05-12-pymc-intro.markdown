---
layout: post
date: 2026-05-12 12:00:00 +1200
title: "pyMC Introduction"
description: "An intro to pyMC, the Python implementation of MeshCore."
image: "assets/images/2026/05/12/control-room.png"
author: rightup
---

![](/assets/images/2026/05/12/control-room.png)

What is pyMC? At its core, pyMC is a Python implementation of a MeshCore-compatible stack. It is designed to run on Linux-based systems and connect embedded LoRa radios to higher-level software services. It started as a way to better understand the MeshCore protocol. But pretty quickly, it became something more useful: a reusable core library for building flexible, Linux-powered mesh infrastructure. The `pymc core` handles the protocol-level work:

* Parsing and generating MeshCore packets
* Managing identities and addressing
* Maintaining compatibility as the protocol evolves
* Handling radio interfaces (SPI, KISS, USB-CH341)

with over 15 boards currently supported and many new ones in development, the system has grown from a simple experiment into a practical toolkit for building and running distributed mesh infrastructure.

On top of pyMC core sits `pymc repeater`, which handles the operational side:

* Processing packets
* Applying routing and filtering logic
* Forwarding traffic
* Supporting services like roomservers, companions and observer data output

Embedded firmware is efficient and well suited to dedicated devices, but it operates under tight resource constraints. Running pyMC on Linux expands the design space: more compute, better tooling, easier debugging, and deeper real-time visibility into mesh behaviour.

A key design choice is how identities are handled. Instead of assuming one physical node equals one logical presence, pyMC lets multiple identities exist on the same radio.

In practice, a single device can take on multiple roles. It might act as a repeater, represent different logical endpoints, or host additional services--each with its own context.

This flexibility allows a `pymc repeater` to evolve into a multi-companion base station. At that point, it behaves less like a simple packet repeater and more like a lightweight message hub for the mesh.

Put another way: pyMC gives MeshCore a Linux-native surface area. It can run repeaters, host services, power analytics, or support entirely new mesh applications--without losing compatibility with the underlying radio layer.

<img alt class="mx-auto" src="/assets/images/2026/05/12/image.pymc-map.webp" />
<img alt class="mx-auto" src="/assets/images/2026/05/12/image.pymc-stats.webp" />
<img alt class="mx-auto" src="/assets/images/2026/05/12/image.pymc-system-stats.webp" />

## Where pymc_console fits

One example of that larger design space is `pymc_console`: a browser-based dashboard built on top of pyMC repeater.

Where pyMC handles the protocol and repeater layer, `pymc_console` turns that activity into something you can actually see and reason about. Packet flow, radio status, connected identities, observer output, airtime utilization, and network behavior can all be surfaced in a more approachable interface.

That matters because a mesh network is difficult to improve if it is invisible. Once packets, routes, signal quality, and utilization patterns become observable, the repeater becomes more than a forwarding node. It becomes a local window into the health and behavior of the mesh.

In that sense, `pymc_console` is less a separate idea than a natural extension of the pyMC architecture. The same Linux-native foundation that lets pyMC talk to radios also makes it possible to build analytics, dashboards, companion experiences, and new tools around the mesh.

<img alt class="mx-auto" src="/assets/images/2026/05/12/image-console.webp" />
<img alt class="mx-auto" src="/assets/images/2026/05/12/image-console-map.webp" />
<img alt class="mx-auto" src="/assets/images/2026/05/12/image-console-compan.webp" />

The interesting part isn’t just what pyMC does today, but what it enables in practice.

If you’d like to explore it, run it yourself, or follow along as it evolves, everything you need to get started is available.

## Where to start

Source and setup instructions are on [https://github.com/pyMC-dev](https://github.com/pyMC-dev/pyMC_Repeater#pymc_repeater)

Or join us in the pyMC Discord [https://discord.gg/hRjW9ha6m](https://discord.gg/hRjW9ha6m)

## Recommended Gear

For a no-fuss pyMC setup, I recommend the following tested and compatible hardware platforms. The MeshToad and MeshTadpole are ideal for lightweight installs, home labs, and desktop usage, while PiMesh provides an excellent Raspberry Pi-based solution for infrastructure and gateway deployments. For repeater-focused installations, the UltraPeater Luckfox Pico Ultra HAT offers a compact and efficient dedicated repeater platform.

* MeshToad V3 — [https://muzi.works/products/nullhop-meshtoad-v3](https://muzi.works/products/nullhop-meshtoad-v3)
* MeshTadpole SX1262 USB Stick — [https://www.elecrow.com/meshtadpole-sx1262-usb-stick.html](https://www.elecrow.com/meshtadpole-sx1262-usb-stick.html)
* PiMesh — [https://meshsmith.net/](https://meshsmith.net/)
* UltraPeater — [https://zindello.com.au/ultrapeater/](https://zindello.com.au/ultrapeater/)

<img alt class="mx-auto" src="/assets/images/2026/05/12/pymc-gear.jpeg" />
