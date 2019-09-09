# Bitrise Workflow Quirks

We currently have four workflows which generate builds:

* `build-demo-sand-prod-apks`: This workflow **builds** the APKs for all environments \(`SANDBOX`, `DEMO`, `PRODUCTION`\) and makes them available for download.
* `deploy-sandbox-to-play-store`: This workflow **builds** the `SANDBOX` APK **and** deploys it to the Play Store.
* `deploy-demo-to-play-store`: This workflow **builds** the `DEMO` APK **and** deploys it to the Play Store.
* `deploy-prod-to-play-store`: This workflow **builds** the `PRODUCTION` APK **and** deploys it to the Play Store.

The quirk here to note is that the gradle build command is duplicated across all of these builds. The gradle config for `build-demo-sand-prod-apks` is hardcoded as part of the workflow, whereas for the others, there are sub-workflows named `_gradle-sandbox-build`, `_gradle-sandbox-build`, and `_gradle-prod-build` respectively.

Currently, if you need to make a change to the gradle build command, you need to make the change in **ALL** of these workflows.

