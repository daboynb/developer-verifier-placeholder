# If you wanna help me

<a href="https://www.buymeacoffee.com/daboynb" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

# Download

**[Get the APK from the Releases page](https://github.com/daboynb/developer-verifier-placeholder/releases/latest)** — it is not in the file list above.

# Instructions
1) Download the APK from the [Releases page](https://github.com/daboynb/developer-verifier-placeholder/releases/latest).
2) Install it.

Enjoy!

If Google tries to replace the APK with the updated Developer Verifier app, it will fail due to a signature mismatch.

# Troubleshooting

Nearly every issue opened on this repo is the same one, in one of two shapes:

- `App not installed as package conflicts with an existing package` — installing by tapping the APK
- `INSTALL_FAILED_UPDATE_INCOMPATIBLE: Existing package com.google.android.verifier signatures do not match newer version` — installing via adb

Both mean the real Developer Verifier is still registered on your device. Android refuses to replace
a package with one signed by a different key ([AOSP](https://cs.android.com/android/platform/superproject/main/+/main:frameworks/base/core/java/android/content/pm/PackageManager.java):
*"a previously installed package of the same name has a different signature than the new package"*).

Whether this is fixable **depends on how the Verifier got onto your device**, and that differs
between vendors and Android builds. Run the triage below to find out which case you are in.

## Step 1 — run the triage

```
adb shell "pm list packages -u | grep verifier"
adb uninstall com.google.android.verifier
```

Do **not** add `--user 0`. By default `adb uninstall` removes the package for *every* user on the
device, which is what you want — `--user 0` only touches the primary profile and is the reason many
people get stuck with `Failure [not installed for 0]`.

## Step 2 — read what it answered

**`Success`** → the Verifier was an ordinary updatable app and is now really gone. Install the placeholder:

```
adb install -r developer-verifier-v3000000000000.apk
```

**`Failure [not installed for 0]`, while step 1 still lists the package** → the Verifier is
preinstalled in your system partition. Android never actually deletes a system package: it marks it
`installed=false` for the user and keeps the package record *and its original signature* in the
package database ([AOSP `DeletePackageHelper`](https://cs.android.com/android/platform/superproject/main/+/main:frameworks/base/services/core/java/com/android/server/pm/DeletePackageHelper.java)).
Any differently signed APK is rejected from then on. There is no non-root workaround — please do not
open an issue for this.

Both outcomes have been reported on similar hardware, so do not assume from someone else's report:
run the two commands on your own device.

## Notes

| Situation | What to do |
|---|---|
| No PC available | Use [Shizuku](https://shizuku.rikka.app/) + aShell to run the same commands on-device |
| `adb shell pm install file.apk` says `Unable to open file` | Wrong command. `adb shell pm` cannot see files on your computer — use `adb install` |
| Samsung | Also uninstall it inside Knox Secure Folder (Secure Folder → Settings → Apps), and turn off Settings → Security and privacy → **Auto Blocker** |
| "Verify apps over USB" in Developer options | Turning it off does not fix a signature mismatch. Only step 2 decides the outcome |

## Changed your mind?

To bring the real Developer Verifier back:

```
adb uninstall com.google.android.verifier
adb shell cmd package install-existing com.google.android.verifier
```

## VirusTotal flags the APK

Some engines report `Android.Riskware.Repack.*`. This is a false positive: the APK contains no code
at all — just a manifest, see [`app/src/main/AndroidManifest.xml`](app/src/main/AndroidManifest.xml).
Heuristics fire because it declares a Google package name while being signed with a different key,
which is precisely the point of a placeholder. Build it yourself if you prefer.

# Changelog :

- v3000000000000: Initial release. `versionName` is `3000000000000` — it is a free-form string, so it
  can be arbitrarily large. `versionCode` stays at `2000000000`: it is a 32-bit integer, and
  [the greatest value Google Play allows is 2100000000](https://developer.android.com/studio/publish/versioning).
  The `versionCode` is what actually blocks the update.
