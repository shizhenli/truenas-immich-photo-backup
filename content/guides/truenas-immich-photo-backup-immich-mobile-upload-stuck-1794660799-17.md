---
title: "Immich Mobile Upload Stuck on the First Batch: A Technician's Checklist"
description: "Phone says backup started but photos stop after the first batch? Work through these checks to find why Immich mobile upload is stuck on TrueNAS."
seo_title: "Fix Immich Mobile Upload Stuck on the First Batch (TrueNAS)"
seo_description: "Phone says backup started but photos stop after the first batch? Work through these checks to find why Immich mobile upload is stuck on TrueNAS."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-18T11:19:36+08:00
publishDate: 2026-11-18T11:19:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone reports backup started, but the progress bar freezes after the first batch and no new photos appear on the TrueNAS server.

## Check the mobile job state first
Open Immich on the phone and go to the backup screen. If it still shows "uploading" with a nonzero count, note the exact number. Then force-stop the app and reopen it. If the count resets to zero but no new assets appear on the server, the first batch is failing silently rather than pausing. If the count stays frozen, the app is waiting on a network response.

## Confirm the server sees the batch
On TrueNAS, check the Immich app logs for upload requests around the time of the stall. A completed batch should show successful upload entries followed by a new job. If you see repeated retries or timeouts on the same asset, the connection is dropping mid-transfer. Verify the phone and TrueNAS are on the same LAN and that the Immich URL uses the LAN address, not a hostname that resolves externally.

## Rule out storage and permissions
Check free space on the dataset backing Immich. A dataset that is nearly full will accept the first small batch and then reject larger writes. Also confirm the Immich user still owns the upload and library folders; a permission change after an update can cause later writes to fail while reads keep working.

## Verification test
Create one new photo on the phone, then start backup manually. Watch the server logs for that single asset. If it uploads and appears in the timeline, the batch path is healthy. If it fails, the issue is connection or storage, not the app.

## Recovery note
If you changed permissions or paths, restore the previous ownership before retrying, then restart the Immich app. This avoids leaving partial uploads that block the queue.

If the **immich mobile upload stuck** behavior persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your logs and dataset details.
