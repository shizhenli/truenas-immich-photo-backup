---
title: "Immich Storage Path Permission: Fix Upload Denied on TrueNAS"
description: "Fix Immich upload denied by dataset permissions on TrueNAS. Check ownership, ACLs, and verify with a test file without risking your photo library."
seo_title: "Immich Storage Path Permission Error on TrueNAS: Fix Upload Denied"
seo_description: "Fix Immich upload denied by dataset permissions on TrueNAS. Check ownership, ACLs, and verify with a test file without risking your photo library."
keywords:
  - "immich storage path permission"
date: 2026-11-24T12:11:40+08:00
publishDate: 2026-11-24T12:11:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You're running Immich on TrueNAS and your phone or web upload fails with a permission error even though the dataset exists and shows free space.

## Confirm the exact failure

First, reproduce the upload and note the error. In Immich, open the job log or server logs and look for `EACCES` or `permission denied` referencing your upload path. If you see that, the dataset is reachable but the Immich process cannot write to it.

## Check ownership and permissions

On the TrueNAS host, run:

```
ls -ld /mnt/yourpool/immich/upload
```

The owner and group must match the user Immich runs as. If Immich runs as a Docker container, check its PUID and PGID. The dataset must be owned by that UID/GID, or the process must be in a group with write access. Also check ACLs with `getfacl` on the upload folder. A common cause is an ACL entry that grants read but not write to the Immich user.

## Fix the permission

Correct the ownership or ACL so the Immich user has write access to the upload path. This is the core **immich storage path permission** issue: the path is correct, but the process lacks write rights. Do not chmod 777 as a permanent fix; set the owner or ACL properly.

## Verify with a test file

From the Immich container or host as the Immich user, run:

```
touch /mnt/yourpool/immich/upload/testfile
```

If it succeeds, remove the test file and retry an upload from the web UI. A successful upload confirms the fix.

## Rollback note

Before changing ownership or ACLs, record the current values with `ls -ld` and `getfacl`. If uploads still fail or other services break, restore the previous ownership and ACL entries.

If the check still fails,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your `ls -ld` and `getfacl` output.
