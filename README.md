# If you wanna help me

<a href="https://www.buymeacoffee.com/daboynb" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

# Instructions
1) Download the APK from this repository.
2) Install it.

Enjoy!

If Google tries to replace the APK with the updated Developer Verifier app, it will fail due to a signature mismatch.

The Verifier ships as a system app, so if it's already installed you can't overwrite it. Disable it with ADB instead:
```
adb shell pm uninstall --user 0 com.google.android.verifier
```

# Changelog :

- v3000000000000: Initial release.
