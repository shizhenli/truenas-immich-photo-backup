---
title: "Immich Mobile Backup Not Working: Why It Stops After the First Batch"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server reachability, permissions, and free space, then verify with a small test album."
seo_title: "Immich Mobile Backup Not Working: Fix the First-Batch Stall on TrueNAS"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server reachability, permissions, and free space, then verify with a small test album."
keywords:
  - "immich mobile backup not working"
date: 2026-11-21T16:26:40+08:00
publishDate: 2026-11-21T16:26:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich mobile backup not working**. Your phone says backup started, then the progress bar freezes after the first batch and never resumes. That pattern usually means the app can reach Immich but cannot complete the next upload request, so let's isolate the cause before changing anything.

## Check the server from the phone's network

Open the Immich web UI from a browser on the same Wi-Fi as the phone. If it loads, the server is reachable. If it does not, fix LAN access first; the mobile backup not working symptom here is a connectivity issue, not a photo problem.

## Confirm the upload destination is writable

On the TrueNAS host, check the dataset that holds Immich uploads. Run `df -h` on that path and confirm free space is above a few gigabytes. Then verify the Immich user owns the upload folder, for example `ls -ld /mnt/tank/immich/upload`. If the owner or permissions look wrong, correct them with `chown -R` and `chmod` matching your install, then restart the Immich app.

## Read the first-batch result

In the Immich web UI, open the most recent asset from the phone. If it appears but later photos do not, the first batch succeeded and the second request failed. Check the Immich server logs for errors at that timestamp. A permission denied or no space message points to the dataset fix above; a timeout points to network or reverse-proxy limits.

## Verification test

Create a small album of five photos on the phone and start a backup of only that album. If all five upload and appear in Immich, the pipeline works; resume the full library.

## Rollback note

If you changed ownership or permissions and uploads still fail, restore the previous ownership values you recorded before editing, restart Immich, and retest the five-photo album.

If the stall persists after these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and dataset path.
