---
title: "Immich External Library Not Scanning: A Controlled Diagnosis"
description: "A step-by-step technician guide to diagnose why an Immich external library on TrueNAS shows no assets after mounting, with permission checks, scan verification, and rollback notes."
seo_title: "Immich External Library Not Scanning on TrueNAS: Fix a Mounted Folder With No Assets"
seo_description: "A step-by-step technician guide to diagnose why an Immich external library on TrueNAS shows no assets after mounting, with permission checks, scan verification, and rollback notes."
keywords:
  - "immich external library not scanning"
date: 2026-11-26T09:07:37+08:00
publishDate: 2026-11-26T09:07:37+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You mounted a phone photo folder into Immich on TrueNAS, but the external library stays empty and no assets appear. This is the point where **immich external library not scanning** usually gets misdiagnosed as a broken mount, when the real cause is often permissions or a stale scan state.

## Confirm the mount is visible inside the container

Open the Immich container shell and run `ls -la /path/to/your/library`. You should see your photo files and folders. If the directory is empty or missing, the volume mapping is wrong. Check the TrueNAS app configuration and confirm the host path and container path match exactly. A correct result shows files owned by a user the Immich process can read.

## Check read permissions for the Immich user

Run `id` inside the container to find the Immich user ID, then compare it to the folder ownership with `ls -ln`. If the folder is owned by root with mode 700, Immich cannot read it. Adjust ownership or group permissions on the TrueNAS dataset so the Immich user has read and execute access. After changing permissions, restart the Immich container so the mount is re-evaluated.

## Trigger a manual scan and read the result

In Immich, open the external library settings and start a scan. Watch the job queue. A successful scan shows discovered files increasing. If the job completes instantly with zero discovered, the path inside the container is still wrong or unreadable. If it fails, note the error text before changing anything else.

## Verification test

Create a small test folder with one JPEG inside the mounted library path, then run the scan again. If that single file appears, permissions and mapping are correct and the original folder likely has a nested path issue. Remove the test file afterward.

## Rollback note

If a permission change breaks other apps, restore the previous ownership and mode from your notes before continuing. For deeper diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
