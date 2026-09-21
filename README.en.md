# DriveDeck

### Keep your map in view, with another app beside it.

[한국어](README.md) · **English**

DriveDeck is an Android launcher for in-car devices. It runs your installed navigation app and another app in separate panes, with quick layout controls, favorites, a saved Home combination, and day/night themes.

[**Download RC8 APK**](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.0.0-rc8/DriveDeck-1.0.0-rc8.apk) · [Releases](https://github.com/bajohy-totb/drivedeck-releases/releases) · [Complete user guide](docs/guide.en.md)

> **1.0.0-rc8 is a preview.** Vehicle mute freezes, lost connections, and delays after repeated standalone launches have not been confirmed resolved. This is not stable 1.0.0. Earlier RC7 testing also recorded a roughly 6.8-second stall while a screen was stopping.

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
| In-app updates | Download previews and confirm installation in Android. |
| Settings backup | Export app choices, Home preset, layout, favorites, and theme. |
| Diagnostics and recovery | Retry connections, reopen panes, save diagnostics, and recover from startup failures. |

## Hold an app for options

![English app options menu](images/en-actions.png)

The app chooser and favorites bar offer the same menu. **Remove from favorites does not uninstall an app.** Uninstall app opens Android's confirmation dialog. Built-in apps are managed through App info. **Open separately** uses the app's own screen outside the split layout.

## Korean / English

![Language selection screen](images/en-language.png)

Open **Settings → Device → Language / 언어** and choose Follow device settings, 한국어, or English. The screen reopens after a language change; saved app choices, favorites, and layout are kept. The selection stays in sync with Android's per-app language settings. DriveDeck does not translate other apps, maps, track titles, or notification content.

## Installation and updates

1. Use an **Android 13 or newer device with root access**. Compatibility depends on the manufacturer's display and input implementation.
2. Install the APK over your existing DriveDeck. You do not need to uninstall the app or clear its settings to update.
3. Allow the root connection to run apps inside the panes. Optionally set DriveDeck as the default launcher in Settings.
4. Open **Settings → Device → Updates → Receive preview versions**, then check for updates. In-app updates are available from RC7 onward.
5. Download an update and tap Install update. If Android asks you to allow installations from DriveDeck, allow it, return, and tap Install update again.

![English update screen](images/en-updates.png)

Automatic checks run when the launcher starts or resumes, skipping checks within six hours of the last one. Downloads and Android installation confirmation remain manual. Only previews are available now, so the stable channel may have no update. Park before installing.

## Learn more

- [Complete guide: setup, every menu, permissions, backup, and troubleshooting](docs/guide.en.md)
- [한국어 사용 설명서](docs/guide.ko.md)
- [Korean and English screenshot gallery](docs/screenshots.md)
- [Preview validation and limitations](docs/validation.md)

This repository distributes **installation files, update metadata, and user documentation**. It does not contain source code or signing keys.
