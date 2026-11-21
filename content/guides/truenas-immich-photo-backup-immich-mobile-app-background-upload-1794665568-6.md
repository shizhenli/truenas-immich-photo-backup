---
title: "Immich Mobile Backup Stops After the First Batch: Fix Background Upload"
description: "Troubleshoot Immich mobile backup stalling after the first batch on TrueNAS. Check background upload settings, battery optimization, and server connectivity to restore full photo backup."
seo_title: "Immich Mobile Backup Stops After First Batch: Fix Background Upload"
seo_description: "Troubleshoot Immich mobile backup stalling after the first batch on TrueNAS. Check background upload settings, battery optimization, and server connectivity to restore full photo backup."
keywords:
  - "immich mobile app background upload"
date: 2026-11-21T16:23:39+08:00
publishDate: 2026-11-21T16:23:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You set up Immich on your TrueNAS host and started a backup from your phone, but the progress bar halts after the first batch of photos and never resumes.

## Check background upload permissions

Open your phone's settings and find the Immich app. Ensure background app refresh is enabled and that battery optimization is disabled for Immich. On Android, go to Settings > Apps > Immich > Battery and select Unrestricted. On iOS, go to Settings > Immich and enable Background App Refresh. If these are off, the **immich mobile app background upload** will stop once the screen locks or the app is backgrounded.

## Verify server connectivity and upload queue

On your TrueNAS host, check that the Immich server container is running and reachable. From your phone's browser, open the Immich web interface using the same URL configured in the app. If the web UI loads but the app stalls, the issue is likely the app's connection settings. In the Immich app, go to Settings > Backup and confirm the server URL is correct and the upload queue is not paused. Look for any error messages in the app's backup status screen.

## Test with a small batch

Create a new album on your phone with 5–10 photos. In Immich, trigger a manual backup for that album only. Watch the upload progress. If all photos upload successfully, the issue is with background execution for large batches. If the upload stops again after the first few, check the server logs for timeouts or permission errors.

## Rollback and recovery

If you changed battery or background settings, revert them one at a time to isolate the cause. If the backup still stalls, restart the Immich server container on TrueNAS and re-run the test. For persistent issues,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your server logs and app version.
