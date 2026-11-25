---
title: "Immich App Container Restarting on TrueNAS: Diagnose Before You Rebuild"
description: "Immich container keeps restarting on TrueNAS and the web UI is down? Follow these checks to diagnose the crash loop and recover safely."
seo_title: "Fix Immich App Container Restart Loop on TrueNAS"
seo_description: "Immich container keeps restarting on TrueNAS and the web UI is down? Follow these checks to diagnose the crash loop and recover safely."
keywords:
  - "immich app container restart"
date: 2026-11-25T09:09:24+08:00
publishDate: 2026-11-25T09:09:24+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich app container restart**. Your TrueNAS host runs Immich for a phone photo library, but the container keeps restarting and the web UI is unavailable. Before rebuilding anything, confirm why the container exits.

## Check the container state and exit code

Open the TrueNAS shell and run `sudo docker ps -a --filter name=immich`. Look at the STATUS column: `Restarting (1) 5 seconds ago` means the process exits with code 1, usually a configuration or database error. `Exited (137)` means it was killed, often out of memory. Note the exact code; it directs the next step.

## Read the last logs before the crash

Run `sudo docker logs --tail 100 immich_server`. Scan the final lines before the restart. A message about a missing environment variable, an unreachable database host, or a permission error on the upload folder points to the cause. If the log ends with a stack trace, capture the first error line, not the last.

## Confirm dependencies are healthy

Run `sudo docker ps` and check that the database and Redis containers are up. If either is restarting, Immich will loop too. Also verify the upload dataset is mounted and writable by the container user; a read-only mount causes an immediate exit.

## Verification test

After correcting the identified issue, restart only the Immich container with `sudo docker restart immich_server`. Wait two minutes, then run `sudo docker ps --filter name=immich_server` and confirm the status reads `Up` with no restart count increase. Load the web UI from a browser on the same LAN.

## Rollback and recovery note

If the container still loops, stop it with `sudo docker stop immich_server` to prevent repeated crashes, then restore the last known-good compose file or environment file before retrying. If the cause remains unclear,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the exit code and the first error line from the logs.
