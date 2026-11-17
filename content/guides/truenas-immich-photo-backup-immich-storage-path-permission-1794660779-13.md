---
title: "Immich Upload Denied by Dataset Permissions: Diagnose the Storage Path"
description: "Troubleshoot Immich uploads denied by dataset permissions on TrueNAS. Check ownership, ACLs, and the storage path, then verify with a test file and recover safely."
seo_title: "Fix Immich Upload Denied by Dataset Permissions on TrueNAS"
seo_description: "Troubleshoot Immich uploads denied by dataset permissions on TrueNAS. Check ownership, ACLs, and the storage path, then verify with a test file and recover safely."
keywords:
  - "immich storage path permission"
date: 2026-11-17T12:09:06+08:00
publishDate: 2026-11-17T12:09:06+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS, the dataset exists with free space, yet phone or web uploads fail with a permission-denied error. This points to the **immich storage path permission** on the dataset, not to disk capacity.

## Check ownership and ACLs

Open the TrueNAS shell and run `ls -ld /mnt/<pool>/<dataset>/<immich-upload-path>`. The owner and group should match the user Immich runs as, typically `apps` or a dedicated `immich` user. If the owner is `root` and the mode is `755`, the Immich process cannot write. Next, run `getfacl` on the same path. Look for an explicit deny entry or a missing write permission for the Immich user or group. An ACL deny overrides a permissive mode, so a path that looks writable can still reject uploads.

## Check the container mount

Confirm the Immich app configuration maps the host dataset to the container upload path. A mismatch between the host path and the container path means Immich writes to a different, unwritable location. Verify the mount source is the dataset you inspected, not a parent folder with stricter permissions.

## Verification test

As the Immich user, run `touch /mnt/<pool>/<dataset>/<immich-upload-path>/perm-test`. If it succeeds, remove the file and retry a small upload from the web UI. If `touch` fails, the permission issue is confirmed at the dataset level.

## Rollback and recovery

Before changing ownership or ACLs, record the current settings with `getfacl`. Apply the minimal change, such as adding write access for the Immich group, then retest. If uploads still fail, restore the saved ACL and review the container mount again. For guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
