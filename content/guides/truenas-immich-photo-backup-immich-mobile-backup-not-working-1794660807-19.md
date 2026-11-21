---
title: "Immich Mobile Backup Stops After the First Batch on TrueNAS: A Practical Fix"
description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check permissions, storage, and logs, then verify with a small test album."
seo_title: "Immich Mobile Backup Not Working: Fix First-Batch Stall"
seo_description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check permissions, storage, and logs, then verify with a small test album."
keywords:
  - "immich mobile backup not working"
date: 2026-11-21T16:20:36+08:00
publishDate: 2026-11-21T16:20:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch of photos the upload simply stops, leaving the rest waiting.

## Check the obvious first
Open the Immich mobile app and confirm the server URL is reachable. On the TrueNAS host, verify the Immich dataset has free space: `df -h /mnt/your-pool/immich`. If usage is above 95%, the upload will stall. Also confirm the phone is on the same LAN and not using a VPN that blocks local traffic.

## Inspect permissions on the upload folder
SSH into TrueNAS and check ownership of the upload directory: `ls -ld /mnt/your-pool/immich/upload`. The user running Immich (often `apps` or `docker`) must own it. If it shows `root:root`, correct it with `chown -R apps:apps /mnt/your-pool/immich/upload`. Then restart the Immich container. This is the most common cause of an **immich mobile backup not working** after the first batch.

## Read the server log for the exact error
Run `docker logs immich-server --tail 100` right after a failed batch. Look for `ENOSPC` (disk full), `EACCES` (permission denied), or `ETIMEDOUT` (network). Each points to a different fix: free space, fix ownership, or check firewall rules.

## Verification test
Create a small album on your phone with five new photos. Trigger backup and watch the server log. If all five upload and the app shows them as backed up, the fix worked. If it stalls again at the same point, note the log line and repeat the permission check.

## Rollback note
If you changed ownership and Immich fails to start, revert with `chown -R root:root /mnt/your-pool/immich/upload` and restart. Then consult the Immich documentation for the correct user for your setup.

If the log shows repeated timeouts despite correct permissions,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exact error line.
