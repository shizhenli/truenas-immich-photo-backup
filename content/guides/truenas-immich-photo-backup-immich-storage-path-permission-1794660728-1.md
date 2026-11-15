---
title: "Immich Storage Path Permission: Diagnosing Upload Denied by Dataset Permissions"
description: "Mobile or web Immich uploads fail with permission denied even though the TrueNAS dataset has free space. Here is a focused check of the immich storage path permission and a verification test."
seo_title: "Immich Storage Path Permission: Fix Upload Denied on TrueNAS"
seo_description: "Mobile or web Immich uploads fail with permission denied even though the TrueNAS dataset has free space. Here is a focused check of the immich storage path permission and a verification test."
keywords:
  - "immich storage path permission"
date: 2026-11-15T19:09:20+08:00
publishDate: 2026-11-15T19:09:20+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone or web upload to Immich fails with a permission error even though the dataset exists and shows free space on TrueNAS.

## Confirm the exact error before changing anything

Open the Immich container logs and reproduce the upload. Look for a line mentioning permission denied or EACCES on the upload path. If you see a network timeout or a 413 error instead, stop here; this is not a permissions fault.

## Check the dataset ownership and mode

On the TrueNAS host, run `ls -ldn` against the parent of your Immich upload directory. Note the numeric owner and group, and the mode. Then run `ls -ldn` on the upload directory itself. The container user (commonly UID 1000, but confirm from your compose file or app settings) must be able to write there. If the directory is owned by root with mode 755, the container user cannot create files, which matches your symptom.

## Check the mount, not just the host path

Confirm the host path you inspected is the one actually mounted into the container. A common trap is fixing permissions on a sibling folder while the container mounts a different subpath. Compare the compose volume source with the host path you just checked.

## Apply the fix and verify

Set the upload directory owner and group to the container user, and give the owner write and execute permission. Then restart the Immich server container. Verification test: upload a single small photo from the web UI, then confirm the file appears on the host with the expected owner. If it succeeds, repeat from the phone.

## Rollback note

Record the original owner, group, and mode before changing them. If uploads still fail, restore those values and re-check the mount path rather than widening permissions further.

If the **immich storage path permission** still blocks uploads after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log line and `ls -ldn` output.
