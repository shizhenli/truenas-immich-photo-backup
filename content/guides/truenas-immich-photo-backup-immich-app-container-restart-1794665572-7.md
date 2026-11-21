---
title: "Immich App Container Restart Loop on TrueNAS: Diagnose Before You Reboot"
description: "Stop an Immich container restart loop on TrueNAS. Check logs, database, and storage, then verify recovery and roll back safely."
seo_title: "Immich App Container Restart Loop on TrueNAS: Fix Repeated Restarts"
seo_description: "Stop an Immich container restart loop on TrueNAS. Check logs, database, and storage, then verify recovery and roll back safely."
keywords:
  - "immich app container restart"
date: 2026-11-21T16:23:39+08:00
publishDate: 2026-11-21T16:23:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your TrueNAS host runs Immich for a phone photo library, and now the web UI is unavailable because the container keeps restarting. You are stuck at the **immich app container restart** step and need a controlled diagnosis before changing anything.

## Check the restart reason in logs

Open the Immich app logs on TrueNAS and read the last lines before each restart. Look for a specific exit reason: database connection refused, permission denied on a dataset path, out of memory, or a missing environment variable. If the log ends with a database connection error, the app is starting before PostgreSQL is ready or the database volume is unavailable. If it shows permission denied, note the exact path. Do not restart the app repeatedly; each cycle overwrites useful context.

## Confirm database and storage health

Verify the PostgreSQL container is running and stable. A database that restarts on its own will drag Immich into a loop. Check that the dataset holding the Immich upload and database volumes is mounted and has free space. Confirm the app's storage paths match the actual dataset names. If the database volume was recreated or emptied, Immich may fail during migration and exit.

## Verification test

After correcting one cause, start the app once and watch the logs until you see the server listening message. Then open the web UI from a browser on the LAN. Success means the login page loads and stays available for at least five minutes. If the container restarts again, stop and capture the new log lines rather than repeating the start.

## Rollback and recovery

If a recent configuration or version change caused the loop, restore the previous app configuration or snapshot the database and upload datasets before retrying. Keep the failed logs for reference.

If the cause remains unclear,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the log excerpt and your dataset layout.
