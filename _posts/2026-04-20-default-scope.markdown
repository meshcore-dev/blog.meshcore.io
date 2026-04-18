---
layout: post
date: 2026-04-17 12:00:00 +1200
title: "Default Scope Region"
description: "Hopefully the last of the missing pieces for region filtering is now here."
image: "assets/images/2026/04/17/banner.png"
author: scottpowell
---
![](/assets/images/2026/04/17/banner.png)

_NOTE: Refer to the [previous blog post](/2026/01/20/region-filtering) if you are unfamiliar with regions/scopes_

Since the v1.12.0 firmware release, scoping _group channel_ messages has been possible, but there are still a number of road-blocks to using regions more effectively, especially for meshes like in Germany who are trying to **disable un-scoped** traffic (ie. `region denyf *`)

In the next release (v1.15.0) there will be a new concept of 'default scope', for both companion nodes and repeater/room server. This will make it possible to do scoped DM messages, login requests, stats request, etc. (even in a mesh that has disabled un-scoped flood traffic)

## Mobile App

The [MeshCore app](https://meshcore.io/#download) ver 1.43.0 will have a new setting in the **Experimental Settings** screen. Here you can set the default scope region name.

<img alt class="mx-auto" src="/assets/images/2026/04/17/app-default-scope.jpg" />

When set, ALL flood packets the companion sends (eg. adverts, DM's/logins/requests when path is unknown) will be **scoped** to this region.

Group channel scope, if set, will _override_ the default scope. So, you can still have (typically) smaller region/scope for individual channels.

## Repeater and Room Server Admin

From v1.15.0 there will be a new cli command:

`region default {name | <null>}`

This sets the given region as the default scope, or clears if you specify `<null>`. Note that the region is auto-created if currently not in the regions list. Also, an implicit `region save` is automatically performed!

Default scope is only applied to packets which _originate_ from the repeater/room server. So, only adverts for repeater. Adverts and room posts for room server.

**Logins/Requests/Commands and Responses**

From v1.15.0 there is a new rule for _responses_. These originate from repeater/room server, but have this rule:
* If request packet scope can be resolved (ie. Region is in its list) then same scope is used for reply
* Otherwise, reply is sent un-scoped.

This includes the case of un-scoped request will have an un-scoped reply.

For now, this is the _safer_ option, instead of having the replies default to the default-scope. While in this transition period, there are too many ways a repeater admin could shoot themselves in the foot and lock themselves out of their own repeater!

**EXTRA: new 'put' default**

The `region put ...` CLI command now defaults the new region to 'allow' for flood. So, you won't need to also follow with `region allowf ...`

## Standalone Devices

For Ripple GUI supported devices, like the T-Deck, the next release (v9.6) will have default-scope support. From the **Regions** screen, just select a plain region (or '*') and in the details screen, select the menu:

<img alt class="mx-auto" src="/assets/images/2026/04/17/region-details-default-scope.png" />

(If needed, create a new region from the main region list, for the default scope)

Back in the Regions list screen, you should see a `(S)` displayed next to the current default scope region:

<img alt class="mx-auto" src="/assets/images/2026/04/17/region-list-default.png" />

## FAQ

**Q: As a repeater admin, what should I set default scope to?**

It should typically be a _large_ region, eg. your city. Or, whatever the region that you wish to confine adverts to (for instance).

**Q: As an app/chat user, what should I set default scope to?**

This should be a _large_ region which will safely include both yourself and the contact(s) you wish to DM. Both your messages, and the returning ACKs will be scoped to this region.

**Q: I manage regional MeshCore firmware builds, with best defaults for the region. Can I configure a custom default region in my builds?**

Yes. In the relevant .ini files, just add to build_flags `-D DEFAULT_FLOOD_SCOPE_NAME='" .. "'` (with the name in quotes)
