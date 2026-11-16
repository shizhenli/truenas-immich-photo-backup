---
title: "Immich Mobile Backup Stops After the First Batch on TrueNAS"
description: "Troubleshoot Immich mobile backup stalling after the first batch on TrueNAS. Check server reachability, permissions, and storage, then verify with a test upload."
seo_title: "Immich Mobile Backup Not Working: Fix First-Batch Stall on TrueNAS"
seo_description: "Troubleshoot Immich mobile backup stalling after the first batch on TrueNAS. Check server reachability, permissions, and storage, then verify with a test upload."
keywords:
  - "immich mobile backup not working"
date: 2026-11-16T12:32:35+08:00
publishDate: 2026-11-16T12:32:35+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone shows backup started, but the progress bar freezes after the first batch and no new photos appear in Immich on your TrueNAS host. This is a common **immich mobile backup not working** symptom, and it usually points to a server-side limit rather than the phone itself.

## Check server reachability and storage

Open the Immich web UI from a desktop on the same LAN. If it loads, the server is up. Next, confirm free space on the dataset that holds Immich uploads; if it is full or near full, the first batch may fill it and later uploads are silently rejected. Check the Immich container logs for errors around the time the batch stops. A permission error or a disk-full message is your signal.

## Check dataset permissions

The Immich user inside the container must be able to write to the upload dataset. On TrueNAS, inspect the dataset ACL and confirm the container user has write access. If permissions were changed recently, the first batch may have succeeded before the change and later writes fail.

## Verification test

On the phone, disable backup, then re-enable it and upload a single new photo. Watch the Immich web UI: if the photo appears, the path works. If it stalls again at the same point, note the exact log line and timestamp.

## Rollback and recovery

If a recent permission or storage change caused the stall, revert that change, restart the Immich container, and retry the single-photo test. Do not delete the existing upload dataset; keep it intact until backup completes.

If these checks do not resolve the stall,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your logs and dataset details.
