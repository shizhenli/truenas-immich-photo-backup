---
title: "Why Your Immich External Library Won't Scan on TrueNAS"
description: "A mounted photo folder shows in Immich but no assets appear. Follow these checks to diagnose why your TrueNAS Immich external library is not scanning."
seo_title: "Immich External Library Not Scanning on TrueNAS: Fix Mounted Folder"
seo_description: "A mounted photo folder shows in Immich but no assets appear. Follow these checks to diagnose why your TrueNAS Immich external library is not scanning."
keywords:
  - "immich external library not scanning"
date: 2026-11-24T09:09:22+08:00
publishDate: 2026-11-24T09:09:22+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You added a mounted photo folder to Immich on TrueNAS, but the library stays empty and no assets appear.

## Check the mount path inside the container

Open the Immich app configuration and confirm the host path is mapped to a container path, for example /photos. Then open a shell in the Immich container and run `ls /photos`. If you see your files, the mount is correct. If the directory is empty or missing, fix the volume mapping in the TrueNAS app settings and restart the container.

## Check permissions and ownership

From the TrueNAS host, run `ls -ln` on the mounted folder. Note the numeric user and group IDs. Immich must be able to read those files. If the IDs do not match the user Immich runs as, adjust ownership with `chown -R` or grant read access with `chmod`. A permission denial here is a common reason for **immich external library not scanning** without an obvious error.

## Check the library import path

In Immich, edit the external library and verify the import path exactly matches the container path, including case. Save the change and trigger a scan. Watch the server logs with `docker logs -f immich_server` while the scan runs. A path error or permission error will appear there.

## Verification test

Create a small test folder with one image inside the mounted path. Trigger a scan and confirm that image appears in Immich. If it does, the configuration is correct and the original folder likely has a deeper permission or path issue.

## Rollback note

If a permission change makes things worse, restore the previous ownership values you recorded before editing. If the scan still fails,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your mount path and log output.
