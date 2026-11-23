---
title: "Immich Mobile Backup Stops After the First Batch on TrueNAS: A Focused Fix"
description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check server logs, storage, and app state, then verify with a controlled test."
seo_title: "Immich Mobile Backup Not Working: Fix First-Batch Stall on TrueNAS"
seo_description: "Troubleshoot Immich mobile backup stopping after the first batch on TrueNAS. Check server logs, storage, and app state, then verify with a controlled test."
keywords:
  - "immich mobile backup not working"
date: 2026-11-23T12:09:32+08:00
publishDate: 2026-11-23T12:09:32+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but the progress bar freezes after the first batch and no new photos appear in Immich on your TrueNAS host.

## Confirm the stall point
Open the Immich mobile app and note the last uploaded asset timestamp. On TrueNAS, check the Immich server logs for the same window. If the logs show the first batch completing and then no further upload requests, the phone is not sending new batches. If the logs show repeated errors on the first batch, the server is rejecting them.

## Check the three common causes
First, verify free space on the dataset backing Immich. A dataset at 100 percent will accept the first batch and then fail silently. Second, confirm the phone is still on the same LAN and that the Immich server URL resolves; a dropped Wi-Fi connection mid-backup looks exactly like this. Third, in the Immich app, check that background backup is allowed and that the battery optimization setting is not pausing the app after the first batch.

## Run a verification test
Force-stop the Immich app, reopen it, and start backup manually while watching the server logs. If the first batch uploads and the second begins within a minute, the stall is resolved. If it stops again at the same point, capture the log lines from that moment before changing anything else.

## Rollback and recovery
If a recent change to the Immich container or dataset permissions preceded the stall, revert that change and restart the container. Do not delete uploaded assets; Immich will resume from the last successful batch. For persistent first-batch stalls that survive these checks,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt so the **immich mobile backup not working** issue can be diagnosed against your specific TrueNAS setup.
