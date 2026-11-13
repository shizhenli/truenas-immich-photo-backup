---
title: 'Immich Phone Photos Not Backing Up: Client, Permission, Server'
description: A three-layer Immich backup test covering mobile permissions, server storage, and a read-back sample.
seo_title: Immich Not Backing Up Phone Photos? Check These Layers
seo_description: A three-layer Immich backup test covering mobile permissions, server storage, and a read-back sample.
keywords:
- immich not backing up photos
- immich mobile upload stuck
- truenas immich backup
date: '2026-11-13'
draft: false
categories:
- Photo Backup
category_slug: photo-backup
---

<p>Immich upload failures can be on the phone, in the container, or on the TrueNAS dataset. Test one photo through all three layers.</p>
<h2>Before you start</h2><ul>
<li>Phone with the Immich app</li>
<li>Access to the Immich web library</li>
<li>TrueNAS access for dataset/container status</li>
<li>One new test photo</li>
</ul>
<h2>Step-by-step checks</h2>
<h3>Step 1: Confirm the server and account</h3>
<p>Open the Immich app settings and record the server URL and signed-in user. Open the same URL in a browser and confirm the library belongs to that user.</p>
<p><strong>Check:</strong> The phone and browser show the same server and account.</p>
<h3>Step 2: Allow background upload</h3>
<p>Grant photo access, background refresh, and mobile data if the test requires it. Disable battery optimization for Immich during the test.</p>
<p><strong>Check:</strong> The app's backup screen shows the test album enabled and no permission warning.</p>
<h3>Step 3: Check server capacity</h3>
<p>Confirm the TrueNAS dataset has free space and the Immich containers are running. Check the server logs for upload or storage errors.</p>
<p><strong>Check:</strong> The dataset has free space and no container restart loop.</p>
<h3>Step 4: Upload one new photo</h3>
<p>Take a new photo, open Immich, and wait for the upload queue to clear. Note the asset's timestamp and album.</p>
<p><strong>Check:</strong> The phone queue shows the asset completed.</p>
<h3>Step 5: Read it back from the server</h3>
<p>Open the photo in the Immich web UI, confirm the date and album, and download a copy. Compare it with the phone original.</p>
<p><strong>Check:</strong> The server copy opens and downloads correctly.</p>
<h2>Verify the result</h2>
<p>A new phone photo appears in the web library, matches the original, and downloads from the server after the phone app is closed.</p>
<h2>Rollback</h2>
<p>Remove only the test asset if needed. Do not delete the phone original or existing server photos during verification.</p>
<p><a href="/#consult">Ask for help with this exact symptom</a> · <a href="/guides/">All troubleshooting guides</a></p>
