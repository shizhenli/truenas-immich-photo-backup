---
title: "Immich App Container Restarting on TrueNAS: Diagnose the Loop Before It Worsens"
description: "Troubleshoot an Immich container that restarts repeatedly on TrueNAS, making the web UI unavailable. Check logs, resources, and config, then verify recovery."
seo_title: "Fix Immich App Container Restarting on TrueNAS"
seo_description: "Troubleshoot an Immich container that restarts repeatedly on TrueNAS, making the web UI unavailable. Check logs, resources, and config, then verify recovery."
keywords:
  - "immich app container restart"
date: 2026-11-23T08:09:19+08:00
publishDate: 2026-11-23T08:09:19+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

**immich app container restart**. You are running Immich on TrueNAS with a phone photo library, and the container keeps restarting, leaving the web UI unreachable.

## Confirm the restart pattern

Open the TrueNAS Apps screen and select the Immich app. Check the container status and restart count. A healthy container shows a stable running state; a rising restart count within minutes confirms a loop. Next, open the container logs and look for the last lines before each restart. A clean shutdown message points to a configuration or dependency issue, while an out-of-memory kill or missing file error points to resources or storage.

## Check resources and storage

On the TrueNAS dashboard, review memory and CPU usage. If memory sits near the limit during startup, the container is being killed and restarted. Stop the Immich app, then confirm the dataset holding Immich data is mounted and writable. A missing or read-only mount causes startup to fail repeatedly.

## Inspect configuration

In the app settings, verify environment variables such as database connection details and upload location. A wrong database host or password makes Immich exit on boot. Compare against your notes or a previous working configuration before changing anything.

## Verification test

After correcting one issue at a time, start the app and watch the logs for two minutes. Success means the log shows the server listening and the restart count stays fixed. Open the web UI from a LAN device to confirm it loads.

## Rollback and recovery

If changes make things worse, stop the app and restore the previous configuration or a recent snapshot of the Immich dataset. Then restart and recheck the logs. If the loop persists,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult) with your log excerpt and restart count. This controlled diagnosis prevents repeated restarts from corrupting uploads and keeps your photo library safe.
