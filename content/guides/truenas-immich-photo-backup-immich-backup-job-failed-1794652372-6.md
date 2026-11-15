---
title: "Immich Backup Job Failed: Thumbnail Generation Stuck on TrueNAS"
description: "Diagnose an immich backup job failed state where new phone photos appear but thumbnails never finish generating on TrueNAS, with checks, a verification test, and recovery steps."
seo_title: "Immich Backup Job Failed: Fix Stuck Thumbnail Generation"
seo_description: "Diagnose an immich backup job failed state where new phone photos appear but thumbnails never finish generating on TrueNAS, with checks, a verification test, and recovery steps."
keywords:
  - "immich backup job failed"
date: 2026-11-15T19:09:20+08:00
publishDate: 2026-11-15T19:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You uploaded new phone photos to Immich on TrueNAS, the assets show up, but thumbnail generation stays stuck and never completes.

## Confirm the Job Is Actually Stalled

Open the Immich admin panel and check the job queue for thumbnail generation. A job that shows active but makes no progress over ten minutes is stalled, not merely slow. Note the asset count before and after that window; if the number is identical, stop waiting and treat it as a failed job.

## Check Storage and Permissions First

On the TrueNAS host, verify free space on the dataset holding Immich uploads. Less than a few gigabytes free will silently block thumbnail writes. Then confirm the Immich app user can write to the thumbs directory. If thumbnails cannot be written, the job reports as an **immich backup job failed** condition even though uploads succeeded.

## Inspect the Job Logs

Open the Immich logs and filter for thumbnail errors. Look for permission denied, no space left, or decode errors on specific files. A single corrupt or unsupported file can block the queue. If the log names one asset repeatedly, move that file out of the library and restart the job.

## Verification Test

Upload one new photo from your phone. Watch the queue until that single asset finishes. If its thumbnail appears, the pipeline is healthy and the earlier backlog will drain. If it stalls again, the problem is environmental, not the file.

## Rollback and Recovery

Before changing permissions or clearing the queue, snapshot the Immich dataset in TrueNAS. If a restart or permission change makes things worse, roll back to that snapshot and restore the previous state. Clear only the stalled job, not the whole queue, so completed thumbnails are preserved.

If the queue still will not move after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your logs and dataset details.
