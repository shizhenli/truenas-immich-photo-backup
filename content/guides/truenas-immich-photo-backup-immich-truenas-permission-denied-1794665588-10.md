---
title: "Immich Upload Denied by Dataset Permissions: Diagnose the ACL Before Changing It"
description: "Mobile or web uploads fail with permission denied even though the Immich dataset exists and has free space. Diagnose the dataset ACL and ownership on TrueNAS, then verify with a controlled test."
seo_title: "Immich TrueNAS Permission Denied: Fix Uploads When the Dataset Has Free Space"
seo_description: "Mobile or web uploads fail with permission denied even though the Immich dataset exists and has free space. Diagnose the dataset ACL and ownership on TrueNAS, then verify with a controlled test."
keywords:
  - "immich truenas permission denied"
date: 2026-11-21T16:26:40+08:00
publishDate: 2026-11-21T16:26:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS and phone or web uploads suddenly fail with permission denied, even though the dataset exists and shows free space.

## Confirm the failure is permissions, not capacity

In the Immich web UI, open the upload error and note the exact message. Then check the dataset in TrueNAS: confirm it is mounted, has free space, and is not read-only. If the error says permission denied rather than no space left, move to the next check.

## Inspect ownership and ACL on the dataset

From a TrueNAS shell, run `ls -ld` on the Immich upload and library paths. Record the owner, group, and mode. Then run `getfacl` on the same paths. The Immich container user must have write and execute on every directory in the path, not just the top-level dataset. A common cause is a parent folder owned by root with mode 755 while the container runs as a non-root user.

## Run a controlled write test

Create a test file as the same user the Immich container runs as, inside the upload directory. If that write fails, the problem is confirmed as an **immich truenas permission denied** condition at the dataset level. If it succeeds, the issue is inside Immich configuration rather than the filesystem.

## Apply the smallest fix and verify

Correct ownership or add an ACL entry granting the container user modify rights on the affected path. Avoid broad recursive changes until you have identified the exact directory. Then retry a single small upload from the phone and confirm it appears in the library.

## Rollback note

Before changing ACLs, record the current `getfacl` output. If uploads still fail or other services lose access, restore the saved ACL and recheck the container user mapping. For guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
