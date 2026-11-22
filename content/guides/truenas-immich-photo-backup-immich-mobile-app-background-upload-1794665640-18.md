---
title: "Immich Mobile App Background Upload Stops After First Batch: A Controlled Diagnosis"
description: "Fix Immich mobile backup stalling after the first batch on TrueNAS. Follow these checks to diagnose and restore background upload."
seo_title: "Immich Mobile App Background Upload Stops After First Batch on TrueNAS"
seo_description: "Fix Immich mobile backup stalling after the first batch on TrueNAS. Follow these checks to diagnose and restore background upload."
keywords:
  - "immich mobile app background upload"
date: 2026-11-22T12:09:45+08:00
publishDate: 2026-11-22T12:09:45+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch of photos, the **immich mobile app background upload** simply stops, leaving the rest of your library untouched.

## Check the app's background behavior

Open Immich on your phone and go to Settings > Backup. Confirm that background backup is enabled and that the app has permission to run in the background. On Android, check that battery optimization is not restricting Immich; on iOS, ensure Background App Refresh is on. If these are off, the app will stop after the first batch when the screen locks.

## Inspect the server's upload logs

On your TrueNAS host, open the Immich container logs and look for upload entries around the time the first batch finishes. If you see repeated timeouts or connection resets, the issue is likely network or server-side. Check that the TrueNAS dataset storing uploads has enough free space and that the Immich user has write permissions. A full dataset or permission error will halt uploads silently.

## Verify with a small test

Create a new album on your phone with five photos that have never been backed up. Trigger a manual backup and watch the progress. If all five upload, the background process is working; if it stops after the first one, the problem is in the background execution or server response.

## Rollback and recovery

If you changed any settings during diagnosis, revert them to their previous state. For a stuck upload, you can safely restart the Immich container on TrueNAS; this will not delete existing photos. After restart, re-enable background backup and monitor the next batch.

If the stall persists,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) for a guided review of your TrueNAS and Immich configuration.
