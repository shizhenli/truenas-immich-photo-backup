---
title: "Immich Upload Stuck: Why Mobile Backup Stops After the First Batch on TrueNAS"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore expected upload behavior."
seo_title: "Immich Upload Stuck After First Batch on TrueNAS: Fix Mobile Backup Stall"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore expected upload behavior."
keywords:
  - "immich upload stuck"
date: 2026-11-21T16:20:36+08:00
publishDate: 2026-11-21T16:20:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You start a mobile backup on your phone, the first batch of photos uploads, and then the progress simply stops. This is a common **immich upload stuck** situation on TrueNAS, and it usually points to a server-side interruption rather than a phone problem.

## Check the Immich server logs first

On your TrueNAS host, open the Immich app logs and look at the last entries around the time the first batch finished. If you see repeated timeouts, connection resets, or permission errors on the upload directory, note the exact message. A clean log with no errors after the first batch suggests a network or client-side stall instead.

## Verify dataset permissions and free space

Confirm the Immich user has write access to the upload dataset. A dataset that is full or has incorrect ownership will accept the first small batch and then fail silently on larger files. Check available space and the dataset ACL. If permissions are wrong, correct them and restart the Immich app before retrying.

## Run a controlled verification test

On the phone, stop the current backup, clear the Immich app cache, and start a new backup of a small album only. Watch the server logs during the upload. If the small album completes, the original stall was likely a transient network drop or a full dataset. If it stalls again at the same point, the issue is server-side and needs deeper log review.

## Rollback and recovery note

If you changed permissions or dataset settings and the backup worsens, revert those changes before troubleshooting further. Keep a snapshot of the upload dataset so you can restore it if a permission change locks out Immich.

If the logs remain unclear after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exact log lines and your dataset configuration.
