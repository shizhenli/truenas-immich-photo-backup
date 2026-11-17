---
title: "Immich External Library Not Scanning: Diagnose a Mounted Folder With No Assets"
description: "A mounted photo folder in Immich shows no assets on TrueNAS. Follow these checks to diagnose why the external library is not scanning and restore it."
seo_title: "Immich External Library Not Scanning on TrueNAS: Fix No Assets"
seo_description: "A mounted photo folder in Immich shows no assets on TrueNAS. Follow these checks to diagnose why the external library is not scanning and restore it."
keywords:
  - "immich external library not scanning"
date: 2026-11-17T10:09:05+08:00
publishDate: 2026-11-17T10:09:05+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added a mounted photo folder to Immich on TrueNAS, but the external library does not scan and no assets appear.

## Confirm the mount is visible inside Immich

Open the Immich container shell and run `ls -la /path/to/your/photos`. If the folder is empty or missing, the mount is not reaching the container. Check your TrueNAS app configuration for the host path and mount path. A correct result lists your photo files. An incorrect result means the container cannot see the dataset, so scanning cannot start.

## Check ownership and read permissions

The Immich process must read the folder. Run `id` inside the container to get the user ID, then compare it with `ls -ln /path/to/your/photos` on the host. If the files are owned by another user and lack read permission for Immich, the scan will silently skip them. Fix ownership or add read access, then restart the app.

## Trigger and watch the scan

In Immich, open the external library and start a scan. Then watch the container logs for scan messages. If the log shows the path but no files, the path inside the container is wrong. If it shows permission errors, revisit the previous step. This is the most common cause of **immich external library not scanning**.

## Verification test

Create a small test folder with one image inside the mounted path, rescan, and confirm the image appears. If it does, the mount and permissions are correct and the original folder likely has a nested path issue.

## Rollback note

If a permission change breaks other apps, restore the previous ownership and test with the small folder again before rescanning the full library.

If the scan still produces no assets,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your mount path and log excerpt.
