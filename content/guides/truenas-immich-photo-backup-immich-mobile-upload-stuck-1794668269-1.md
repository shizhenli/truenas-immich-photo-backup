---
title: "Immich Mobile Upload Stuck: Why the First Batch Stalls and How to Clear It"
description: "Troubleshoot an Immich mobile backup that stops after the first batch on TrueNAS. Check server-side job state, app cache, and network, then verify with a small test batch."
seo_title: "Immich Mobile Upload Stuck: Fix the First-Batch Stall on TrueNAS"
seo_description: "Troubleshoot an Immich mobile backup that stops after the first batch on TrueNAS. Check server-side job state, app cache, and network, then verify with a small test batch."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-23T10:09:25+08:00
publishDate: 2026-11-23T10:09:25+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch of photos the progress simply stops, and you want to know whether the server or the app is holding the queue.

## Check the server-side job first
Open Immich on the TrueNAS host and look at the job queue under administration. If the upload job shows as active but no new assets appear, the server is waiting on a stalled worker rather than receiving new files. If the queue is empty while the phone still shows progress, the phone has not handed off the next batch. Note the exact state before changing anything.

## Clear the phone-side queue
On the phone, force-stop the Immich app, then reopen it and check the backup status. A common cause of an **immich mobile upload stuck** at the first batch is a stale foreground upload session that never releases. If the app still shows the same batch, clear the app cache (not the data) and retry. Do not delete the app data unless you have confirmed the server already holds those assets.

## Rule out the network handoff
Confirm the phone is on the same LAN as the TrueNAS host and that the server address in the app resolves. Switch off any VPN on the phone for one test. If the first batch completes only when the VPN is off, the tunnel is dropping the upload connection between batches.

## Verification test
Create a small album of five new photos on the phone and start a backup of only that album. Watch the server job queue while it runs. Success means all five appear as assets and the job returns to idle. If it stalls again at the same point, the problem is server-side scheduling, not the phone.

## Rollback note
If clearing the cache or restarting the app makes things worse, stop and restore the previous app state before retrying; do not wipe app data on a library you have not verified on the server. For a guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
