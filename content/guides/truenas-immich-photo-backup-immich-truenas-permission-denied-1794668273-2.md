---
title: "Immich Upload Denied by Dataset Permissions: Diagnose Before You Change ACLs"
description: "Mobile or web uploads fail with permission denied even though the Immich dataset exists and has free space. Diagnose ownership, ACLs, and mount paths before changing anything."
seo_title: "Immich TrueNAS Permission Denied: Diagnose Dataset Uploads"
seo_description: "Mobile or web uploads fail with permission denied even though the Immich dataset exists and has free space. Diagnose ownership, ACLs, and mount paths before changing anything."
keywords:
  - "immich truenas permission denied"
date: 2026-11-23T11:09:30+08:00
publishDate: 2026-11-23T11:09:30+08:00
draft: false
categories:
  - "Photo Backup"
category_slug: "photo-backup"
---

## Troubleshooting

You are uploading from your phone to Immich on TrueNAS, the dataset exists with plenty of free space, and yet the upload is refused with a permissions error. ## Confirm the failure is really a permission problem First, reproduce it from the web UI and note the exact error text. A storage-full or quota message means you are in the wrong article. Then open the Immich app shell and run `id`. If the app runs as a non-root user such as 1000 or 568, that numeric UID is the one that must be allowed to write. Next, check the host mount path Immich uses for uploads with `ls -ld /mnt/<pool>/<dataset>/`. Read the owner, group, and mode. If the owner is root and the mode is 755, a non-root Immich user cannot create files there, which is the classic cause of **immich truenas permission denied** on upload. Also confirm the path in the app configuration matches the dataset you inspected, not a parent folder. ## Apply the narrowest fix Give the Immich runtime user ownership of the upload directory, or add it to the owning group with group write permission. Prefer a recursive ACL preset only on that single dataset, never on the pool root. ## Verification test Upload one small photo from the phone, then run `ls -l` in the upload directory and confirm a new file appears owned by the Immich UID. If it does, the path is fixed. ## Rollback If the upload still fails, revert the ACL change and capture `id`, `ls -ld`, and the exact error before trying anything else. For a guided review,

![An Immich mobile backup screen and server photo library](/images/service-detail-960.webp "Immich mobile upload and read-back check")

[Ask for help](/#consult).
