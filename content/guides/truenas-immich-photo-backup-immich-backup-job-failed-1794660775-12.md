---
title: "Immich Backup Job Failed: Thumbnails Stuck Generating on TrueNAS"
description: "Troubleshoot an Immich backup job failed state on TrueNAS where new assets upload but thumbnail generation never finishes, with checks, a verification test, and recovery steps."
seo_title: "Immich Backup Job Failed: Fix Thumbnails Stuck Generating"
seo_description: "Troubleshoot an Immich backup job failed state on TrueNAS where new assets upload but thumbnail generation never finishes, with checks, a verification test, and recovery steps."
keywords:
  - "immich backup job failed"
date: 2026-11-17T11:09:05+08:00
publishDate: 2026-11-17T11:09:05+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

## What you are seeing

You uploaded new photos from your phone to Immich on TrueNAS, the assets appear in the timeline, but the thumbnail generation stays stuck and never completes.

## Check the job queue first

Open the Immich web UI and go to the administration section, then the jobs page. Look at the thumbnail generation queue. If the waiting count keeps growing while the active count sits at zero or one, the worker is not draining. Note the exact numbers before you change anything.

Next, confirm the container is healthy. On the TrueNAS shell, run `docker ps` and check that the Immich server and machine learning containers show a running status and a recent uptime. A container that restarted minutes ago suggests a crash loop rather than a slow queue.

## Check storage and permissions

Thumbnails are written to the upload location inside your Immich dataset. From the TrueNAS shell, check free space with `df -h` on the dataset mount. If usage is near full, generation will stall silently. Also verify the dataset is not read-only and that the container user still owns the upload folder.

## Verification test

Upload one small photo from your phone. Watch the jobs page for two minutes. If the waiting count drops by one and a thumbnail appears, the queue is working and the earlier stall was transient. If the count does not move, the worker is blocked and needs a restart.

## Recovery and rollback

Restart the Immich server container from the TrueNAS apps screen, then recheck the queue. If the queue still does not drain, stop the container, confirm your most recent dataset snapshot exists, and restore that snapshot before retrying. This returns you to a known state if a partial write caused the **immich backup job failed** condition.

If the queue remains stuck after a restart and snapshot check,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your queue counts and container logs.
