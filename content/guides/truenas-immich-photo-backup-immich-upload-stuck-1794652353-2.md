---
title: "Immich Upload Stuck at the First Batch: A Step-by-Step Fix"
description: "Phone backup starts but photos stop after the first batch on your TrueNAS Immich server. Follow these checks to confirm the cause and restore a full mobile backup."
seo_title: "Immich Upload Stuck on TrueNAS: Fix the First-Batch Stall"
seo_description: "Phone backup starts but photos stop after the first batch on your TrueNAS Immich server. Follow these checks to confirm the cause and restore a full mobile backup."
keywords:
  - "immich upload stuck"
date: 2026-11-14T19:50:04+08:00
publishDate: 2026-11-14T19:50:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says the Immich backup started, but the photos stop after the first batch and never resume.

## Confirm the stall before changing anything

Open the Immich app on the phone and note the count of uploaded assets. Wait five minutes and check again. If the number is unchanged while the status still shows backup running, the job is stalled rather than slow. Then open the Immich web interface on the TrueNAS host and look at the server logs for the mobile upload endpoint. A repeated timeout or a permission error on the upload folder points to the cause. If the log shows the batch completed and no new request arrives, the phone side is the problem.

## Check the phone and server sides

On the phone, confirm the app has background upload permission and is not restricted by battery optimization. Disable any VPN and retry on the same Wi-Fi as the TrueNAS host. On the server, verify the upload dataset has free space and that the Immich user can write to it. A full dataset is a common reason an **immich upload stuck** state appears right after the first batch.

## Verification test

Create one new photo on the phone, then trigger a manual backup. Watch the server log for the new upload request. If the photo appears in the web interface within a minute, the path is clear and you can re-enable the full library backup.

## Rollback and recovery

If you changed permissions or app settings during diagnosis, revert them one at a time and retest with a single photo before starting the full backup again. Keep the original dataset permissions recorded so you can restore them if a change makes things worse.

If the stall returns after these steps,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your server log excerpt and phone model.
