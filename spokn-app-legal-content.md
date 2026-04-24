# Spokn — Privacy Policy & Terms of Service content

Companion to `website-build-prompt.md`. Scoped to **Spokn the iOS dictation app**; other SpoknLabs apps will need their own section or page.

Source: the Spokn app's own `docs/app-store-readiness.md` (in the Spokn repo) §§2–3, which is the App Store submission agenda. That doc lists the publishing deliverables; this doc lists the *content* those pages must cover for Spokn specifically.

---

## Privacy Policy — required content for Spokn

### What Spokn processes
- **Audio recordings** captured via the microphone when the user dictates.
- **Transcription text** returned from the configured engine.
- **Vocabulary hint** — an optional text field of domain terms the user saves in Settings, passed to the transcription engine as a prompt to improve accuracy.
- **Language preference** — an optional ISO-639-1 code the user picks in Settings.

### Where processing happens (dual-engine)
Spokn v3+ runs in one of three modes, user-selectable in Settings:
- **Auto (default):** transcribe with **OpenAI** when the device is online, **on-device** (WhisperKit) when offline.
- **Online only:** always transcribe with OpenAI.
- **Local only:** always transcribe on-device. Audio never leaves the iPhone.

Privacy Policy must cover both paths honestly. When a recording is transcribed via OpenAI, the audio and any vocabulary hint/language fields are sent to OpenAI's `/v1/audio/transcriptions` API using the user's own API key. When a recording is transcribed locally, nothing leaves the device.

### Purpose
- Provide the core dictation service (speech → clipboard text).
- No analytics, telemetry, or marketing processing.

### Retention / deletion
- Audio files are temporary — written to the app's temp directory during recording and deleted immediately after the transcript is delivered or after a failed attempt is abandoned.
- Transcripts are held in memory only for the duration of the app session plus the most recent one (shown in the in-app "Copy again" card). They are not archived, synced, or written to persistent storage.
- No user accounts, no server-side records (Spokn has no backend).
- The OpenAI API key is stored locally in the iOS Keychain; Spokn does not transmit or inspect it beyond using it as the Bearer token in the transcription request.

### Third-party processors
- **OpenAI** — only when Online mode is active or Auto mode routes online. Audio and text fields are sent as documented in OpenAI's API reference. Retention and use are governed by OpenAI's own API data policies (API data is not used to train models by default); link out to OpenAI's policy.

### User rights
- Since Spokn holds no server-side data, there's no account to delete. Uninstalling the app removes all local state (Keychain, UserDefaults, any downloaded on-device model).
- To revoke the OpenAI path, the user deletes their API key in Settings. To revoke the on-device path, they delete any downloaded model in Settings.

### Contact
- A support/privacy contact email (reuse the general SpoknLabs contact channel from `support`).

### App Privacy questionnaire (App Store Connect)
Submission-time work that stays in the Spokn repo, not on the website. The questionnaire must match the content above — notably: "Data Not Collected" is accurate if Local-only mode; under Auto/Online, OpenAI is a third-party processor, not a Spokn data collection.

---

## Terms of Service — required content for Spokn

### Service description
- A personal iPhone dictation utility.
- Two transcription engines: on-device (local) and OpenAI API (BYO key).
- No account, no in-app purchases, no subscription.

### User responsibility for the OpenAI path
- The OpenAI API key is provided by the user; Spokn does not sell, resell, or rebate OpenAI usage.
- All charges accrued via the user's OpenAI account are between the user and OpenAI. Spokn is not a party to that billing relationship.
- Users must comply with OpenAI's own terms when using the OpenAI path.

### Acceptable use
- Personal dictation only — do not record third parties without consent in jurisdictions requiring it.
- No dictation of unlawful, abusive, or policy-violating content via the OpenAI path.

### Warranty disclaimer / limitation of liability
Standard indie-app boilerplate. Transcription quality is not guaranteed; users should not rely on Spokn for legal, medical, or safety-critical transcription.

### Changes
- The terms may change; a dated "Last updated" line in the footer.

### Support contact
- Same contact as the Privacy Policy.

---

## Publishing requirements (interaction with the Spokn app)

- Both URLs must be publicly reachable, stable, and mobile-readable.
- The Spokn app (v3.0+) will add Settings rows linking to both pages — the app ships those hyperlinks, the content lives here.
- For App Store submission metadata, the Privacy Policy URL is required in App Store Connect; Terms is optional at the metadata level but required in-app.

---

## Related files

- **Spokn repo:** `docs/app-store-readiness.md` — the parent publishing agenda. §§2 and 3 of that doc are the reason this file exists.
- **Spokn repo:** `spokn-v3-plan.md` — the v3 execution plan. Used to include a Phase B for legal surface; that has been removed now that authoring lives here. Spokn v3.0 still owns the Settings-row deep-links to the hosted pages.
