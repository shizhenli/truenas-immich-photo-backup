---
title: "Immich Mobile Backup Stalls After the First Batch on TrueNAS"
description: "Fix Immich mobile backup that stops after the first batch on TrueNAS. Check server reachability, app permissions, and storage, then verify with a small test album."
seo_title: "Immich Upload Stuck After First Batch: Mobile Backup Fix"
seo_description: "Fix Immich mobile backup that stops after the first batch on TrueNAS. Check server reachability, app permissions, and storage, then verify with a small test album."
keywords:
  - "immich upload stuck"
date: 2026-11-21T16:26:40+08:00
publishDate: 2026-11-21T16:26:40+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your phone says backup started, but after the first batch of photos the progress freezes and nothing else uploads. This is a common **immich upload stuck** symptom on a TrueNAS host, and it usually points to a resource or permission limit that only appears once the first batch finishes.

## Check the server is still reachable
Open the Immich web UI from a browser on the same LAN. If it loads, the server is up. If it does not, the mobile app may have lost its connection after the first batch. Confirm the phone and TrueNAS are on the same subnet and that no firewall rule blocks the Immich port after initial traffic.

## Check the mobile app's upload settings
In the Immich mobile app, open Settings > Backup. Confirm background backup is enabled and that the app has photo library permission set to allow all photos, not selected photos. If permission is limited, the app stops after the first batch because it cannot read the remaining files.

## Check storage and dataset permissions
On TrueNAS, open the Immich dataset and confirm free space is available. Then check that the Immich user owns the upload and thumbnail folders. A dataset that fills or loses write permission will accept the first batch, then reject later writes.

## Verification test
Create a small album on the phone with five new photos and start a manual backup. Watch the Immich web UI activity. If all five appear, the stall is resolved. If it stops again, note the exact photo count and check the Immich server logs for permission or disk errors at that moment.

## Rollback and recovery
If a recent permission or dataset change caused the stall, revert that change in TrueNAS, restart the Immich app, and retry the five-photo test. Do not delete the existing library; the first batch is already stored and will remain intact.

If the test still stalls,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your TrueNAS version, Immich version, and the photo count where it stops.
