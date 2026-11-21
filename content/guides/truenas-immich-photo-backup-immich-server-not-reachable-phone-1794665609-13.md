---
title: "Immich Server Not Reachable from Phone: Diagnose the LAN Block"
description: "Phone can't reach Immich on TrueNAS while the web UI works on LAN? Diagnose the mobile-only block with these exact checks and a verification test."
seo_title: "Immich Server Not Reachable from Phone on TrueNAS: LAN Fix"
seo_description: "Phone can't reach Immich on TrueNAS while the web UI works on LAN? Diagnose the mobile-only block with these exact checks and a verification test."
keywords:
  - "immich server not reachable phone"
date: 2026-11-21T16:26:40+08:00
publishDate: 2026-11-21T16:26:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on a TrueNAS host, the web UI loads fine on your LAN, but the phone app reports the server as unreachable.

## Confirm the phone is on the same LAN

On the phone, open Wi-Fi settings and note the IP address and subnet. Compare it with the TrueNAS host IP. If the phone is on a guest network, a mobile hotspot, or a different VLAN, the app cannot reach the server. Result interpretation: a different subnet or a guest SSID means isolation is the cause, not Immich.

## Check the Immich port and protocol

In the phone app, open the server URL setting. It should be http:// followed by the TrueNAS LAN IP and the Immich port, typically 2283. A trailing slash, a hostname that only resolves on the desktop, or https on a plain HTTP port will fail. Result interpretation: if the web UI uses a hostname, replace it with the raw IP in the app and retry.

## Test reachability from the phone browser

Open the phone browser and navigate to the same URL. If the login page loads, the network path is open and the app configuration is at fault. If it times out, the block is at the network or firewall layer. This is your verification test: the browser result tells you which side to fix.

## Rollback and recovery

Before changing firewall rules or TrueNAS network settings, record the current values. If a change makes the web UI unreachable too, restore the previous rule and confirm LAN access before retrying the phone.

If the browser test still fails after these checks, the issue is likely a firewall rule or app binding specific to the mobile path. For a controlled diagnosis of this **immich server not reachable phone** case,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
