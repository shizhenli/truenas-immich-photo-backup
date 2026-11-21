---
title: "Immich Mobile Upload Stuck at the First Batch: A Practical Fix"
description: "Troubleshoot an Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, app state, and network to restore uploads."
seo_title: "Fix Immich Mobile Upload Stuck at the First Batch on TrueNAS"
seo_description: "Troubleshoot an Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, app state, and network to restore uploads."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-21T16:26:40+08:00
publishDate: 2026-11-21T16:26:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch of photos, nothing else uploads to Immich on TrueNAS.

## Check the server-side upload log
On the TrueNAS host, open the Immich container logs and look for entries around the time the first batch finished. A healthy batch ends with completed upload messages and no repeated errors. If you see repeated timeouts, 500 responses, or permission errors, the server is rejecting or dropping the next batch. Note the exact error text; that tells you whether to look at storage, database, or network.

## Check the mobile app state
In the Immich app, open the backup screen and confirm the backup status. If it shows "uploading" but the count does not increase for several minutes, the app may be waiting on a stalled request. Force-stop the app, reopen it, and check whether the pending count changes. If it resumes briefly and stalls again at the same point, the issue is likely a single problematic file or a server-side limit, not the whole library.

## Verify with a small test
Create a new album on the phone with five small photos and enable backup only for that album. If those upload completely, the original stall is tied to a specific file or batch size. If they also stop after the first batch, the problem is server-side and affects all uploads.

## Recovery note
Before changing settings, export or note your current Immich backup configuration so you can restore it. If a test upload succeeds, re-enable the full library backup and monitor the first two batches. If the **immich mobile upload stuck** behavior returns, stop the backup and review the server log again with the new timestamps.

If the logs point to a configuration issue you cannot resolve,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exact error text and timestamps.
