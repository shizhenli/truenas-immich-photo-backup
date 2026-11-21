---
title: "Immich Container Restart Loop After a Failed Database Migration on TrueNAS"
description: "Immich container keeps restarting on TrueNAS after a database migration failure? Follow these checks to diagnose the loop, verify recovery, and roll back safely."
seo_title: "Fix Immich Database Migration Failed: Stop the Container Restart Loop"
seo_description: "Immich container keeps restarting on TrueNAS after a database migration failure? Follow these checks to diagnose the loop, verify recovery, and roll back safely."
keywords:
  - "immich database migration failed"
date: 2026-11-21T16:23:39+08:00
publishDate: 2026-11-21T16:23:39+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your Immich app container keeps restarting on TrueNAS, and the web UI is unreachable after an upgrade triggered an **immich database migration failed** condition.

## Confirm the restart loop and read the real error

Open the TrueNAS app logs for Immich and look at the last lines before the container exits. If you see a migration error such as a failed SQL statement, a schema version mismatch, or "migration failed" followed by the container stopping, the app is in a crash loop. A healthy start should log that migrations are complete and then show the server listening. If the log repeats the same failure on every restart, stop the app now so you can diagnose without further churn.

## Check the database and version state

Verify the Postgres container is running and reachable from the Immich container. Check that the database volume is mounted and has free space. Confirm the Immich image tag matches the expected release; a partial upgrade or a tag pointing to a newer major version can leave the schema half-migrated. If the database reports a migration table entry for a version that does not match the app, that confirms the mismatch.

## Verification test

After correcting the version or restoring the database, start the app and watch the logs until migrations finish. Then open the web UI and load the photo timeline. A successful test shows the login page and existing assets without restart messages.

## Rollback and recovery

If the migration cannot complete, stop the app and restore the database from the backup taken before the upgrade, then pin the Immich image to the previous working tag. Do not delete the database volume. If you need guided recovery,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
