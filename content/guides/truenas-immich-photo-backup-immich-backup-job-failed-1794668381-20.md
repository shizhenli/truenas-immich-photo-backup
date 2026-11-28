---
title: "Immich Backup Job Failed: Clear a Stuck Thumbnail Queue on TrueNAS"
description: "New photos appear in Immich but thumbnails never finish. Diagnose a stalled thumbnail queue after an immich backup job failed, with checks, a verification test, and rollback steps."
seo_title: "Immich Backup Job Failed: Fix Stuck Thumbnail Generation on TrueNAS"
seo_description: "New photos appear in Immich but thumbnails never finish. Diagnose a stalled thumbnail queue after an immich backup job failed, with checks, a verification test, and rollback steps."
keywords:
  - "immich backup job failed"
date: 2026-11-28T21:57:28+08:00
publishDate: 2026-11-28T21:57:28+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone uploads new photos to Immich on TrueNAS, but the thumbnail generation stays stuck and the library never looks finished. This usually means the job that processes new assets stopped partway, so the queue never drains. Work through the checks below before changing anything.

## Check the job and worker state

Open the Immich administration panel and look at the job queue for thumbnails. If pending jobs keep growing while active jobs stay at zero, the worker is not processing. Then check the TrueNAS apps screen and confirm the Immich container is running and has not restarted recently. A container that restarts repeatedly will abandon thumbnail work, which often shows up as an **immich backup job failed** entry in the logs.

## Confirm storage and permissions

From the TrueNAS shell, check free space on the dataset holding Immich uploads. If usage is near full, thumbnail writes fail silently and the queue stalls. Also verify the upload and thumbnail folders are writable by the Immich user. A permissions change after an update is a common cause.

## Verification test

Upload one new photo from your phone and watch the thumbnail queue for five minutes. If the pending count drops and a thumbnail appears, processing has recovered. If the count stays flat, stop and review the container logs before retrying.

## Recovery note

If you restarted or reconfigured the container, re-run thumbnail generation for the affected assets from the admin panel. Keep a snapshot of the Immich dataset before making permission changes so you can roll back cleanly.

If the queue still will not move,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
