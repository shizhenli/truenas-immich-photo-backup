---
title: "Immich Mobile Upload Stuck After the First Batch on TrueNAS"
description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, app permissions, and network to restore uploads."
seo_title: "Fix Immich Mobile Upload Stuck After First Batch on TrueNAS"
seo_description: "Troubleshoot Immich mobile backup that stops after the first batch on TrueNAS. Check server logs, app permissions, and network to restore uploads."
keywords:
  - "immich mobile upload stuck"
date: 2026-11-14T19:50:04+08:00
publishDate: 2026-11-14T19:50:04+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

You're backing up your phone's photo library to Immich on TrueNAS, but the mobile app reports backup started and then stalls after the first batch of photos. This is a common **immich mobile upload stuck** issue, and it usually points to a server-side or network interruption rather than a phone problem.

## Check the Immich server logs
On your TrueNAS host, open the Immich container logs (via the TrueNAS UI or `docker logs immich-server`). Look for errors around the time the first batch completes. If you see repeated timeout or connection reset messages, the server is dropping the upload. If logs show no errors, the phone may be losing connection between batches.

## Verify phone app permissions and background behavior
On the phone, confirm Immich has full photo library access and is allowed to run in the background. On Android, disable battery optimization for Immich; on iOS, enable Background App Refresh. Then force-close and reopen the app. If the app shows a partial backup state, tap resume and watch whether it advances past the first batch.

## Test network stability
Switch the phone to a different Wi-Fi network or temporarily use mobile data. Start a small backup of 5–10 new photos. If it completes, your original network likely has a timeout or firewall rule blocking sustained uploads. Check your TrueNAS firewall or reverse proxy for idle connection limits.

## Verification test
After making changes, add one new photo to your phone and trigger backup. The expected result is that the new photo uploads within a minute and the backup counter increments. If it still stalls, revert any firewall or proxy changes you made and try again.

## Rollback note
If you adjusted reverse proxy timeouts, restore the previous configuration before further testing to avoid introducing new issues. For deeper diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
