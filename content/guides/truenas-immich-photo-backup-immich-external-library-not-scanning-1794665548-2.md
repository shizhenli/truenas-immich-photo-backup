---
title: "Immich External Library Not Scanning: Diagnose a Mounted Folder With Zero Assets"
description: "A mounted photo folder in Immich shows no assets on TrueNAS. Follow these checks to diagnose why the external library does not scan and restore indexing."
seo_title: "Immich External Library Not Scanning on TrueNAS: Fix a Mounted Folder With No Assets"
seo_description: "A mounted photo folder in Immich shows no assets on TrueNAS. Follow these checks to diagnose why the external library does not scan and restore indexing."
keywords:
  - "immich external library not scanning"
date: 2026-11-21T16:20:36+08:00
publishDate: 2026-11-21T16:20:36+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added a mounted photo folder to Immich on TrueNAS, but no assets appear, and you are stuck at the external library does not scan step before a controlled diagnosis is completed.

## Confirm the mount is visible inside the Immich container

Open a shell on the TrueNAS host and run `docker exec -it immich_server ls -la /path/to/your/library`. Replace the path with the container path you configured for the external library. If the command returns `No such file or directory`, the host path is not mounted into the container. If it lists files, the mount is correct and the problem is elsewhere.

## Check permissions for the Immich user

Run `docker exec -it immich_server id` to see the user ID Immich runs as. Then run `docker exec -it immich_server ls -la /path/to/your/library` and compare the owner and group. If the folder is owned by root with mode 700, Immich cannot read it. Adjust ownership on the TrueNAS dataset so the Immich user has read and execute access, then restart the container.

## Verify the library path in Immich settings

In the Immich web UI, go to Administration, then External Libraries. Confirm the import path exactly matches the container path from the first check. A trailing slash or a typo will silently produce no assets. Save the library again and trigger a scan.

## Run a verification test

Create a small test folder inside the mounted path with one JPEG, then rescan. If that single file appears, the original folder likely has permission or path issues deeper in its tree. If nothing appears, the mount or container path is still wrong.

## Rollback and recovery

Before changing ownership or paths, note the original values. If a scan still fails after corrections, remove the external library entry in Immich, restart the container, and re-add it with the verified path. This does not delete original files.

If the **immich external library not scanning** persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
