---
title: "Immich App Container Restart Loop on TrueNAS: Diagnose and Recover"
description: "Immich container restarts repeatedly and the web UI is unavailable. Follow these checks to diagnose the loop and restore a stable Immich app container restart."
seo_title: "Fix Immich App Container Restart Loop on TrueNAS"
seo_description: "Immich container restarts repeatedly and the web UI is unavailable. Follow these checks to diagnose the loop and restore a stable Immich app container restart."
keywords:
  - "immich app container restart"
date: 2026-11-16T08:09:01+08:00
publishDate: 2026-11-16T08:09:01+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

Your TrueNAS host is running Immich with a phone photo library, and the container keeps restarting before you can complete a controlled diagnosis.

## Confirm the restart loop

Open a shell on the TrueNAS host and run `docker ps -a --filter name=immich`. Look at the STATUS column. A value like `Restarting (1) 5 seconds ago` or a rising restart count confirms the loop. If STATUS shows `Up`, the problem is elsewhere and you should stop here.

## Read the last exit reason

Run `docker logs --tail 100 immich_server` (adjust the name to your container). Read the final lines before the restart. An exit code 137 means the process was killed, often by memory pressure. A database connection refused message points to the Postgres container, not Immich itself. A permission denied error on the upload path points to the dataset mount. Match the message to one cause before changing anything.

## Check dependencies and resources

Run `docker ps` and confirm the database and Redis containers are running and healthy. If either is restarting, fix that container first. Then run `free -h` on the host; if available memory is near zero, the kernel is likely killing Immich. Reduce concurrent jobs or add memory.

## Verification test

After correcting the cause, run `docker restart immich_server` and watch `docker ps` for two minutes. The STATUS should remain `Up` with a steady uptime, and the web UI should load at your usual address. Open the phone app and confirm a new photo uploads.

## Rollback note

If the loop began after an image update, stop the container and redeploy the previous image tag, then confirm the UI returns before investigating further. If the cause is still unclear,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with the last log lines and exit code. A controlled **immich app container restart** should then stay stable instead of looping.
