---
title: "Immich Thumbnail Generation Stuck: Diagnose the Worker Queue"
description: "New photos appear in Immich on TrueNAS, but thumbnails never finish. Check the thumbnail worker, queue, and logs to clear the stall."
seo_title: "Fix Immich Thumbnail Generation Stuck on TrueNAS"
seo_description: "New photos appear in Immich on TrueNAS, but thumbnails never finish. Check the thumbnail worker, queue, and logs to clear the stall."
keywords:
  - "immich thumbnail generation stuck"
date: 2026-11-22T09:09:18+08:00
publishDate: 2026-11-22T09:09:18+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added photos from your phone, the assets show up in Immich, but the thumbnail job never completes on your TrueNAS host.

## Check the worker and queue

Open the Immich web UI and go to Administration > Jobs. Find the Thumbnail Generation job and note its status. If it shows active but the count does not move, the worker is stalled rather than idle. Next, open the logs for the immich-server container on TrueNAS (Apps > Instances > immich-server > Logs). Look for repeated errors such as sharp, libvips, or out-of-memory messages. A healthy worker logs progress; a stuck one repeats the same failure or goes silent.

## Confirm storage and permissions

From a TrueNAS shell, check free space on the dataset holding Immich uploads and thumbnails. If the dataset is full, generation cannot write output. Also confirm the immich user can write to the thumbnails folder. A permissions error here produces exactly this symptom: assets appear, thumbnails do not.

## Clear the stalled queue

In Administration > Jobs, stop the Thumbnail Generation job, then start it again. If it stalls immediately, restart the immich-server container from TrueNAS Apps. After restart, watch the job count for two minutes. A moving count means recovery; a frozen count means the underlying error persists.

## Verification test

Upload one new photo from your phone. Within a few minutes, its thumbnail should appear in the main timeline. Then recheck Administration > Jobs: the thumbnail queue should drain to zero. If it does not, the **immich thumbnail generation stuck** state is still active and needs log-level diagnosis.

## Rollback note

If a restart makes things worse, stop the container and restore your last TrueNAS app snapshot before retrying. For guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
