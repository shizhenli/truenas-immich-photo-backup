---
title: "Immich Phone Backup Not Starting: Check These Four Layers"
description: "A practical first-pass check for TrueNAS Immich Photo Backup."
date: 2026-11-13
draft: true
---
<p>Immich backup failures usually sit between the phone, the reverse proxy, the app container, and storage permissions. I verify each layer.</p>
<h2>1. Reproduce the exact failure</h2>
<p>Write down what works, what does not, and the last change. Test from the same device and network where the problem happens. A second device is useful only as a comparison, not as proof.</p>
<h2>2. Check the layer before the app</h2>
<p>Confirm the host is reachable, the account or device is allowed, and the local client can see the service. Then test the app setting. Changing three layers at once makes the result impossible to interpret.</p>
<h2>3. Make one change and retest</h2>
<p>Record the old value, apply one change, retest, and keep the result. If the change does not fix it, roll it back before trying the next path.</p>
<h2>4. Handover test</h2>
<p>Repeat the successful test after a restart or reconnect. A fix that only works in the current session is not ready for handover.</p>
<p><a href="/">Back to the service page</a> 路 <a href="mailto:hello@handytasklab.com?subject=TrueNAS%20Immich%20Photo%20Backup%20question&body=Source%3A%20truenas-immich-photo-backup">Ask about this exact symptom</a></p>

