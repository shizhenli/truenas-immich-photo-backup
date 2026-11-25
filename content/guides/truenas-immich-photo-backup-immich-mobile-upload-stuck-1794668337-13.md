---
title: "Why Immich Mobile Backup Halts After the First Batch on TrueNAS"
description: "Phone says backup started but Immich stops after the first batch on TrueNAS. Diagnose the stall with server logs, app state, and a controlled re-test."
seo_title: "Immich Mobile Upload Stuck After First Batch: TrueNAS Fix"
seo_description: "Phone says backup started but Immich stops after the first batch on TrueNAS. Diagnose the stall with server logs, app state, and a controlled re-test."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-25T10:09:17+08:00
publishDate: 2026-11-25T10:09:17+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone shows backup running, then the counter freezes after the first batch and nothing else leaves the device.

## Confirm the stall is real
Open the Immich web UI and check the upload activity for your phone's user. If the first batch appears but no new assets arrive after several minutes, the job is genuinely stalled, not just slow. On the TrueNAS host, run `docker logs --tail 100 immich_server` (or your app's log path) and look for repeated errors around the time of the last upload. A loop of timeouts, 413 responses, or connection resets points to the server dropping the connection rather than the phone refusing to send.

## Check the phone side
In the Immich app, open backup settings and note whether foreground or background backup is enabled. Background backup on iOS and Android is frequently suspended by the OS after a short window, which looks exactly like a first-batch stall. Disable background backup, keep the app open and on screen, then start a manual backup. If photos continue past the first batch, the OS suspension was the cause.

## Verify with a controlled test
Add one new photo to the phone, open the app, and trigger a manual backup while watching the server log. A healthy run shows the asset arriving within seconds. If it stalls again at the same point, the issue is server-side.

## Recovery note
If you changed dataset permissions or app settings during diagnosis, revert them before retesting so you isolate one variable. Keep a note of the original values so you can roll back cleanly.

If the log still shows dropped connections after these checks, this **immich mobile upload stuck** behavior usually needs a closer look at the server configuration.

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and app version.
