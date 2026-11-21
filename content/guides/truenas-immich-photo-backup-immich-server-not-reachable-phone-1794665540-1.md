---
title: "Immich Server Not Reachable from Phone: Diagnose the Mobile-Only Connection Failure"
description: "Your phone can't reach Immich even though the web UI works on LAN. Follow these steps to diagnose and fix the mobile-only connection failure on TrueNAS."
seo_title: "Immich Server Not Reachable from Phone: Fix Mobile Access When Web UI Works"
seo_description: "Your phone can't reach Immich even though the web UI works on LAN. Follow these steps to diagnose and fix the mobile-only connection failure on TrueNAS."
keywords:
  - "immich server not reachable phone"
date: 2026-11-21T16:20:36+08:00
publishDate: 2026-11-21T16:20:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You're running Immich on TrueNAS, the web UI loads fine on your computer, but your phone shows the **immich server not reachable phone** error. This is a common mobile-only connection issue, and we'll walk through it step by step.

## Check the server address on your phone
Open the Immich app and go to Settings > Server URL. Make sure it matches exactly what you use in the web browser—including http:// or https:// and the port number (usually 2283). If you used a hostname like truenas.local, try the TrueNAS IP address instead. A wrong address is the most frequent cause.

## Confirm your phone is on the same network
Verify your phone is connected to the same Wi-Fi network as the TrueNAS host. If you're on cellular or a guest network, the phone can't reach the LAN IP. Turn off mobile data temporarily and reconnect to Wi-Fi.

## Test basic connectivity from the phone
Open your phone's browser and enter the Immich URL. If the page loads, the app configuration is the problem. If it doesn't load, the network path is blocked. Check that TrueNAS firewall rules allow port 2283 from your phone's subnet.

## Verify the Immich service is listening
On TrueNAS, open a shell and run `sudo netstat -tlnp | grep 2283`. You should see the Immich process listening on 0.0.0.0:2283 or your LAN IP. If it only shows 127.0.0.1, the service is bound to localhost and won't accept phone connections—adjust the Immich configuration to bind to all interfaces.

## Verification test
After fixing the address or firewall, force-close the Immich app and reopen it. Try a manual backup. If photos upload, the issue is resolved.

## Rollback note
If changes break web access, revert the server URL or firewall rule to the previous working state. For deeper help,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
