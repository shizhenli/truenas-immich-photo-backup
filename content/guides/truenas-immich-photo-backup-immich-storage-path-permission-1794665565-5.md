---
title: "Immich Upload Denied by Dataset Permissions: A Controlled Diagnosis"
description: "Mobile or web uploads fail with permission denied even though the dataset has free space. Run these checks to confirm and fix the immich storage path permission issue on TrueNAS."
seo_title: "Fix Immich Storage Path Permission Denied on TrueNAS"
seo_description: "Mobile or web uploads fail with permission denied even though the dataset has free space. Run these checks to confirm and fix the immich storage path permission issue on TrueNAS."
keywords:
  - "immich storage path permission"
date: 2026-11-21T16:23:39+08:00
publishDate: 2026-11-21T16:23:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS, the dataset exists with free space, but phone and web uploads fail with a permission-denied error before you can confirm the cause.

## Confirm the error and the path

Open the Immich container logs and look for the exact upload error. A permission denial typically reads `EACCES` or `permission denied` and names the target folder. Note that path; it must match the dataset mount inside the container, not the host path. If the log shows a different folder, stop and correct the mount first.

## Check ownership and mode on the host

From the TrueNAS shell, run `ls -ldn` on the dataset mount and on its parent. Compare the numeric owner and group with the user ID Immich runs as. If the owner is root and the mode lacks write for others, uploads will be denied. Also run `getfacl` on the same path; an explicit deny or a missing entry for the Immich user overrides the mode bits. Interpret results this way: owner mismatch means change ownership; an ACL deny means adjust or remove that entry; correct owner but no write bit means fix the mode.

## Apply the smallest correction

Set ownership to the Immich user and group, then grant write on the directory only. Avoid recursive changes on the whole pool. This is the **immich storage path permission** step most often missed: the parent folder is writable but the upload subfolder is not.

## Verification test

Upload one small photo from the web UI, then one from the phone. Confirm the file appears under the expected folder and that logs show no denial. If either fails, recheck the ACL on the exact subfolder named in the log.

## Rollback note

Record the previous owner, mode, and ACL before changing them. To recover, restore those values with `chown`, `chmod`, and `setfacl` using your saved output.

If the denial persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
