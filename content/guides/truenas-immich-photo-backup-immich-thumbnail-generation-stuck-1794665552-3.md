---
title: "Immich Thumbnail Generation Stuck: Diagnose the Real Blocker"
description: "New photos appear in Immich but thumbnails never finish. Diagnose the thumbnail job, check logs, and recover safely on TrueNAS."
seo_title: "Immich Thumbnail Generation Stuck on TrueNAS: Fix It"
seo_description: "New photos appear in Immich but thumbnails never finish. Diagnose the thumbnail job, check logs, and recover safely on TrueNAS."
keywords:
  - "immich thumbnail generation stuck"
date: 2026-11-21T16:20:36+08:00
publishDate: 2026-11-21T16:20:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You upload new photos and they appear in Immich, but the thumbnail job never reaches completion, leaving blank previews across the library.

## Confirm the Job Is Actually Stalled

Open the Immich web UI as an administrator and go to Administration, then Jobs. Find the Thumbnail Generation job. If it shows active but the processed count has not moved after several minutes, the worker is stalled. Note the pending count. If it is zero while thumbnails are still missing, the queue lost the job and you should re-run it rather than wait.

## Check the Worker and Logs

On the TrueNAS host, inspect the Immich app logs from the Apps screen or the shell. Look for repeated errors around ffmpeg, sharp, or disk writes. A common cause is the thumbnail cache path being full or read-only. Check free space on the dataset holding Immich configuration and uploads. If the log repeats permission denied for the thumbnails folder, the container user cannot write there, which keeps the job in a loop.

## Run a Verification Test

Upload one new photo from your phone and watch the Thumbnail Generation job count. If the count increases and the preview appears within a minute, the queue is healthy. If it stays frozen, restart the Immich app from TrueNAS Apps, then re-run the thumbnail job from Administration, Jobs. This clears a wedged worker without touching your originals.

## Recovery Note

If you changed permissions or paths, revert them before restarting, and keep a configuration backup. Re-running thumbnail generation is safe and rebuilds previews only. If the job still will not progress after a restart and log review, the **immich thumbnail generation stuck** condition likely needs a deeper check of the database and worker container.

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your logs and job counts.
