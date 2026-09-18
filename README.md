# MiCodes organization workflows

Central reusable CI/CD for MiCodes projects.

## `android-release.yml`

Release pipeline for Android apps: on a `vX.Y.Z` tag it derives the version,
decodes the keystore, builds the signed AAB (and optional APK), uploads to
**Google Play** and creates a **GitHub Release**.

### Usage

In a project, replace `.github/workflows/release.yml` with:

```yaml
name: Release to Play Store

on:
  push:
    tags:
      - "v*"

jobs:
  release:
    uses: MiCodesOrg/.github/.github/workflows/android-release.yml@main
    with:
      package_name: fr.micodes.example
    secrets: inherit
```

### Inputs

| Input | Default | Description |
|---|---|---|
| `package_name` | — (required) | Play Console application id |
| `java_version` | `17` | JDK version |
| `gradle_command` | `./gradlew :app:bundleRelease` | Build command |
| `aab_path` | `app/build/outputs/bundle/release/app-release.aab` | AAB to publish |
| `apk_path` | *(empty)* | Optional APK to attach to the GitHub Release |
| `mapping_path` | `app/build/outputs/mapping/release/mapping.txt` | Optional R8 mapping |
| `play_track` | `internal` | Play track |
| `environment` | `PROD` | GitHub environment (must exist in the caller repo) |

### Secrets

Either at organization level (shared to the repos) or per repository:

`KEYSTORE_BASE64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`, `SERVICE_ACCOUNT_JSON`.

### Project requirements

The app's Gradle build must read the version and signing from the environment:

- `VERSION_CODE`, `VERSION_NAME`
- `KEYSTORE_FILE`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`
