# DriveDeck

### Keep your map in view, with another app beside it.

[한국어](README.md) · **English**

DriveDeck is an Android launcher for in-car devices. It runs your installed navigation app and another app in separate panes, with quick layout controls, favorites, a saved Home combination, and day/night themes.

[**Download 1.0.0 APK**](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.0.0/DriveDeck-1.0.0.apk) · [Releases](https://github.com/bajohy-totb/drivedeck-releases/releases) · [Complete user guide](docs/guide.en.md)

**Built-in connection preview:** [1.0.1-rc3 APK](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.0.1-rc3/DriveDeck-1.0.1-rc3.apk) connects directly through **wireless debugging without installing Shizuku** and improves physical keyboard input in App 1. Pair once by entering Android's six-digit code in the DriveDeck notification. Root and Shizuku remain available. Requires Android 13+; the user's phone and vehicle remain unverified. [Setup instructions](docs/guide.en.md#built-in-connection-preview-101-rc3)

> **1.0.0 is the official GitHub release.** This release APK disables debugging and can update existing RC installations. Resolution of reported vehicle mute freezes, lost connections, and repeated standalone-launch delays has not been confirmed in a vehicle. Read the [validation scope and known limitations](docs/validation.md).

![DriveDeck's English settings over the two-app workspace in a real Android emulator](images/en-settings.png)

*Actual Android 13 emulator capture. Organic Maps and Android Clock are separate apps, not bundled with DriveDeck. They keep their own language settings. This image does not represent an in-vehicle or driving test.*

## Features

| Feature | What it does |
|---|---|
| Two apps side by side | Run and touch navigation and App 1 in separate panes. |
| Layout controls | Expand navigation, return to split screen, or expand App 1. |
| Adjustable split | Choose a ratio from 3:7 to 7:3, or hold and drag the divider. |
| App chooser | Browse All, Favorites, and Recent filters. |
| Hold for options | Add/remove favorites, open separately, open in a pane, view app info, or uninstall. |
| Home preset | Save your current pair of apps for the Home button. |
| Display settings | Set theme, control-bar side, favorites bar, and app/keyboard scale. |
| Media and guidance | Show supported playback controls and navigation notifications with permission. |
| Korean and English | Follow the device language or choose a language for DriveDeck only. |
| In-app updates | Download from stable or preview channels and confirm installation in Android. |
| Settings backup | Export app choices, Home preset, layout, favorites, and theme. |
| Diagnostics and recovery | Retry connections, reopen panes, save diagnostics, and recover from startup failures. |

## Hold an app for options

![English app options menu](images/en-actions.png)

The app chooser and favorites bar offer the same menu. **Remove from favorites does not uninstall an app.** Uninstall app opens Android's confirmation dialog. Built-in apps are managed through App info. **Open separately** uses the app's own screen outside the split layout.

## Korean / English

![Language selection screen](images/en-language.png)

Open **Settings → Device → Language / 언어** and choose Follow device settings, 한국어, or English. The screen reopens after a language change; saved app choices, favorites, and layout are kept. The selection stays in sync with Android's per-app language settings. DriveDeck does not translate other apps, maps, track titles, or notification content.

## Installation and updates

1. Use an **Android 13 or newer device**. Stable 1.0.0 uses root; preview 1.0.1-rc3 supports built-in wireless debugging, Root and Shizuku. Compatibility depends on the manufacturer's display and input implementation.
2. Install the APK over your existing DriveDeck. You do not need to uninstall the app or clear its settings to update.
3. Pair the built-in connection or configure Root or Shizuku as described above. Optionally set DriveDeck as the default launcher in Settings.
4. Open **Settings → Device → Updates**, then check for updates. Stable versions are available with **Receive preview versions** turned off. In-app updates are available from RC7 onward.
5. Download an update and tap Install update. If Android asks you to allow installations from DriveDeck, allow it, return, and tap Install update again.

![English update screen](images/en-updates.png)

Automatic checks run when the launcher starts or resumes, skipping checks within six hours of the last one. Downloads and Android installation confirmation remain manual. The stable channel stays on 1.0.0. Enable **Receive preview versions** for 1.0.1-rc3. Park before installing.

## Learn more

- [Complete guide: setup, every menu, permissions, backup, and troubleshooting](docs/guide.en.md)
- [한국어 사용 설명서](docs/guide.ko.md)
- [Korean and English screenshot gallery](docs/screenshots.md)
- [Preview validation and limitations](docs/validation.md)

This repository distributes **installation files, update metadata, and user documentation**. It does not contain source code or signing keys.
