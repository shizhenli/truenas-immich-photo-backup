---
title: "Immich External Library Not Scanning: Fix a Mounted Folder With No Assets"
description: "Troubleshoot an Immich external library that shows no assets after adding a mounted photo folder on TrueNAS, with permission checks and a verification test."
seo_title: "Immich External Library Not Scanning on TrueNAS: Fix No Assets"
seo_description: "Troubleshoot an Immich external library that shows no assets after adding a mounted photo folder on TrueNAS, with permission checks and a verification test."
keywords:
  - "immich external library not scanning"
date: 2026-11-15T19:09:20+08:00
publishDate: 2026-11-15T19:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You mounted a phone photo folder on your TrueNAS host, added it as an external library in Immich, but no assets appear. Start by confirming the folder is visible from inside the Immich container, not just from the TrueNAS shell. Open a shell in the Immich container and run `ls -la /path/to/your/photos`. If you get permission denied, the container user cannot read the mount. On TrueNAS, check the dataset ACL: the Immich user (often UID 1000 or the app's configured user) needs read and execute on every parent directory and read on files. If `ls` lists files, permissions are fine and the problem is likely the library path or scan trigger.

## Check the library path and trigger a scan

In Immich, open Administration > External Libraries, select the library, and verify the import path matches the container path exactly, including case. A host path like `/mnt/tank/photos` is not valid inside the container unless that path is mounted there. Correct it, save, then click Scan Library. Watch the job queue: a scan job that finishes instantly with zero discovered files usually means the path is wrong or empty from the container's view.

## Verification test

Create a test file inside the mounted folder from the TrueNAS shell, for example `touch /mnt/tank/photos/test.jpg`, then run Scan Library again. If the test file is still not discovered, the mount or permissions are wrong. If it appears, the mount is correct and the original files may be in an unsupported format or a subfolder excluded by your settings. Remove the test file afterward.

If the scan still fails after these checks, this is the point where **immich external library not scanning** needs deeper log review. Check the Immich server logs for permission errors or path-not-found messages during the scan job. As a rollback, remove the external library entry, fix the mount or ACL, and re-add it; this does not delete your original photos. For guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
