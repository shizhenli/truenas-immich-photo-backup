---
title: "Immich Upload Denied by Dataset Permissions: Diagnose the Storage Path Before You Change ACLs"
description: "Immich uploads fail with permission denied even though the dataset has free space. Learn how to trace the storage path and verify write access before changing ACLs."
seo_title: "Fix Immich Upload Denied by Dataset Permissions on TrueNAS"
seo_description: "Immich uploads fail with permission denied even though the dataset has free space. Learn how to trace the storage path and verify write access before changing ACLs."
keywords:
  - "immich storage path permission"
date: 2026-11-28T21:57:28+08:00
publishDate: 2026-11-28T21:57:28+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich storage path permission**. Your phone shows a failed upload in Immich, the TrueNAS dataset exists and has free space, and you are stuck at the immich upload is denied by dataset permissions step before you have completed a controlled diagnosis. This article walks you through the checks that isolate the cause without guessing.

## Confirm the exact storage path Immich is writing to

Open the Immich app configuration on TrueNAS and note the container path mapped to your photo library, then confirm the host dataset path it points to. In the Immich admin settings, check the storage template and library location. If the container path and the host dataset path do not match, Immich is writing somewhere you did not intend, and permissions on the intended dataset are irrelevant.

## Check ownership and write access on the host

From a TrueNAS shell, run `ls -ld` on the dataset mount point and on a subfolder Immich created. The owner and group must match the user or group Immich runs as inside the container. Then test write access as that user, for example with `sudo -u <immich-user> touch <dataset-path>/testfile`. If this returns permission denied, the dataset ACL or mode is blocking writes. If it succeeds, the problem is inside the container mapping, not the dataset.

## Verify from Immich itself

Upload one small photo from the web interface. A success confirms the path and permissions are correct. A failure with a permission error in the Immich logs confirms the storage path permission issue is still active. Check the container logs for the exact path being denied.

## Rollback and recovery

Before editing ACLs, record the current permissions with `getfacl`. If a change makes things worse, restore the saved ACL. Avoid recursive permission changes on a dataset that also holds other data.

If the checks do not isolate the cause,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) before making further changes.
