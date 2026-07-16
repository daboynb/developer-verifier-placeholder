# If you wanna help me

<a href="https://www.buymeacoffee.com/daboynb" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

# What is it?

An empty placeholder APK for `com.google.android.verifier` (Android Developer Verifier), the Google system service that enforces developer verification and can block sideloaded apps from unverified developers on certified devices.

Installing this empty APK takes over the package so the real Verifier can't be (re)installed on top of it (signature mismatch).

# Instructions

The Verifier ships as a privileged **system app** (`/product/priv-app`), so how you get rid of it depends on whether it's already on your device.

### Case A — Verifier NOT yet on your device
1) Download the APK from this repository.
2) Install it. The package slot is now taken, so Google System Updates can't push the real Verifier over it (signature mismatch).

### Case B — Verifier already installed as a system app
You can't overwrite a Google-signed system app with this APK (`INSTALL_FAILED_UPDATE_INCOMPATIBLE`). Disable it with ADB instead (no root):

```
adb shell pm uninstall --user 0 com.google.android.verifier
```
Fallback if that fails:
```
adb shell pm disable-user --user 0 com.google.android.verifier
```

To undo:
```
adb shell cmd package install-existing com.google.android.verifier
adb shell pm enable com.google.android.verifier
```

Enjoy!

# Notes

- Disabling via `--user 0` survives reboot but **not** a factory reset, and a future system update may re-push the Verifier — just run the command again.
- If Google ever makes the Verifier non-removable (baked deeper into the system installer / verified boot, or enforced server-side), a placeholder won't be enough — a de-Googled ROM (GrapheneOS, LineageOS without GApps) removes the enforcement at the source.

# Build it yourself

1) Fork this repo.
2) Enable GitHub Actions.
3) Change references:
  - Inside `settings.gradle.kts`, change `rootProject.name = "Developer Verifier placeholder"` to whatever you want.
  - Inside `app/build.gradle.kts`, change `namespace = "com.google.android.verifier"` to whatever you want.
  - Inside `app/build.gradle.kts`, change `applicationId = "com.google.android.verifier"` to whatever you want.
4) Run the GitHub Action, and you will have your APK!

# Changelog

- v3000000000000: Initial release. `versionName` set to `3000000000000`; `versionCode` kept at `2000000000` (Android caps `versionCode` at the 32-bit int max, ~2.1 billion).
