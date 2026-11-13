---
title: TrueNAS Immich Photo Backup
seo_title: TrueNAS Immich Photo Backup Help
seo_description: Remote help when Immich stops backing up phone photos, with client, permission, storage, and read-back checks.
keywords:
- immich not backing up photos
- immich mobile upload stuck
- truenas immich backup
hero_kicker: TRUENAS IMMICH BACKUP
hero_h1: Immich stopped backing up phone photos?
hero_lead: Check the Immich mobile client, background permissions, server storage, and one sample upload before trusting the backup.
hero_proof:
- One phone and one Immich account
- Background permission and queue checked
- Server copy opened and downloaded
hero_alt: NAS, phone, and monitor showing an Immich photo library
hero_title: Immich photo library across NAS, phone, and monitor
body_alt: An Immich mobile backup screen and server photo library
body_title: Immich mobile upload and read-back check
body_caption: A successful test ends with the photo readable from the server, not just queued on the phone.
failures_kicker: THE THREE FAILURES WE SEE
failures_heading: What is going wrong?
test_kicker: HOW WE TEST
test_heading: Find the failing layer, then prove the fix.
scope_kicker: SERVICE SCOPE
scope_heading: What is included
scope_note: The session covers one agreed device and one verified path. Anything outside the written scope is quoted separately before work starts.
faq_kicker: FAQ
faq_heading: Before you book
consult_heading: Tell us what is happening.
consult_lead: Send the device, software, exact symptom, and what you have already tried. We will confirm scope and a fixed quote before booking.
form_placeholder: Device, software version, exact symptom, and what you have tried.
first_guide_url: /guides/truenas-immich-photo-backup/
symptoms:
- title: New photos stay queued
  text: The phone shows pending uploads that never complete, even on Wi-Fi.
- title: Only some albums upload
  text: Selected albums or recent photos sync while the rest remain local.
- title: The server copy is missing
  text: The client says backed up, but the photo is absent from the Immich web library.
checks:
- title: Confirm the client and account
  text: Check the phone is signed into the intended Immich server and user. A second account can make the library look empty.
- title: Check background permissions
  text: Allow photos, background refresh, and mobile data as required. Battery optimization can stop uploads when the app is closed.
- title: Check server storage
  text: Confirm the TrueNAS dataset has free space and the Immich containers are healthy. A full dataset or failed mount stops uploads.
- title: Upload and read back one photo
  text: Take a new photo, wait for the upload, open it in the web UI, and download it from the server.
included:
- One phone and one Immich account
- Client permissions and queue review
- Server storage/container health check
- One photo upload and read-back
excluded:
- Recovering deleted photos without backups
- Buying storage hardware
- Immich feature development
- Guaranteeing every album or RAW format
faqs:
- q: Does “backed up” mean the file is safe?
  a: Only after the server copy is visible and readable. The client status is a queue state, not proof of a recoverable file.
- q: Why do uploads stop when the app closes?
  a: Mobile OS background limits and battery optimization can pause the upload. The permission and queue settings show whether the client is allowed to continue.
- q: Can you recover missing photos?
  a: If the only copy was deleted from the phone and never reached the server, recovery is outside this service. We verify the current backup path.
---
