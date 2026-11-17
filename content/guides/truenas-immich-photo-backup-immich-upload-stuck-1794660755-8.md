---
title: "Immich Upload Stuck: Why Mobile Backup Halts After the First Batch"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore full photo uploads."
seo_title: "Immich Upload Stuck After First Batch: Troubleshooting Mobile Backup on TrueNAS"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, permissions, and network to restore full photo uploads."
keywords:
  - "immich upload stuck"
date: 2026-11-17T08:17:20+08:00
publishDate: 2026-11-17T08:17:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You start a mobile backup in Immich, the first batch uploads, then the progress bar freezes and no more photos transfer. This is a common **immich upload stuck** symptom, and it usually points to a server-side limit or a permission issue that only appears after the initial batch.

## Check the Immich server logs

On your TrueNAS host, open the Immich app logs (via the TrueNAS UI or `docker logs immich-server`). Look for the last successful upload entry and any error immediately after it. If you see `EACCES` or `permission denied`, the upload folder is not writable by the Immich user. If you see `ENOSPC`, the dataset is full. If the log simply stops with no error, the connection may have dropped.

## Verify dataset permissions and free space

In TrueNAS, go to Datasets and check the Immich upload dataset. Confirm the owner matches the user inside the Immich container (often `immich` or UID 1000). Check available space; a full pool will silently stop new uploads. Also confirm the dataset is not read-only.

## Test network stability

On the phone, switch from Wi-Fi to cellular (or vice versa) and retry the backup. If the second batch uploads, the original network had a timeout or firewall rule blocking sustained transfers. Check your router for client isolation or idle timeout settings.

## Verification test

After fixing permissions or space, force-stop the Immich app on your phone, reopen it, and start backup again. Watch the server logs for new upload entries. A successful test shows multiple batches completing without a gap.

## Rollback note

If you changed dataset permissions and uploads still fail, revert to the previous owner settings and consult the TrueNAS Immich guide before making further changes. If the issue persists,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
