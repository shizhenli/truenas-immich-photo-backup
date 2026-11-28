---
title: "Immich Mobile Backup Stops After the First Batch: A Controlled Diagnosis"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Follow these checks to diagnose and restore background upload."
seo_title: "Fix Immich Mobile Backup Stopping After First Batch"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Follow these checks to diagnose and restore background upload."
keywords:
  - "immich mobile app background upload"
date: 2026-11-28T22:25:08+08:00
publishDate: 2026-11-28T22:25:08+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

## Understand the stall

You started a backup on your phone, the first batch of photos uploaded, and then everything stopped even though the app still shows backup in progress. This points to a break in the **immich mobile app background upload** chain, often caused by the phone pausing the job or the server rejecting the next request.

## Check the server side first

On your TrueNAS host, confirm Immich is running and reachable from the phone's browser. Open the Immich web UI and check the job queue under Administration > Jobs. Look for a stuck or failed "Upload" job. If it shows failed, note the error. A common cause is the storage pool running out of space or the dataset becoming read-only. Check free space and dataset permissions; the Immich user must have write access. If the job is stuck in "active" with no progress, cancel it and retry.

## Check the phone side

On the phone, open Immich settings and confirm background upload is enabled and the correct album or folder is selected. Check that the phone is not in low-power mode and that Immich has background refresh allowed. Then force-close the app and reopen it. Start a small test: select one new photo and upload it manually. If that succeeds, the connection and permissions are fine, and the issue is likely the background job being suspended.

## Verification test

Add three new photos to the phone's library. With the app open and the screen on, trigger backup. Watch the Immich web UI job queue. If all three appear within two minutes, the pipeline works. If it stops again after one, the server is rejecting the batch.

## Rollback and recovery

If you changed permissions or settings, revert them to the last known working state before retrying. If the queue remains stuck, restart the Immich service on TrueNAS, then retry the test. For deeper diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
