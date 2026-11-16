---
title: "Immich Mobile Backup Stalls After the First Batch: Diagnose the Queue"
description: "Fix Immich mobile backup that stops after the first batch on TrueNAS by checking the upload queue, server logs, and app state, then verifying with a controlled test."
seo_title: "Immich Mobile Upload Stuck After First Batch on TrueNAS"
seo_description: "Fix Immich mobile backup that stops after the first batch on TrueNAS by checking the upload queue, server logs, and app state, then verifying with a controlled test."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-16T10:09:04+08:00
publishDate: 2026-11-16T10:09:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch the count stops moving and the remaining photos never leave the device.

## Confirm the stall is real
Open the Immich mobile app and note the number of pending uploads. Wait five minutes without touching the phone. If the pending count is unchanged and no new assets appear in the web UI, the queue is stalled rather than slow.

## Check the server-side queue
On the TrueNAS host, review the Immich server logs for repeated upload or job errors around the time the first batch finished. Look for messages about database writes, disk space, or job worker failures. If the logs show a repeating error, that is your blocker; if they show nothing after the first batch, the phone stopped sending requests.

## Reset the mobile upload state
In the app, pause backup, force-stop the app, and reopen it. Check that the server URL still resolves and that the account session is valid. Then resume backup and watch the pending count for two minutes. A resumed count means the first batch left the queue in a bad state; no movement means the connection or session is failing silently.

## Verification test
Take one new photo and trigger a manual backup of that single asset. If it uploads, the pipeline works and the original batch needs to be re-queued by toggling backup off and on. If it does not upload, treat it as a connectivity or permission issue before retrying the full library.

## Recovery note
Before clearing app data or reinstalling, confirm your originals are still on the phone. Clearing the app removes its local queue state but not your photos, so you can safely retry once the server is confirmed healthy. If the **immich mobile upload stuck** behavior returns after these checks, stop retrying and gather the logs for review.

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the log excerpt and the exact point the batch stops.
