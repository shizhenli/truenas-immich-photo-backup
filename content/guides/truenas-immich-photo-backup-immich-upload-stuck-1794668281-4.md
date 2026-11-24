---
title: "Immich Upload Stuck at the First Batch: Diagnose the Mobile Backup Stall"
description: "Troubleshoot an Immich mobile backup that starts but stops after the first batch on TrueNAS. Check server logs, phone state, and network to restore reliable uploads."
seo_title: "Immich Upload Stuck: Fix Mobile Backup Stopping After First Batch"
seo_description: "Troubleshoot an Immich mobile backup that starts but stops after the first batch on TrueNAS. Check server logs, phone state, and network to restore reliable uploads."
keywords:
  - "immich upload stuck"
date: 2026-11-24T08:09:22+08:00
publishDate: 2026-11-24T08:09:22+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich upload stuck**. Your phone says backup started, but the photos stop after the first batch and never resume.

## Check the server-side upload log

On the TrueNAS host, open the Immich app logs and filter for the upload endpoint. Look for repeated 4xx or 5xx responses at the moment the batch stops. A 413 means the batch size exceeds the proxy limit; a 401 or 403 points to an expired token or permission issue; a 500 suggests the server rejected the write. If the log shows no incoming requests after the first batch, the phone is not retrying, so move to the phone checks.

## Check the phone's backup state

In the Immich mobile app, open the backup screen and note whether the progress bar advances or freezes. Then force-stop the app, reopen it, and start backup again. If the first batch uploads and the stall repeats, the issue is consistent, not a one-off network drop. Also confirm the phone is not on a metered connection and that background app refresh is allowed.

## Verification test

Create a new album on the phone with five small photos and back up only that album. If those five upload and the counter reaches five, the server and credentials are fine and the original batch was likely too large or contained a file the server rejected. If the test also stops after the first item, the problem is server-side.

## Rollback and recovery

If a recent Immich update or proxy change preceded the stall, revert that change and restart the app. Before retrying, clear the app's backup queue rather than deleting uploaded assets.

If these checks do not isolate the cause,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and phone model.
