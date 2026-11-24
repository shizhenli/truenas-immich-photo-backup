---
title: "Immich Thumbnail Generation Stuck on TrueNAS: Clear the Queue and Recover"
description: "New photos appear in Immich but thumbnails never finish on TrueNAS. Diagnose the stuck thumbnail queue, fix the worker, and recover without losing assets."
seo_title: "Fix Immich Thumbnail Generation Stuck on TrueNAS"
seo_description: "New photos appear in Immich but thumbnails never finish on TrueNAS. Diagnose the stuck thumbnail queue, fix the worker, and recover without losing assets."
keywords:
  - "immich thumbnail generation stuck"
date: 2026-11-24T10:09:20+08:00
publishDate: 2026-11-24T10:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You upload new photos from your phone, they appear in Immich, but the thumbnail generation stays stuck and the grid fills with blank placeholders.

## Confirm the queue is actually stalled

Open Immich as admin and check the job queue under Administration > Jobs. Look at the thumbnail generation job. If pending or active counts stay unchanged for more than ten minutes with no progress, the worker is stalled rather than just slow. Note the exact job name and any error text shown.

## Check the container and logs

On TrueNAS, open the Immich app and confirm the machine learning and microservices containers are running. Then inspect the logs for the thumbnail worker. Look for repeated errors about missing files, permission denied, or a database connection timeout. A permission error on the uploads or thumbs dataset is the most common cause: the container user cannot write to the thumbnail folder. Verify the dataset path and ownership match the user the container runs as.

## Clear the stuck job

In the Jobs page, cancel the stalled thumbnail job, then restart the Immich app from TrueNAS. After it comes back, trigger a library scan. Watch the queue counts move. If they remain frozen, restart the microservices container alone and recheck.

## Verification test

Upload one new photo from your phone. Within a few minutes the thumbnail should appear and the queue count should return to zero. If it does, the worker recovered.

## Rollback note

If changes to dataset permissions caused the stall, restore the previous ownership before retrying. If the issue persists after a restart,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your logs and queue screenshot.

If the **immich thumbnail generation stuck** state returns after each restart, capture the worker log before clearing the job again.
