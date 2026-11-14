---
title: "Immich Mobile Backup Stops After First Batch: Diagnose the Stall"
description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check upload logs, app permissions, and server storage, then verify with a test album."
seo_title: "Immich Mobile Backup Not Working: Fix First-Batch Stall"
seo_description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check upload logs, app permissions, and server storage, then verify with a test album."
keywords:
  - "immich mobile backup not working"
date: 2026-11-14T19:50:04+08:00
publishDate: 2026-11-14T19:50:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You start a phone backup in Immich, the first batch uploads, and then the queue freezes while the app still says backup is running.

## Confirm where the first batch actually ended
Open Immich on the phone and check the backup status screen. Note the last uploaded asset and its timestamp. Then open the TrueNAS Immich web UI and compare the most recent asset from that device. If the phone shows more uploaded than the server received, the stall is in transfer. If counts match, the app is failing to queue the next batch.

## Check the mobile app's background and network state
On the phone, confirm Immich has photo library access set to all photos, not limited. Disable battery optimization for Immich, then switch from Wi-Fi to cellular or vice versa and watch whether the next batch starts. A batch that only moves after a network change points to a stale connection or DNS timeout on the TrueNAS side.

## Inspect server storage and logs
On TrueNAS, verify the Immich dataset has free space and that the pool is not full. Check the Immich server logs for upload errors around the time the first batch finished; repeated timeouts or permission errors mean the server is rejecting later requests. If you recently changed dataset permissions, restore the previous ACL before retrying. This is the most common reason for **immich mobile backup not working** after an initial success.

## Verification test
Create a small test album with five new photos on the phone and run backup only for that album. If all five upload and the counter advances, the queue is healthy. If it stops at the first image again, capture the timestamp and recheck the server logs.

If the stall persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and dataset details.
