---
title: "Immich Server Not Reachable from Phone: Fix the Mobile App When Web UI Works"
description: "Your phone can't reach Immich even though the web UI works on LAN? Follow these checks to diagnose and fix the immich server not reachable phone issue on TrueNAS."
seo_title: "Immich Server Not Reachable from Phone: Fix Mobile App When Web UI Works"
seo_description: "Your phone can't reach Immich even though the web UI works on LAN? Follow these checks to diagnose and fix the immich server not reachable phone issue on TrueNAS."
keywords:
  - "immich server not reachable phone"
date: 2026-11-17T09:29:52+08:00
publishDate: 2026-11-17T09:29:52+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS, the web UI loads fine on your computer, but the Immich mobile app says the server is unreachable. This guide helps you diagnose that specific failure.

## Check the phone's network and server address

First, confirm your phone is on the same LAN as the TrueNAS host. Open the phone's Wi-Fi settings and verify the IP address is in the same subnet (e.g., 192.168.1.x). Next, in the Immich app, go to the server URL setting. It must use the TrueNAS host's LAN IP and the correct port (usually 2283), like http://192.168.1.50:2283. If you used a hostname, replace it with the IP. A wrong URL is the most common cause of the **immich server not reachable phone** error.

## Test basic connectivity

From the phone's browser, navigate to http://<truenas-ip>:2283. If the Immich login page appears, the network path is open, and the issue is app-specific. If it times out, the problem is network or firewall. Check that TrueNAS firewall rules allow port 2283 from your LAN. Also verify the Immich container is running and bound to 0.0.0.0, not just localhost.

## Verify with a ping and port check

Install a network tool app on your phone and ping the TrueNAS IP. A reply confirms basic reachability. Then use a port scanner to check if port 2283 is open. If the port is closed, review your TrueNAS app settings or firewall. After fixing, force-close and reopen the Immich app, then retry.

## Rollback and recovery

If you changed the server URL or network settings, revert to the last working configuration. If the app still fails, clear the app's cache or reinstall it, then re-enter the server address. For persistent issues,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) from our team.

## Verification test

Open the Immich app and trigger a small backup. If photos upload without errors, the connection is restored.
