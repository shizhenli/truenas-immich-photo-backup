---
title: "Immich App Container Restart Loop on TrueNAS: Diagnose the Crash Before It Wipes Your Uploads"
description: "Stop the Immich container from restarting repeatedly on TrueNAS. Check logs, resources, and storage, then verify recovery without losing your phone photo library."
seo_title: "Fix Immich App Container Restart Loop on TrueNAS"
seo_description: "Stop the Immich container from restarting repeatedly on TrueNAS. Check logs, resources, and storage, then verify recovery without losing your phone photo library."
keywords:
  - "immich app container restart"
date: 2026-11-18T09:09:18+08:00
publishDate: 2026-11-18T09:09:18+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich app container restart**. Your phone photos are safe, but the Immich web UI keeps disappearing because the container is stuck in a restart loop.

## Confirm the loop and capture the first failure

Open the TrueNAS Apps screen and watch the Immich workload for two minutes. If the state flips between running and restarting more than twice, you have a loop, not a slow start. Next, open the container logs and scroll to the very first error after the last start, not the repeated tail. A message about a missing database, permission denied on a mount, or an out-of-memory kill tells you which subsystem to inspect. If logs show a clean startup followed by silence, check the host memory graph; a spike to the limit points to memory pressure.

## Check storage and resources

Verify the dataset backing Immich is mounted and has free space. A full pool or a read-only dataset causes the app to exit during startup. Confirm the database container is healthy before the app container; if the database is still initializing, the app will keep cycling. Check that the app's environment still points to the correct database host and credentials. If you recently changed a mount path or permission, revert that change first, then restart the app once and observe.

## Verification test

After correcting the cause, start the app and wait five minutes without touching it. The container should stay running, the web UI should load, and a test upload from your phone should appear in the timeline. If it restarts again, stop and re-read the first error rather than restarting repeatedly.

## Rollback and recovery

If the loop began after an update, roll back to the previous app version and confirm the UI returns. Keep your photo dataset untouched during rollback so no uploads are lost. For guided diagnosis,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
