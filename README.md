# ForgeMedia

**Native Android catalogue and video player — phone, tablet and Android TV.**

ForgeMedia browses movies and TV from [TMDB](https://www.themoviedb.org/),
resolves playable sources through its built-in provider catalogue, and plays
them entirely on-device with Media3/ExoPlayer. No local server, no desktop
runtime, no browser dependency — one APK, written in Kotlin with Jetpack
Compose and Material 3.

---

## Download

**ForgeMedia 1.2.70** — versionCode 71

- [**Download `ForgeMedia.apk`**](https://github.com/Tigstar82/ForgeMedia_Release/releases/download/v1.2.70/ForgeMedia.apk)
- [All releases](https://github.com/Tigstar82/ForgeMedia_Release/releases)

| | |
|---|---|
| File | `ForgeMedia.apk` |
| Size | 13943210 bytes |
| SHA-256 | `263e8a834e2b9da3dcf667586e7770a8c8c0cb5bab0c16f3d92bb07ba4ada2e9` |
| Requires | Android 8.0 (API 26) or newer |

The same APK covers phones, tablets and Android TV: both the standard and the
TV leanback launcher are declared, and touch is optional, so a remote and
D-pad work everywhere in the app.

---

## What's in it

**Browse**

- Trending, popular and top-rated movies and TV shows
- Search, genre filtering and sorting
- Detail pages with overview, release status, seasons and episodes
- Watch-List with tags, ratings, watched state, per-episode progress,
  mark-all-watched and CSV export

**Watch**

- On-device playback of HLS, DASH and progressive streams
- In-app WebView playback for sources that don't expose a direct media URL
- One-click source resolution with automatic fallback when a source fails
- Resume — a saved position is restored when you come back to a title
- Subtitles and audio-track settings, seeking, next/previous title or episode
- Progress saved as you watch, so a series continues where you left off
- FFmpeg audio decoding (EAC3, AC3, DTS, TrueHD) when the platform decoder
  can't handle the stream

**Keep it healthy**

- Provider catalogue with per-site status and health checks
- Optional signed provider updates: an Ed2519 signature, a trusted key and a
  newer manifest version must *all* validate, otherwise the bundled
  definitions stay active rather than a rejected update being applied
- Background monitor re-checks twice daily
- Self-update, described below

**Metadata key** — titles come from TMDB. If your build doesn't already carry
a key, open **Providers → TMDB_API_KEY** and paste one; it is stored on the
device only.

---

## Updating an installed app

You no longer need to look. ForgeMedia checks its own releases twice a day and
tells you when a new one is published, two ways:

- **A notification**, if you allow notifications. It appears while the app is
  closed; tapping it opens ForgeMedia. On Android 13 and later you are asked
  once, from **Providers → App updates → Check for updates**.
- **A prompt inside the app** on launch, which works even if you decline
  notifications. It only checks for the version — nothing downloads unless
  you press the button. **Not now** is remembered for that one version, so the
  next release still asks you.

**Providers → App updates** still shows the same information on demand, and
**Download & install** starts the update immediately.

A check costs two small JSON requests — the APK is never downloaded just to
find out whether you're current. Before anything is installed, the download is
verified for HTTPS on a GitHub host, declared size, SHA-256, package name, a
`versionCode` that is not a downgrade, and a signing certificate in the
installed app's lineage. Android then asks you to confirm.

---

## Recent changes

**1.2.70**
- Release engineering only — no behaviour change. The published APK, this
  README and the SHA-256 below are now guaranteed to come from one and the
  same build, and the source is checked out with the same line endings on every
  machine, so a rebuilt APK no longer changes its own hash for that reason.

**1.2.69**
- Fixed the TV tab still showing movie posters after you switched to it. The
  grid was the old Movies list with the new TV page appended — 117 items after
  a single switch. Fresh loads now carry a generation, so a late response or a
  cancelled load from the previous tab can no longer publish into the new one.

**1.2.67 – 1.2.68**
- Fixed signed provider manifests being rejected on **release** builds below
  Android 13, which silently left those devices on the two-row emergency embed
  table while reporting nothing. The bundled BouncyCastle provider was being
  stripped of its Ed25519 algorithms; manifests are now also checked on launch
  and by the background worker.
- Fixed the in-app update refusing this app's own published release on some
  devices with "APK signing key does not match this install". The check now
  tries every route the platform offers and names the failure that actually
  occurred instead of inventing one.

**1.2.66**
- Fixed the provider manifest URL fields rendering blank, and "Save & check now"
  writing that blank back and clearing the URL you had configured — the app then
  quietly fell back to its bundled providers with no error shown.

**1.2.62**
- Fixed the catalogue not following the media tab. Tapping **Movies** while
  browsing TV shows used to leave the TV catalogue on screen under a
  "Movies"-highlighted tab, silently and with no error.
- Fixed the catalogue freezing partway through a scroll instead of loading
  more as you reached the end.
- Finishing the newest episode of a series that is still being made no longer
  tries to play an episode that has not aired yet, which ended on a confusing
  "no playable sources" message. It now says when the next episode airs.
- Post-render playback error handling moved into a single tested policy, and
  player state and stream errors are now visible in release builds.

**1.2.61**
- Watching one episode no longer marks a whole series as watched. A season now
  reports what you actually watched (for example `3/7 watched`) instead of
  treating the first episode as the whole season.
- Removing a title from your watchlist no longer destroys its resume point.

**1.2.63 – 1.2.64**
- Added the update notification and the in-app prompt described above.
- 1.2.64 announces an update as soon as it is detected rather than after the
  whole download, so a slow connection no longer means you are never told.

**1.2.65** is a verification build with no functional change from 1.2.64.

---

## Installing

1. Download `ForgeMedia.apk` from the link above, or let the app update itself.
2. Allow installs from unknown sources for your browser or file manager when
   Android prompts.
3. Open the APK. An existing install upgrades in place. Android refuses
   downgrades, so installing an older build needs an uninstall first.

## Verifying the download

Each release also carries `ForgeMedia.json`, whose `apk_size` and `apk_sha256`
are what the updater checks before installing anything. Confirm the file the
same way:

    Get-FileHash .\ForgeMedia.apk -Algorithm SHA256

Lowercase the result and compare it against `apk_sha256` in
`ForgeMedia.json`. Any mismatch — don't install.

---

## About this repository

The source repository is private. GitHub answers unauthenticated release
lookups against a private repository with a 404, and the app's updater
deliberately sends no `Authorization` header, so releases are published here
instead. This repository contains no source code — only this README and the
release assets.

Use ForgeMedia only with media sources you are authorized to access.

---

*Generated by `writeForgeMediaReleaseMeta` from the build that produced the
APK, so the version block above cannot drift from the release it describes.
Do not edit it by hand — edit `release/README.template.md` instead.*
