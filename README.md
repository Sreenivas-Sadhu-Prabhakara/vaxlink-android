# VaxLink Android app

Ships the live VaxLink web app (<https://vaxlink-ai.web.app>) as an installable Android app, and hosts
the install page.

## Install

- **Install page + APK:** <https://sreenivas-sadhu-prabhakara.github.io/vaxlink-android/>
- The app isn't on the Play Store; sideload the APK (allow installs from the source when Android asks).
- Installed an earlier build? Uninstall it first — the current release is signed with a new key.

## What ships: a Trusted Web Activity (TWA)

The installable app (`twa/`, v2.0) is a **Trusted Web Activity**: it runs the live site in the device's
**Chrome engine**, full-screen, verified against the domain via Digital Asset Links. Because it is real
Chrome — not a WebView — **Sign in with Google works**, alongside mobile OTP (employees) and email +
password (HR / clinic). Design and behaviour are identical to the website and update automatically.

Verification: the app declares `asset_statements` for `https://vaxlink-ai.web.app`, and the site serves
`/.well-known/assetlinks.json` listing this app's package (`ai.vaxlink.app`) and signing-cert SHA-256.

### Build the TWA

Requires JDK 17, the Android SDK (build-tools 34, `android-34`), and a signing key in a gitignored
`twa/keystore.properties` (`storeFile`, `storePassword`, `keyAlias`, `keyPassword`).

```sh
cd twa
JAVA_HOME=/path/to/jdk-17 ANDROID_HOME=/path/to/android-sdk ./gradlew assembleRelease
# → twa/app/build/outputs/apk/release/app-release.apk
```

Get the signing SHA-256 for `assetlinks.json`:
`keytool -list -v -keystore twa/vaxlink-release.keystore -alias vaxlink`.

## Also here: the Capacitor build (`android/`)

`android/` holds an earlier **Capacitor** WebView wrapper. It builds an installable APK too, but Google
blocks OAuth in embedded WebViews, so Google sign-in is hidden there — which is why the shipped app is
the TWA above. Kept for reference. `capacitor.config.json` points it at the live site.

> Preview build with synthetic data — not for real personal or health information.
