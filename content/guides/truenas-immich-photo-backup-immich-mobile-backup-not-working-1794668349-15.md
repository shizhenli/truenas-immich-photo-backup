---
title: "Why Immich Mobile Backup Stalls After the First Batch on TrueNAS"
description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check server reachability, dataset permissions, and app state, then verify with a test upload."
seo_title: "Immich Mobile Backup Not Working: Fix First-Batch Stall"
seo_description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check server reachability, dataset permissions, and app state, then verify with a test upload."
keywords:
  - "immich mobile backup not working"
date: 2026-11-25T12:09:21+08:00
publishDate: 2026-11-25T12:09:21+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You start a backup on your phone, the first few photos upload, and then the progress bar freezes while the app still says backup is running. This is a common case of **immich mobile backup not working**, and it usually points to a single blocked step rather than a broken library.

## Confirm the server is reachable from the phone
Open the Immich web UI on a computer on the same network and note the exact address and port. On the phone, switch off mobile data and connect only to Wi-Fi. In the Immich app, go to settings and check the server URL. If the app cannot load the timeline, the phone is not reaching the server, and the first batch may have succeeded only because it was queued locally. A reachable server should show your existing photos within a few seconds.

## Check the upload dataset permissions
On TrueNAS, open the Immich app and find the host path mapped to the upload location. Confirm the user running the Immich container has write access to that dataset. A quick check is to create a test file from the app's shell inside the upload folder. If that fails, the first batch filled a buffer and later writes are being denied. Correct the dataset permissions or ownership, then restart the Immich app so the change takes effect.

## Clear the stalled mobile queue
In the Immich app, stop the backup, then force-close the app and reopen it. Start the backup again and watch the first batch. If it completes and moves to the next batch, the queue was the issue.

## Verification test
Take one new photo on the phone, wait for it to appear in the Immich web timeline, then take a second photo. Both should upload without stopping. If only the first appears, return to the permission check.

If the stall returns after a restart, revert any recent permission change before retrying, and

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exact point where the batch stops.
