# ForgeMedia_Release

Public distribution repository for the ForgeMedia Android app.

This repository exists for exactly one reason: the source repository is
private, and GitHub answers unauthenticated `GET /releases/latest` requests
against a private repository with a 404. The app deliberately sends no
`Authorization` header — an APK-fetching client with an embedded token would
be a credential shipped in an APK — so its in-app updater reads its release
feed from somewhere anonymous GETs can reach.

Each release carries exactly two assets:

- `ForgeMedia.apk` — the release-signed app build
- `ForgeMedia.json` — sidecar describing that build (`version_name`,
  `version_code`, `apk_size`, `apk_sha256`), so a periodic update check can
  compare versions with two small JSON requests instead of downloading the
  whole APK

There is no source here and none is expected. Do not build from this
repository; it holds published binaries only.
