# If you want to help me

<a href="https://www.buymeacoffee.com/daboynb" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

# What is it?

Build an empty APK for placeholders.

# Instructions, how to?

1) Fork this repo.
2) Enable GitHub Actions.
3) Change references:
  - Inside `settings.gradle.kts`, change `rootProject.name = "Developer Verifier placeholder"` to whatever you want.
  - Inside `app/build.gradle.kts`, change `namespace = "com.google.android.verifier"` to whatever you want.
  - Inside `app/build.gradle.kts`, change `applicationId = "com.google.android.verifier"` to whatever you want.
4) Run the GitHub Action, and you will have your APK!