---
layout: post
date: 2026-03-06 12:00:00 +1200
title: "Path Diagnostics Improvements"
description: "With the v1.14.0 firmware releases comes support for what is known as 'multibyte path hashes'"
image: "assets/images/2026/03/06/032259_1772767377598_meshgraph.png"
author: scottpowell
pinned: true
---
With the v1.14.0 firmware releases comes support for what is known as 'multibyte path hashes'. The single byte prefixes in the paths, and the inevitable duplications that exist out in the wild, have been a source of annoyance for many months now, so this release will hopefully give some people relief/hope.

![](/assets/images/2026/03/06/meshgraph.png)

**What is it, and how does it work?**

Origin/sender nodes can now specify whether a flood packet should use 1, 2 or 3 byte path hashes (ie. the pubkey prefixes). Repeaters that are on v1.14.0 or later will then append their own ID/hash with the given hash size before forwarding. So, this scheme is completely backward compatible. Origin nodes can still send packets in the 1-byte path hash size, and these will propagate as normal.

When a node receives a path, and then uses it in direct mode, the whole path can be in 1, 2 or 3 byte encoding. Each sending node can handle having a mix of contacts/other nodes, with paths to each being in any of the above encodings. So, a gradual move over to these encodings is supported.

However, it will take sometime until enough repeaters have updated to v1.14.0, and if an older repeater receives a packet with path hash size 2 or 3, it will simply discard the packet.

**Ripple UI Changes**

The main change comes in the **Optimise** screen:

![](/assets/images/2026/03/06/newoptimise.png)

By default, the new **Path hash size** preference is **1**. This is the legacy setting. You can set this to 1, 2 or 3. So, every flood mode packet your device sends (eg. adverts, messages, telemetry requests, etc) will use this mode/size. If the destination node is successfully reached, and path returned, it will use this path encoding, and stored for later direct mode use.

When you go into the **Message Details** screens, from group chats for example, the message path details will show the path hops with the finer grained (ie. _wider_) path hashes, so the node names that these resolve to will be much more accurate.

Same with the **Node Details** screens (from **Discover**). The advert path details screen will have the more accurate path/hops break down.

Also, the map on the home screen will show path vector lines with more accuracy.

NOTE: These changes will be in the **v9.2 release**, which will be within the next day or so.

**Android/iOS App Changes**

Liam has [released v1.41.0](https://meshcore.nz/) which now supports the multibyte path hashes. For now, the global setting is in the **Experimental Settings**. When more/most repeaters support this, it will be moved to the main Settings screen:

![](/assets/images/2026/03/06/Screenshot_20260306_173818_MeshCore.png)

You can also set the path hash mode/size _per contact_ when manually setting paths:

![](/assets/images/2026/03/06/Screenshot_20260306_at_3.32.00pm.png)

Also, everywhere that a path is displayed, the hash size is taken into account, like in the **RxLog** you can see these:

![](/assets/images/2026/03/06/0Screenshot_20260224_at_2.29.53_AM.png)

**Repeater/Room/Sensor CLI Changes**

Repeaters will mainly just forward in whichever mode the packet arrives in, but there are a few ways repeaters, room servers and sensors can be the _origin_ node, and for these cases there is a new preference which is set via CLI command:

```
set path.hash.mode {0,1,2}
```

**NOTE:** the 'mode' is the low-level encoding (0..3) where 0 -> 1-byte (legacy), 1 -> 2-byte, 2 -> 3-byte. The **mode 3 is currently reserved** for future use. 4-byte path encoding is not necessarily what this will be used for. 3-byte should be more than enough, and the 'mode 3' could be some entirely novel/different scheme. (as yet not formulated)

So, for repeaters, room server and sensors, flood adverts will use this preference.

For room servers, this preference is also used for when the room sends new/unsynced messages.

For sensor nodes, this preference is also used when sending alert messages to registered nodes in the ACL.

**EXTRA: Loop Detection**

With the v1.14.0 repeater release also comes a new _loop detection_ feature. From the CLI you can now get/set the new **loop.detect** preference. It can be set to one of: **off, minimal, moderate,** or **strict**.

Default is **off**.

When it is enabled, repeaters will now reject flood packets which look like they are in a loop. This has been happening recently in some meshes when there is just a single 'bad' repeater firmware out there (prob some forked or custom firmware). If the payload is messed with, then forwarded, the same packet ends up causing a packet storm, repeated up to the max 64 hops.

The loop detection should mitigate this fairly effectively.

The new loop detection logic uses the preference (min, mod, strict) + the path hash size with a lookup table, to see what the _maximum_ number of ID/hash duplicates are allowed before the packet will be rejected: (please forgive the code representation)

![](/assets/images/2026/03/06/loopdetect.png)

So, if preference is loop.detect **minimal**, and a 1-byte path size packet is received, the repeater will see if its own ID/hash is already in the path. If it's already encoded **4** times, it will reject the packet.  If the packet uses 2-byte path size, and repeater's own ID/hash is already encoded **2** times, it rejects.

So, this new feature can potentially be used _before_ the multibyte path hash support has gained wide spread support. (ie. with the 'minimal' setting, it can reject 1-byte loops)
