# DriveDeck

### Keep your map in view, with another app beside it.

[한국어](README.md) · **English**

DriveDeck is an Android launcher for in-car devices. It runs your installed navigation app and another app in separate panes, with quick layout controls, favorites, a saved Home combination, and day/night themes.

[**Download 1.0.1 APK**](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.0.1/DriveDeck-1.0.1.apk) · [Releases](https://github.com/bajohy-totb/drivedeck-releases/releases) · [Complete user guide](docs/guide.en.md)

**Stable 1.0.1:** Built-in wireless debugging, Root and Shizuku are supported. This release improves connection waiting and retries, and includes physical-keyboard focus protection, repeated Back handling and automatic pane sizing recovery. [First connection](docs/guide.en.md#built-in-connection-101)

> **Stable 1.0.1 on GitHub.** Install the non-debuggable release APK over 1.0.0 or a 1.0.1 RC. See the [validation record](docs/validation.md) for actual-vehicle coverage and known limitations.

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

1. Use **Android 13 or newer**. Select built-in wireless debugging, Root or Shizuku for your device. Compatibility depends on the manufacturer’s display and input implementation.
2. Install the APK over your existing DriveDeck. You do not need to uninstall the app or clear its settings to update.
3. Pair the built-in connection or configure Root or Shizuku as described above. Optionally set DriveDeck as the default launcher in Settings.
4. Open **Settings → Device → Updates**, then check for updates. Stable versions are available with **Receive preview versions** turned off. In-app updates are available from RC7 onward.
5. Download an update and tap Install update. If Android asks you to allow installations from DriveDeck, allow it, return, and tap Install update again.

![English update screen](images/en-updates.png)

Automatic checks run when the launcher starts or resumes: six hours after success, or five minutes after failure. **Check for updates** remains immediately available. Downloads and Android installation confirmation remain manual. Both stable and preview channels receive this final 1.0.1 release. Park before installing.

## Learn more

- [Complete guide: setup, every menu, permissions, backup, and troubleshooting](docs/guide.en.md)
- [한국어 사용 설명서](docs/guide.ko.md)
- [Korean and English screenshot gallery](docs/screenshots.md)
- [Preview validation and limitations](docs/validation.md)

This repository distributes **installation files, update metadata, and user documentation**. It does not contain DriveDeck's application source or signing keys. Corresponding LGPL library source and replacement/relink materials are supplied in each release's `relink.zip` asset.
