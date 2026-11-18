---
title: "Immich Container Restarts Repeatedly After a Failed Database Migration"
description: "Troubleshoot an Immich container that restarts repeatedly after a failed database migration on TrueNAS, with checks, verification, and recovery steps."
seo_title: "Fix Immich Database Migration Failed on TrueNAS (Restart Loop)"
seo_description: "Troubleshoot an Immich container that restarts repeatedly after a failed database migration on TrueNAS, with checks, verification, and recovery steps."
keywords:
  - "immich database migration failed"
date: 2026-11-18T10:35:35+08:00
publishDate: 2026-11-18T10:35:35+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your TrueNAS host runs Immich with a phone photo library, and after an update the Immich app container restarts repeatedly while the web UI stays unavailable.

## Confirm the restart loop and read the real error

Open the TrueNAS apps screen and check the Immich container state. If it shows restarting or exited with a rising restart count, the loop is confirmed. Next, open the container logs and scroll to the last lines before the exit. Look for a migration error such as a failed SQL statement, a missing column, or a version mismatch between the Immich server and its Postgres database. If the log ends with a migration failure, stop restarting the app so the error stays readable.

## Check database reachability and version

Verify the Postgres container is running and healthy, and that the Immich server can resolve its hostname. Then check the database schema version against the Immich release notes. A server image newer than the database schema, or a migration interrupted midway, is the usual cause of an **immich database migration failed** loop. If the database is reachable but the schema is behind, the migration did not complete.

## Verification test

Start the stack once and watch the logs until the migration step finishes or fails. A successful run logs the migration as applied and the container stays up for several minutes. If it exits again at the same statement, the failure is reproducible and needs a restore.

## Rollback and recovery

Stop the app, restore the Postgres dataset from the snapshot taken before the update, and pin the Immich image to the previous working tag. Start the stack and confirm the web UI loads. Keep the failed database copy aside for inspection. If the cause is unclear,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) before retrying the upgrade.
