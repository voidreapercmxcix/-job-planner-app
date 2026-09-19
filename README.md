# Job Planner — Android app

The Android build of [Job Planner](../job-planner): the same daily planner and money tracker for vehicle delivery drivers, packaged as a proper app with its own alarms.

It's built for people who'd rather have an icon in the app drawer than a web page. Nothing goes online: the app has no server, no account, and no internet permission requests beyond what Android gives every app. Jobs, expenses and odometer readings stay on the phone.

## What the app adds over the web version

- **Alarms that fire on their own.** Set a planned time and the phone reminds you before it, with the address on the notification, whether the app is open or not. Window-closing reminders too. No calendar needed.
- **Tap the reminder to open the job.**
- **A test button** in the ⋯ menu that fires an alarm in 15 seconds, so you can check it works on a new phone.
- **Reminder lead time** you can change (default 60 minutes).
- Backups and CSV exports go through the phone's Share sheet, so you can send them to yourself or save them wherever you like.

Everything else is the same as the web version, and a backup file from one restores into the other.

## Installing on a phone

1. Open the **Releases** page of this repository on the phone.
2. Download the `.apk` file from the newest release.
3. Open it. Android will ask, once, to allow installs from your browser. Allow it, then Install.
4. Open Job Planner. When it asks to send notifications, allow it. If it opens the "Alarms and reminders" settings page, switch it on there and go back. That's what lets alarms fire at the exact minute.
5. In the ⋯ menu tap **Test an alarm**, lock the phone, and wait 15 seconds.

To update: download the newer `.apk` from Releases and open it. It installs over the old one and keeps your jobs.

## How the build works

You don't need Android Studio or any build tools. GitHub builds the app for you.

Every push to `main` runs the workflow in `.github/workflows/build-apk.yml`. It creates the Android project fresh, copies the web app in, builds the APK, signs it, and attaches it to a new entry on the Releases page. Takes about five minutes.

### One-time setup: the signing key

Android only installs an update over an existing app if both were signed with the same key. So the key must stay the same for the life of the app. Keep the keystore file and its password somewhere safe (a password manager is ideal). If it's lost, nobody can update the app without uninstalling it first.

In the repository go to Settings, Secrets and variables, Actions, and add four repository secrets:

| Name | Value |
|---|---|
| `KEYSTORE_BASE64` | the contents of `keystore-base64.txt` (one long line) |
| `KEYSTORE_PASSWORD` | the password |
| `KEY_ALIAS` | `jobplanner` |
| `KEY_PASSWORD` | the same password |

Until those are set, the workflow still runs but produces a test APK that can't be updated in place.

### Updating the app

1. Change the version in `package.json` (for example `1.0.0` to `1.1.0`).
2. Replace `www/index.html` with the new one.
3. Commit. The new release appears a few minutes later.

## Files

| File | Purpose |
|---|---|
| `www/` | The app itself, identical to the web version plus the alarm bridge |
| `capacitor.config.json` | App name, package ID and notification settings |
| `package.json` | The build packages and the app version number |
| `resources/` | Icon and splash artwork the build turns into every size Android needs |
| `.github/workflows/build-apk.yml` | The build recipe GitHub runs |

## Licence

MIT.
