---
title: "Immich Database Migration Failed: Stop the Container Restart Loop on TrueNAS"
description: "Immich container restarting repeatedly after a database migration failure? Follow these checks to diagnose the loop and restore the web UI."
seo_title: "Fix Immich Database Migration Failed on TrueNAS"
seo_description: "Immich container restarting repeatedly after a database migration failure? Follow these checks to diagnose the loop and restore the web UI."
keywords:
  - "immich database migration failed"
date: 2026-11-23T09:09:15+08:00
publishDate: 2026-11-23T09:09:15+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your Immich container on TrueNAS keeps restarting, the web UI is unreachable, and you suspect an **immich database migration failed** during startup. Let's stop the loop and check the database state before changing anything.

## Check the container logs for the migration error

Open the TrueNAS Apps screen, select Immich, and view the container logs. Look for lines mentioning migration, schema, or a specific SQL error. If the last lines repeat the same migration number and then the container exits, the database is stuck at that migration. Note the exact error text; a permissions error points to the Postgres data directory, while a duplicate column or relation error means a partial migration was applied.

## Confirm Postgres is healthy and reachable

In the Immich app, check that the Postgres container is running and not restarting. From a shell on the TrueNAS host, test the connection with `pg_isready -h <postgres-host> -p 5432`. A result of accepting connections means the database is up; no response means the database container or its dataset is the problem, not Immich itself.

## Inspect the migration state

Connect to the database with `psql` and run `SELECT * FROM migrations ORDER BY id DESC LIMIT 5;`. Compare the latest entry with the migration named in the Immich logs. If the log names a later migration than the table shows, Immich will retry it on every start. Do not delete rows or tables yet.

## Verification test

Stop the Immich app, start it again, and watch the logs for two minutes. If the container stays up and the web UI loads, the migration completed. If it restarts again with the same error, the database needs manual recovery.

## Rollback and recovery

Restore the Postgres dataset from the snapshot taken before the upgrade, then pin Immich to the previous image tag. If no snapshot exists,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) before editing the database directly.
