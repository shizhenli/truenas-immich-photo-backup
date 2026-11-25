---
title: "Immich Mobile Backup Stops After First Batch: Fix Background Upload"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, battery optimization, and server logs to restore reliable photo backup."
seo_title: "Immich Mobile App Background Upload Stops After First Batch"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check background upload settings, battery optimization, and server logs to restore reliable photo backup."
keywords:
  - "immich mobile app background upload"
date: 2026-11-25T08:09:22+08:00
publishDate: 2026-11-25T08:09:22+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone shows backup started, but after the first batch of photos, the **immich mobile app background upload** silently stops, leaving the rest of your library unsynced.

## Check background upload permissions

Open your phone's app settings for Immich and confirm background activity is allowed. On Android, go to Settings > Apps > Immich > Battery and select Unrestricted. On iOS, enable Background App Refresh for Immich. If the setting is Optimized or Restricted, the OS will suspend the upload after the first batch. Set it to Unrestricted and retry.

## Verify server-side batch limits

On your TrueNAS host, check the Immich server logs for upload errors around the time the first batch completes. Look for messages about timeouts, memory, or connection resets. A common cause is a reverse proxy or firewall closing idle connections after the first batch. Confirm your proxy timeout is at least 300 seconds. If you see repeated 413 or 504 errors, adjust the proxy buffer size or timeout accordingly.

## Run a controlled verification test

Disconnect Wi-Fi, switch to cellular, and start a small backup of 10 new photos. Watch the Immich app's upload progress. If all 10 complete, the issue is likely Wi-Fi-specific (proxy or router). If it stops again after the first few, the problem is server-side. Note the exact photo count where it stops.

## Rollback and recovery

If you changed battery or proxy settings and the backup worsens, revert to the previous values. To recover unsynced photos, force-stop the Immich app, clear its cache (not data), and restart the backup. Avoid deleting the app, as that removes upload history.

If these steps do not resolve the stall,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your server logs and the exact batch size where uploads stop.
