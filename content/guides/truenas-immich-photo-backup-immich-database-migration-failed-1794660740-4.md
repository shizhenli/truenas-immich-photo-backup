---
title: "Immich Database Migration Failed: Stop the Restart Loop on TrueNAS"
description: "Immich container restarting repeatedly after a database migration failure on TrueNAS? Follow these checks to read the logs, verify the database, and recover safely."
seo_title: "Fix Immich Database Migration Failed on TrueNAS (Restart Loop)"
seo_description: "Immich container restarting repeatedly after a database migration failure on TrueNAS? Follow these checks to read the logs, verify the database, and recover safely."
keywords:
  - "immich database migration failed"
date: 2026-11-16T09:09:02+08:00
publishDate: 2026-11-16T09:09:02+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your Immich app container restarts and the web UI disappears after a database migration, and you need to break the loop before anything else.

## Read the container log first

Open the TrueNAS Apps screen, select Immich, and view its container logs. Look for the first error before the restart, not the last line. A message containing "migration failed," "relation already exists," or "permission denied" confirms the migration aborted. If the log only shows a health-check timeout, the database may be unreachable rather than corrupt.

## Check the database service

Confirm the PostgreSQL container is running and its dataset is mounted at the expected path. A stopped or read-only database volume causes the migration to fail on every boot. Also check free space on the pool; a full dataset blocks writes and triggers the same loop. If the log shows an authentication error, verify the database password in the app configuration has not changed.

## Verify the failure is migration-specific

Stop the Immich app, then start only the database container. If it stays up, the problem is in the application migration, not the database engine. Restart Immich once and watch the log until it exits. This tells you whether the failure is deterministic.

## Recover without losing photos

Before retrying, snapshot the Immich database dataset. If the migration is partially applied, restore that snapshot and roll back the Immich app to its previous version, then let it start cleanly. Do not delete the database volume unless you have a verified backup; your phone library is separate, but albums and metadata live in the database.

## Verification test

After recovery, open the web UI, confirm the timeline loads, and upload one new photo from your phone. If the container stays up for ten minutes and the upload appears, the **immich database migration failed** loop is resolved. If it restarts again, capture the full log and

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
