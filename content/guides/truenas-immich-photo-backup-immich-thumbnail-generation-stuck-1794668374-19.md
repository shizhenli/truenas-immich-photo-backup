---
title: "Immich Thumbnail Generation Stuck on TrueNAS: A Controlled Diagnosis"
description: "New photos appear in Immich on TrueNAS but thumbnails never finish. Run these checks to confirm whether the queue, worker, or permissions are stalled, then verify recovery."
seo_title: "Fix Immich Thumbnail Generation Stuck on TrueNAS"
seo_description: "New photos appear in Immich on TrueNAS but thumbnails never finish. Run these checks to confirm whether the queue, worker, or permissions are stalled, then verify recovery."
keywords:
  - "immich thumbnail generation stuck"
date: 2026-11-26T10:07:39+08:00
publishDate: 2026-11-26T10:07:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

## Confirm the queue is actually stalled

You uploaded new photos to Immich on your TrueNAS host, the assets appear, but thumbnails never finish generating. First, open the Immich web UI and go to Administration > Jobs. Under the thumbnail job, note the waiting and active counts. If waiting stays above zero while active is zero for more than a few minutes, the queue is stalled rather than slow. If active is stuck on one item, a single corrupt asset may be blocking the worker.

## Check the worker and permissions

In TrueNAS, open the Immich app and inspect its logs for repeated errors mentioning ffmpeg, sharp, or permission denied. A permission error means the container user cannot read the upload directory or write to the thumbnail cache. Confirm the dataset mount paths in the app configuration match where your phone library actually lands. If logs show the worker restarting in a loop, the container is running out of memory or crashing on a specific file.

## Verification test

Pause the thumbnail job, then resume it. Upload one new test photo from your phone. Watch the active count: it should briefly rise and then return to zero while a thumbnail appears. If it stays at zero with waiting items, the worker is not consuming the queue. If it fails on one asset, delete that asset and retry.

## Rollback and recovery

Before changing permissions or clearing the thumbnail cache, note your current dataset settings so you can restore them. Clearing the cache forces regeneration and is safe because originals remain untouched. If the **immich thumbnail generation stuck** state persists after these checks, stop changing settings and gather logs.

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the log excerpt and job counts so the cause can be isolated without risking your library.
