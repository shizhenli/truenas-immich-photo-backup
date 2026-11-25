---
title: "Immich Upload Denied by Dataset Permissions on TrueNAS: Diagnose Before You Change ACLs"
description: "Uploads fail with permission denied on TrueNAS even though the Immich dataset has free space. Run these checks to confirm ownership and ACLs, then verify with a test upload."
seo_title: "Immich TrueNAS Permission Denied: Fix Uploads When Dataset Has Space"
seo_description: "Uploads fail with permission denied on TrueNAS even though the Immich dataset has free space. Run these checks to confirm ownership and ACLs, then verify with a test upload."
keywords:
  - "immich truenas permission denied"
date: 2026-11-25T11:09:20+08:00
publishDate: 2026-11-25T11:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS, the dataset exists with free space, but a phone or web upload fails with a permission error before you have confirmed why.

## Confirm the failure is permissions, not space or network

On the TrueNAS shell, run `df -h` on the Immich dataset path. If use is below 100 percent, space is not the cause. Then check the upload log inside Immich or `docker logs immich-server` for `EACCES` or `permission denied` on the upload folder. That exact string confirms the dataset is rejecting writes.

## Check ownership and ACLs on the upload path

Run `ls -ld` on the dataset mount and the `upload` subfolder. Note the owner and group. Next run `getfacl` on the same paths. The Immich container user must match the owner or have write permission through the ACL. If the owner is root and the container runs as a non-root UID, writes will be denied. Also confirm the dataset is not mounted read-only in the app configuration.

## Verification test

Create a test file as the container user inside the upload directory, for example `docker exec -u <uid> immich-server touch /path/upload/testfile`. If that fails, the permission mapping is still wrong. Fix ownership or add a write ACL for the container UID, then retry. Delete the test file afterward.

## Rollback note

Before changing ACLs, record the current `getfacl` output. If uploads still fail or other apps lose access, restore the saved ACL with `setfacl --restore`. This reverses your change without rebuilding the dataset.

If the checks are inconclusive,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your `ls -ld`, `getfacl`, and container UID output. A confirmed **immich truenas permission denied** diagnosis prevents random ACL edits.
