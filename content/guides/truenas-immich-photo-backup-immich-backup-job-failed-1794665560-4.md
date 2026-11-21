---
title: "Immich Backup Job Failed: Why Thumbnails Stay Stuck on TrueNAS"
description: "Fix a failed Immich backup job on TrueNAS where new photos appear but thumbnail generation never finishes. Step-by-step diagnosis and recovery."
seo_title: "Immich Backup Job Failed: Thumbnail Generation Stuck on TrueNAS"
seo_description: "Fix a failed Immich backup job on TrueNAS where new photos appear but thumbnail generation never finishes. Step-by-step diagnosis and recovery."
keywords:
  - "immich backup job failed"
date: 2026-11-21T16:23:39+08:00
publishDate: 2026-11-21T16:23:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS, new phone photos appear in the library, but thumbnail generation stays stuck and never completes, leaving you unable to confirm whether the backup job actually succeeded.

## Check the Immich job queue

Open the Immich web UI as an administrator and go to Administration > Jobs. Look at the Thumbnail Generation queue. If the active count is greater than zero but the completed count is not increasing over several minutes, the worker is stalled rather than busy. Note the queue length and the timestamp of the oldest waiting job. A queue that grows while completed stays flat confirms a stuck worker, not slow processing.

## Inspect the container logs

On the TrueNAS host, open a shell and run `docker logs immich_microservices --tail 100` (or the equivalent for your app deployment). Look for repeated errors such as out-of-memory kills, permission denied on the upload or thumbs folders, or database connection timeouts. If you see permission denied, the dataset permissions are wrong; if you see OOM, the container needs more memory. Either condition explains why the **immich backup job failed** to finish thumbnail work.

## Verify with a single test asset

Upload one small JPEG from the web UI. Watch the Thumbnail Generation queue for two minutes. If the test asset also stays pending, the worker is still blocked; if it completes, the earlier backlog is the problem and you can safely restart the microservices container to clear it.

## Rollback and recovery

Before restarting, stop the Immich app in TrueNAS, confirm the database and upload datasets are intact, then start the app again. If thumbnails still stall, restore the most recent app configuration snapshot and retry the test asset.

If the queue remains stuck after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and queue counts.
