# US-003 Target Android API 36

## Status

implemented

## Lane

normal

## Product Contract

Android release builds compile against and target Android 16 (API level 36) so new releases satisfy the Google Play target API requirement.

## Relevant Product Docs

- `README.md`
- `docs/ARCHITECTURE.md`

## Acceptance Criteria

- Android `compileSdk` is explicitly set to API 36.
- Android `targetSdk` is explicitly set to API 36.
- AGP and Gradle versions support compiling against API 36.
- Android CI caches the API 36 SDK platform.
- A non-uploading Gradle validation succeeds.

## Design Notes

- Keep `minSdk` at 23; the Play requirement only changes the target API.
- Keep NDK `27.0.12077973`; it remains compatible with the selected AGP version.
- Pin API 36 explicitly instead of depending on the Flutter SDK defaults.

## Validation

| Layer | Expected proof |
| --- | --- |
| Unit | Not applicable; no Dart behavior changes. |
| Integration | Gradle resolves the Android application tasks with AGP 8.10.1 and Gradle 8.11.1. |
| E2E | Not required for this configuration-only change. |
| Platform | Android configuration reports `compileSdk = 36` and `targetSdk = 36`. |
| Release | A new AAB with a higher `versionCode` must be uploaded and published separately. |

## Harness Delta

No Harness behavior changed. The project-scoped intake skill referenced by `AGENTS.md` was missing, so the documented Harness intake flow was used directly.

## Evidence

- `scripts/bin/harness-cli story verify US-003` passed on 2026-08-06.
- Gradle task `:app:processDebugMainManifest` completed successfully with AGP 8.10.1 and Gradle 8.11.1.
- Generated debug merged manifest reports `android:targetSdkVersion="36"`.
- No AAB was built or uploaded during this change.
