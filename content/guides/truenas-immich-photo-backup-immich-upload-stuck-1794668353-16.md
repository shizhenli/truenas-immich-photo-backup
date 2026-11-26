---
title: "Immich Upload Stuck at the First Batch: A Practical Fix"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore uploads."
seo_title: "Immich Upload Stuck at First Batch on TrueNAS: Fix Mobile Backup"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore uploads."
keywords:
  - "immich upload stuck"
date: 2026-11-26T08:07:43+08:00
publishDate: 2026-11-26T08:07:43+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You start a mobile backup in Immich, the first batch uploads, and then the transfer halts without an error, leaving you with an **immich upload stuck** situation.

## Check the server logs first

On your TrueNAS host, open a shell and run `docker logs --tail 100 immich_server` (adjust the container name if needed). Look for lines around the time the upload stopped. If you see `EACCES` or `permission denied`, the issue is likely dataset permissions. If you see `connection reset` or timeouts, focus on the network.

## Verify dataset permissions

Immich writes to the upload location defined in your compose file, often `/mnt/tank/immich/upload`. Run `ls -ld` on that path. The owner and group should match the user ID the Immich container runs as (commonly 1000). If they differ, correct ownership with `chown -R 1000:1000` on the upload directory, then restart the container. A mismatch here often causes the first batch to succeed and subsequent writes to fail silently.

## Test network stability

From your phone, open the Immich web UI in a browser and try uploading a single photo manually. If that also stalls, the problem is connectivity, not the app. Ensure your phone and TrueNAS are on the same LAN and that no firewall rule blocks the Immich port after the initial handshake.

## Verification test

After making changes, force-stop the Immich app on your phone, clear its cache, and start a new backup of a small album (10–20 photos). Watch the server logs during the upload. You should see each file processed without errors. If the batch completes and the next begins, the fix is working.

## Rollback note

If permissions changes cause new errors, revert the ownership to the previous values and restart. Keep a note of the original `ls -ld` output before editing.

If the stall persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpts and permission output.
