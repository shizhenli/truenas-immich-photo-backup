---
title: "Immich Mobile Backup Stalls After the First Batch: Fix Background Upload"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, battery optimization, and verify with a test batch."
seo_title: "Immich Mobile App Background Upload Stops After First Batch"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, battery optimization, and verify with a test batch."
keywords:
  - "immich mobile app background upload"
date: 2026-11-15T19:09:20+08:00
publishDate: 2026-11-15T19:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You started a backup on your phone, the first few photos uploaded, and then everything stopped—this guide helps you diagnose and fix that first-batch stall.

## Check background upload permissions

On Android, open Settings > Apps > Immich > Battery and set it to Unrestricted. On iOS, go to Settings > Immich and enable Background App Refresh. Then open Immich, go to Backup settings, and confirm that background backup is enabled. If it was off, turn it on and retry. If it was already on, toggle it off and back on to reset the scheduler.

## Verify the upload queue is not paused

In the Immich app, open the backup screen and check for a paused or pending status. If the queue shows paused, tap resume. Also confirm that the phone is on Wi-Fi and that the Immich server address is reachable—open the web UI from the phone browser. If the web UI loads but the app does not, the app may be using a stale server URL; re-enter it in the app settings.

## Test with a small batch

Create a new album with five photos and set it as the only backup source. Start backup and watch the progress. If all five upload, the issue is likely battery optimization killing the background task. If it stalls again, check the TrueNAS host for storage space and dataset permissions. Run `df -h` on the host; if the pool is full, free space. Then verify the upload user has write access to the dataset.

## Rollback and recovery

If you changed battery settings or server URL and the app behaves worse, revert those changes and restart the phone. For a failed batch, delete the partial upload from the Immich web UI and retry. If the problem persists,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your app logs and TrueNAS pool status. The **immich mobile app background upload** depends on both phone permissions and server reachability, so test each layer separately.
