---
title: "Immich Backup Job Failed: Clear the Stuck Thumbnail Queue on TrueNAS"
description: "New photos appear but thumbnails never finish generating on TrueNAS. Diagnose the stuck Immich thumbnail queue, clear the failed job, and verify recovery."
seo_title: "Immich Backup Job Failed: Fix Stuck Thumbnail Generation on TrueNAS"
seo_description: "New photos appear but thumbnails never finish generating on TrueNAS. Diagnose the stuck Immich thumbnail queue, clear the failed job, and verify recovery."
keywords:
  - "immich backup job failed"
date: 2026-11-24T11:09:31+08:00
publishDate: 2026-11-24T11:09:31+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added photos from your phone, the assets show up, but thumbnail generation stays stuck and never finishes.

## Check the job queue first

Open the Immich web UI and go to Administration > Jobs. Look at the Thumbnail Generation job. A healthy job shows a recent completion time and a decreasing queue. If it shows a growing queue with no progress, or a failed status, the worker is stalled. Note the exact queue count and the last completed timestamp.

## Inspect the container logs

On your TrueNAS host, open the Immich app logs and filter for the thumbnail worker. Look for repeated errors such as out-of-memory kills, permission denied on the upload or thumbs folders, or database connection timeouts. If you see OOM messages, the worker is being killed before it can finish. If you see permission errors, the dataset mount is not writable by the Immich user.

## Clear and restart the stalled job

In Administration > Jobs, stop the thumbnail job if it is running, then clear the failed job from the queue. Restart the Immich app from the TrueNAS Apps screen. This forces a fresh worker process and re-queues pending thumbnails.

## Verification test

Upload one new photo from your phone. Watch the Thumbnail Generation queue in Administration > Jobs. The queue should increment by one and then return to its previous count within a minute, and the new asset should show a thumbnail in the main timeline. If the queue still does not move, the worker is failing again and you have a deeper resource or permission issue.

## Rollback note

If clearing the job makes things worse, stop the app and restore the previous app configuration from your TrueNAS snapshot before restarting. This returns you to the last known state.

If the queue still will not drain after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and queue counts so the **immich backup job failed** state can be diagnosed precisely.
