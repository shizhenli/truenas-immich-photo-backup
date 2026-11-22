---
title: "Immich Upload Denied by Dataset Permissions: Diagnose the Storage Path Before You Change Anything"
description: "Mobile or web uploads fail even though the dataset has free space. Diagnose the immich storage path permission mismatch on TrueNAS, verify with a test write, and recover safely."
seo_title: "Immich Storage Path Permission: Fix Upload Denied on TrueNAS"
seo_description: "Mobile or web uploads fail even though the dataset has free space. Diagnose the immich storage path permission mismatch on TrueNAS, verify with a test write, and recover safely."
keywords:
  - "immich storage path permission"
date: 2026-11-22T11:09:31+08:00
publishDate: 2026-11-22T11:09:31+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone shows an upload error while the Immich dataset on TrueNAS clearly has free space.

## Confirm the failure point

Open the Immich container logs and look for permission denied or EACCES messages naming the upload folder. If the log shows a write error on the storage path, the dataset exists but the Immich process cannot write into it. If the log shows a network or connection error instead, stop here; permissions are not the cause.

## Check ownership and mode

On the TrueNAS shell, run ls -ld on the exact host path mounted into the container. Note the owner user and group, then compare them with the user ID and group ID the Immich container runs as. A mismatch means the process is treated as an outsider. Also check that the mode grants write to the group or owner the container uses. If the path sits under a parent directory, check each parent with ls -ld as well, because a restrictive parent blocks access even when the leaf looks correct. This is the usual **immich storage path permission** gap.

## Verification test

From inside the running container, create a small test file in the upload directory and then remove it. Success means the path is writable and you can retry an upload from the phone. Failure means the identity or mode is still wrong; adjust one variable at a time and repeat the test.

## Rollback and recovery

Before changing ownership or mode, record the original values. If Immich or another service breaks, restore those exact values and restart the container. Do not run recursive permission changes across the whole pool.

If the test still fails after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log lines and ls output.
