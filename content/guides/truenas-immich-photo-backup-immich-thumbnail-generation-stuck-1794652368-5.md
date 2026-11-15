---
title: "Immich Thumbnail Generation Stuck on TrueNAS: Diagnose and Recover"
description: "New photos appear in Immich on TrueNAS, but thumbnails never finish generating. Follow these checks to diagnose the stuck job and restore thumbnail generation."
seo_title: "Immich Thumbnail Generation Stuck on TrueNAS: Fix It"
seo_description: "New photos appear in Immich on TrueNAS, but thumbnails never finish generating. Follow these checks to diagnose the stuck job and restore thumbnail generation."
keywords:
  - "immich thumbnail generation stuck"
date: 2026-11-15T19:09:20+08:00
publishDate: 2026-11-15T19:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added new photos to Immich on your TrueNAS host, the assets show up, but the thumbnail generation stays stuck and the grid stays blank.

## Check the Job Queue

Open Immich's administration panel and look at the job queue for thumbnails. If the queue shows a growing number of pending jobs with no active worker, the worker is not processing. If it shows one job running for an unusually long time, a single asset may be blocking the queue. Note the count before changing anything so you can compare later.

## Inspect the Logs

On TrueNAS, open the Immich app logs and filter for thumbnail or job worker entries. Look for repeated errors such as permission denied, out of memory, or a missing file path. A permission error on the upload or thumbnail folder is the most common cause: the container user cannot read the original asset. Confirm the dataset permissions allow the Immich user to read and write.

## Verify Storage and Resources

Check that the dataset holding Immich data has free space. A full pool stops thumbnail writes silently. Also confirm the container has enough memory; thumbnail workers can be killed when memory is tight, leaving jobs pending.

## Verification Test

Restart the Immich app from TrueNAS, then upload one new photo from your phone. Watch the thumbnail queue: the job should move from pending to active and finish within a minute. If the new thumbnail appears, generation is working again.

## Rollback and Recovery

If the restart does not clear the backlog, pause the thumbnail job, clear the failed jobs, and resume it. Before changing permissions or configuration, snapshot the Immich dataset so you can roll back if the app fails to start. If the **immich thumbnail generation stuck** state returns after these steps, the logs will show whether the cause is storage, permissions, or memory. For a guided review,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
