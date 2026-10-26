# VaxLink Android app

A [Capacitor](https://capacitorjs.com) wrapper that packages the live VaxLink web app
(<https://vaxlink-ai.web.app>) into an installable Android APK, and hosts the install page.

## Install

- **Install page + APK:** <https://sreenivas-sadhu-prabhakara.github.io/vaxlink-android/>
- Or download the APK from the latest [Release](../../releases).

The app isn't on the Play Store; sideload the APK (allow installs from the source when Android asks).

## Design & behaviour

The app loads the live web experience in a WebView, so it matches the website's design exactly and
stays up to date automatically. The WebView user-agent is tagged `VaxLinkAndroid`; the web app detects
this and hides **Sign in with Google** (Google blocks OAuth in embedded WebViews) in favour of the
methods that work there — mobile OTP for employees, email + password for HR and clinic teams.

## Build

Requires a JDK 17 and the Android SDK (build-tools 34, platform `android-34`).

```sh
npm install
npx cap sync android
cd android
JAVA_HOME=/path/to/jdk-17 ANDROID_HOME=/path/to/android-sdk ./gradlew assembleDebug
# → android/app/build/outputs/apk/debug/app-debug.apk
```

Config lives in `capacitor.config.json` — app id `ai.vaxlink.app`, `server.url` = the live site. Brand
icon/theme overrides are in `android/app/src/main/res` (teal `#087676`, white medical-cross glyph).

> Preview build with synthetic data — not for real personal or health information.
