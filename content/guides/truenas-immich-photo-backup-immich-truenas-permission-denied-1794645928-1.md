---
title: "Fix Immich Upload Denied by Dataset Permissions on TrueNAS"
description: "Troubleshoot Immich uploads failing with permission denied on TrueNAS. Check dataset ownership, ACLs, and verify with a test file."
seo_title: "Immich TrueNAS Permission Denied: Fix Uploads"
seo_description: "Troubleshoot Immich uploads failing with permission denied on TrueNAS. Check dataset ownership, ACLs, and verify with a test file."
keywords:
  - "immich truenas permission denied"
date: 2026-11-14T19:50:04+08:00
publishDate: 2026-11-14T19:50:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS and your phone or web upload fails with a permission error even though the dataset exists and has free space.

## Check dataset ownership and permissions
Open the TrueNAS shell and run `ls -ld /mnt/yourpool/yourdataset`. The owner and group should match the user Immich runs as (often `apps` or `netdata`). If they show `root` or a different user, that is your culprit. Next, run `getfacl /mnt/yourpool/yourdataset` and look for `user:apps:rwx` or equivalent. Missing write permission for the Immich user confirms the **immich truenas permission denied** error.

## Fix ownership and ACLs
In TrueNAS, edit the dataset permissions via the UI: set the owner to the Immich user and group, and apply recursively. Alternatively, from the shell run `chown -R apps:apps /mnt/yourpool/yourdataset` and `setfacl -R -m u:apps:rwx /mnt/yourpool/yourdataset`. Do not use `chmod 777` as a shortcut; it weakens security and may not fix ACL inheritance.

## Verification test
Create a test file as the Immich user: `sudo -u apps touch /mnt/yourpool/yourdataset/testfile`. If it succeeds, delete it and retry an upload from the Immich web interface. A successful upload confirms the fix. If it still fails, check the Immich container logs for the exact path being written.

## Rollback note
If you changed ownership recursively and other services break, restore the previous owner with `chown -R root:root /mnt/yourpool/yourdataset` and reapply only the Immich-specific ACL entry. Keep a note of original permissions before editing.

If these steps do not resolve it,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your dataset path and Immich log excerpt.
