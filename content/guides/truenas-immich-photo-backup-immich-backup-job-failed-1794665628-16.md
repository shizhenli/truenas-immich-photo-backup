---
title: "Immich Thumbnail Generation Stuck: A Controlled Diagnosis for TrueNAS"
description: "New photos appear in Immich on TrueNAS, but thumbnail generation never finishes. Follow these checks to diagnose the stuck job and recover safely."
seo_title: "Immich Backup Job Failed: Fix Stuck Thumbnail Generation on TrueNAS"
seo_description: "New photos appear in Immich on TrueNAS, but thumbnail generation never finishes. Follow these checks to diagnose the stuck job and recover safely."
keywords:
  - "immich backup job failed"
date: 2026-11-22T10:09:26+08:00
publishDate: 2026-11-22T10:09:26+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added new photos to your phone library, they show up in Immich on your TrueNAS host, but the thumbnail generation stays stuck and never completes. This is a common symptom when the background job that creates previews cannot finish, often reported as an **immich backup job failed** in the job queue. Let's diagnose it step by step.

## Check the job queue and logs

Open the Immich web UI and go to Administration > Jobs. Look at the Thumbnail Generation job. If it shows as active but the processed count is not increasing, the worker is stalled. Next, open the Immich server logs on TrueNAS (via the app's log viewer or `docker logs immich_server`). Search for errors around thumbnail generation, such as permission denied, out of memory, or missing ffmpeg. If you see permission errors on the upload or thumbs folder, the job cannot write previews.

## Verify storage and permissions

On TrueNAS, confirm the dataset holding Immich uploads has free space and that the immich user owns the thumbs directory. Run `df -h` on the pool and check that the dataset is not full. Then inspect the thumbs folder: if it is owned by root instead of the immich user, the worker will fail silently. Correct ownership with `chown -R immich:immich /path/to/immich/thumbs` (adjust the path to your setup).

## Run a verification test

Restart the Immich server app from TrueNAS. Upload a single new photo from your phone. Watch the Thumbnail Generation job: the processed count should increment within a minute. If it stays at zero, the worker is still blocked. Check logs again for the same error.

## Rollback and recovery

If the job remains stuck after these checks, stop the Immich app, back up the database, and restore the most recent working snapshot of the uploads dataset before retrying. This prevents partial thumbnails from corrupting the library. If you need a guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
