---
title: "Immich Upload Denied by Dataset Permissions: A Controlled Diagnosis on TrueNAS"
description: "Mobile or web uploads fail while the Immich dataset has free space. Diagnose immich truenas permission denied with exact checks, a verification test, and rollback steps."
seo_title: "Fix immich truenas permission denied: Upload Denied by Dataset Permissions"
seo_description: "Mobile or web uploads fail while the Immich dataset has free space. Diagnose immich truenas permission denied with exact checks, a verification test, and rollback steps."
keywords:
  - "immich truenas permission denied"
date: 2026-11-16T11:09:06+08:00
publishDate: 2026-11-16T11:09:06+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You are running Immich on TrueNAS with a phone photo library, and uploads fail at the immich upload is denied by dataset permissions step even though the dataset exists and has free space.

## Confirm the failure point

Open the Immich web UI and attempt a small upload from the browser. If it fails immediately with a permission error, the problem is dataset ownership or mode, not storage. Then check the dataset from the TrueNAS shell:

`ls -ld /mnt/<pool>/<dataset>`

Interpret the result: the owner and group should match the user Immich runs as, typically the built-in apps user or a dedicated immich user. If the owner is root and the mode is 755, the Immich process cannot write. Also run `df -h` on the dataset to confirm free space is not the cause.

## Check the container mapping

Inspect the Immich app configuration and confirm the host path maps to the same dataset you checked. A mismatch between the mounted path and the dataset path produces the same denial. Verify the app's user and group IDs, then compare them to the dataset owner.

## Verification test

Create a test file as the Immich user:

`sudo -u <immich-user> touch /mnt/<pool>/<dataset>/upload-test`

If this succeeds, retry a small upload in Immich. If it fails, correct ownership with `chown -R <immich-user>:<immich-group>` on the dataset, then repeat the test. This resolves most cases of **immich truenas permission denied**.

## Rollback note

Before changing ownership recursively, record the current owner and mode with `ls -ld`. If other services share the dataset, restore the previous ownership and mode instead of leaving broad permissions in place. If the denial persists after ownership and mapping are correct,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exact ls output and app configuration.
