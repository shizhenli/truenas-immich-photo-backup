---
title: "Immich Server Not Reachable from Phone: Fix LAN Access When Web UI Works"
description: "Phone can't reach Immich on TrueNAS even though the web UI works on LAN? Diagnose the immich server not reachable phone issue with exact checks and a verification test."
seo_title: "Immich Server Not Reachable Phone: TrueNAS LAN Fix"
seo_description: "Phone can't reach Immich on TrueNAS even though the web UI works on LAN? Diagnose the immich server not reachable phone issue with exact checks and a verification test."
keywords:
  - "immich server not reachable phone"
date: 2026-11-14T19:50:04+08:00
publishDate: 2026-11-14T19:50:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS and the web UI opens fine on your computer, but the phone app says the server is unreachable.

## Confirm the phone is on the same network

On the phone, open Wi-Fi settings and note the IP address. Compare it with the TrueNAS host IP. If the phone is on cellular or a guest VLAN, the LAN address will not respond. Result: different subnet or guest network means the phone cannot route to Immich. Switch the phone to the same trusted LAN SSID and retry.

## Check the exact server URL in the app

The Immich app needs the full address, including the port. On TrueNAS, the default is usually http:// followed by the host IP and :2283. Open the app's server settings and verify the URL character by character. Result: a missing port, https instead of http, or an old DHCP address will fail immediately. Correct the URL and save.

## Test reachability from the phone browser

Open the phone browser and enter the same URL. If the login page loads, the network path is fine and the app configuration is the problem. If it times out, the TrueNAS firewall or app port binding is blocking the phone. Check that the Immich app is running and the port is published on the LAN interface.

## Verification test

After correcting the URL or network, force-close the Immich app, reopen it, and sign in. A successful login and thumbnail load confirms the fix. If it still fails, note the exact error text before changing anything else.

## Rollback note

If a firewall or app setting change makes the web UI unreachable too, revert that change immediately and confirm LAN access from the computer before retrying the phone.

If the **immich server not reachable phone** problem persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your TrueNAS version and app URL.
