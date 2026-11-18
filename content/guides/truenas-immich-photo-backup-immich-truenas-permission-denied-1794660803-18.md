---
title: "Immich TrueNAS Permission Denied: Diagnose the Upload Denial"
description: "Mobile or web uploads fail with permission denied even though the dataset has free space. Diagnose the Immich TrueNAS permission denied error with exact checks and a verification test."
seo_title: "Immich TrueNAS Permission Denied: Fix Upload Denial"
seo_description: "Mobile or web uploads fail with permission denied even though the dataset has free space. Diagnose the Immich TrueNAS permission denied error with exact checks and a verification test."
keywords:
  - "immich truenas permission denied"
date: 2026-11-18T12:12:14+08:00
publishDate: 2026-11-18T12:12:14+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your Immich uploads fail with a permission denied error on TrueNAS, and you want to confirm the dataset permissions are the cause before changing anything.

## Check the dataset ownership and mode

Open the TrueNAS shell and run `ls -ld /mnt/your-pool/immich`. The third and fourth columns are the owner and group; the first column shows the mode. Immich's container user must match the owner or be in the group, and the mode must allow write. If the owner is root and the mode is `755`, a non-root Immich user cannot write. That result confirms the **immich truenas permission denied** condition.

## Check the container user

Run `docker exec -it immich_server id` (or the equivalent for your app). Note the uid and gid. Compare them with the dataset owner and group from the previous step. If they differ and the group lacks write, uploads will be denied.

## Apply the narrow fix

Set the dataset owner to the Immich uid and gid, for example `chown -R 1000:1000 /mnt/your-pool/immich`, then confirm the mode allows the owner to write. Avoid broad `777` changes; they hide the real mismatch.

## Verification test

From the Immich web UI, upload a single small photo. A successful upload with no permission error confirms the fix. Then retry one photo from the phone on the same LAN.

## Rollback note

Record the original owner, group, and mode before changing them. If Immich or another app breaks, restore those values with `chown` and `chmod` to return to the prior state.

If the checks still point to an unclear ownership mismatch,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the `ls -ld` and `id` output.
