---
title: "Immich Mobile Backup Stops After First Batch: Fix Background Upload Stalls"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, verify with a test batch, and recover safely."
seo_title: "Immich Mobile App Background Upload Stops After First Batch"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, verify with a test batch, and recover safely."
keywords:
  - "immich mobile app background upload"
date: 2026-11-18T08:09:32+08:00
publishDate: 2026-11-18T08:09:32+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You started a backup on your phone, the first batch of photos uploaded, and then the **immich mobile app background upload** simply stopped, leaving the rest of your library untouched.

## Check background upload settings
Open the Immich app on your phone and go to Settings > Backup. Confirm that Background Backup is enabled and that the app has permission to run in the background (on Android, check battery optimization; on iOS, check Background App Refresh). If either is off, the app may upload only while open and stop after the first batch. Also verify that the backup album selection includes the photos you expect.

## Inspect the first batch result
In the Immich web UI on your TrueNAS host, open the backup album and note how many assets were uploaded. Then check the phone's backup status screen: if it shows a last upload time but no progress, the app likely paused due to a background restriction or a network drop. If the server shows fewer assets than the phone reports, check the TrueNAS dataset permissions for the upload user.

## Verification test
Disable battery optimization for Immich, keep the app open, and trigger a manual backup of a small new album. Watch the upload count increase beyond the first batch. If it completes, background upload is now working; if it stalls again, note the exact asset where it stops and check server logs for errors at that time.

## Rollback and recovery
If changes make things worse, revert the battery optimization setting and restart the Immich app. Your uploaded assets remain safe; you can re-enable backup once the test passes. For deeper diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
