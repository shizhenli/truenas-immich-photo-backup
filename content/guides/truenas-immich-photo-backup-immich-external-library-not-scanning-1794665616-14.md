---
title: "Mounted Folder Added but No Assets Appear: Resolving Immich External Library Scanning"
description: "Troubleshoot why a mounted photo folder shows no assets in Immich on TrueNAS. Step-by-step checks for permissions, mount visibility, and scan triggers to restore your external library."
seo_title: "Immich External Library Not Scanning: Fix a Mounted Folder With No Assets"
seo_description: "Troubleshoot why a mounted photo folder shows no assets in Immich on TrueNAS. Step-by-step checks for permissions, mount visibility, and scan triggers to restore your external library."
keywords:
  - "immich external library not scanning"
date: 2026-11-22T08:09:23+08:00
publishDate: 2026-11-22T08:09:23+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You mounted a phone photo folder on your TrueNAS host, added it as an external library in Immich, but no assets appear. This is a common **immich external library not scanning** issue, and it usually comes down to permissions or mount visibility.

## Check mount visibility inside the container

First, confirm Immich can see the folder. From the TrueNAS shell, run:

```
docker exec -it immich_server ls -la /mnt/photos
```

Replace `/mnt/photos` with your container path. If you get "No such file or directory," the host path is not mounted correctly. Verify the volume mapping in your Immich app configuration and restart the container.

## Verify permissions

The Immich user (often `immich` or UID 1000) must have read and execute access to the folder and its contents. Run:

```
docker exec -it immich_server id
```

Then check ownership on the host:

```
ls -la /mnt/your-photo-folder
```

If the UID does not match, adjust ownership with `chown -R` or add the user to the correct group. On TrueNAS, use the dataset permissions UI to grant read access to the Immich user.

## Trigger a manual scan

After fixing permissions, go to the Immich web UI, open the external library, and click the scan button. Watch the server logs:

```
docker logs -f immich_server
```

You should see lines indicating files are being discovered. If the scan completes instantly with no files, the path is still wrong or empty.

## Verification test

Place a single test image in the mounted folder. Trigger a scan and check if it appears in Immich. If it does, your configuration is correct; remove the test file and rescan.

## Rollback note

If changes make things worse, revert the volume mapping and permissions to their previous state, then restart Immich. Keep a note of original ownership before using `chown`.

Still stuck?

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) for a guided diagnosis.
