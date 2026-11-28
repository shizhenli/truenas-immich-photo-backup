---
title: "Immich Container Restart Loop After a Failed Database Migration"
description: "Fix an Immich container that restarts repeatedly after a failed database migration on TrueNAS. Check logs, verify the database, and restore the web UI."
seo_title: "Immich Database Migration Failed: Stop the Container Restart Loop"
seo_description: "Fix an Immich container that restarts repeatedly after a failed database migration on TrueNAS. Check logs, verify the database, and restore the web UI."
keywords:
  - "immich database migration failed"
date: 2026-11-28T22:25:08+08:00
publishDate: 2026-11-28T22:25:08+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your Immich app container keeps restarting after a database migration failed, and the web UI is unreachable. This guide walks you through a controlled diagnosis to break the loop.

## Check the container logs first

Open the TrueNAS shell and run `sudo docker logs --tail 100 immich_server` (or use the app's log viewer). Look for a line containing **immich database migration failed** or a PostgreSQL error such as `relation ... does not exist` or `migration ... dirty`. If you see a dirty migration state, the database schema is inconsistent and the server will keep exiting.

## Inspect the database state

Connect to the Postgres container: `sudo docker exec -it immich_postgres psql -U postgres -d immich`. Run `SELECT * FROM pg_migration;` (or check the migrations table). A row with `dirty = true` confirms a half-applied migration. Also run `SELECT version();` to ensure Postgres is reachable. If the query fails, the database volume may be corrupted or the password changed.

## Stop the restart loop

Stop the app: `sudo docker stop immich_server`. Then either restore the last known-good database backup or, if you have no backup, manually clear the dirty flag with `UPDATE pg_migration SET dirty = false WHERE dirty = true;` only after confirming the migration did not partially alter tables. Restart the stack and watch the logs for a clean startup.

## Verification test

After restart, run `curl -I http://localhost:2283/api/server/ping`. A `200 OK` response means the API is up. Open the web UI and confirm your photo library loads. If the container still restarts, the migration is still failing.

## Rollback note

If the fix worsens, restore the pre-migration database dump and the matching Immich version. Keep the old container image tag until the new one is stable. For hands-on help,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
