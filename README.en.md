# DriveDeck

### Keep your map in view, with another app beside it.

[한국어](README.md) · **English**

DriveDeck is an Android launcher for in-car devices. Display two installed apps side by side or stacked vertically, then expand either app from the control bar.

The launcher holds its starting screen orientation while running. Split direction and control-bar position remain adjustable in Settings.

[**Download 1.1.0 APK**](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.1.0/DriveDeck-1.1.0.apk) · [Releases](https://github.com/bajohy-totb/drivedeck-releases/releases) · [Complete guide](docs/guide.en.md)

**Stable 1.1.0:** A new sage and orange interface, sidebar app library and settings, horizontal/vertical splits, a favorites popup, and named app combinations. Install over the existing app to retain settings. Requires Android 13 or newer and built-in wireless debugging, Root, or Shizuku.

![DriveDeck 1.1.0 with two installed apps](images/en-workspace.png)

*Actual Android 13 emulator capture. Organic Maps and Android Clock are separate apps, not bundled with DriveDeck. This is not an in-vehicle or driving-test image. [Validation scope and limitations](docs/validation.md)*

## Features

The table and screenshots below describe stable 1.1.0. **[Preview 1.1.1-rc2](docs/preview.en.md)** adds per-app 120–640dpi, a scrolling app grid and an editor for apps, order and split ratio. Tap Home pairs to open the list; hold it to run default home. Enable **Receive preview versions** in the updater to get it.

| Feature | What it does |
|---|---|
| More space for apps | Full-height content without a global top bar. Optional compact app-name headers. |
| Flexible arrangement | Automatic, horizontal or vertical splits; reverse pane order; left, right or bottom controls. |
| Expand either app | Buttons show actual app names, with a separate return-to-split button. |
| Drag the divider | Preview a ratio while dragging, then release to apply it. Range: 30–70%. Tap for ratio choices. |
| App library | All, Favorites, Recent, Navigation, Music/video and Other filters, name search, responsive grid and paging. |
| App options | Long press or tap ⋯ for favorites, standalone launch, pane assignment, app info or uninstall. |
| Quick favorites | The star opens up to three shortcuts and the full list. Choose automatic classification or the selected pane as the destination. |
| Saved combinations | A Home preset plus up to eight named app pairs. Hold Home to open the list. |
| Settings | Display, Controls, Apps, Connection and Updates sections. Choose a drawer, centered dialog or bottom sheet. |
| Input and recovery | Physical-keyboard routing to the secondary app, non-focusable control bars, connection retry and pane geometry recovery. |
| Korean and English | Follow the device or select DriveDeck's language. |
| Updates and backup | Stable/preview downloads with Android installation confirmation. Back up layouts, favorites and saved combinations. |

![Sidebar app library](images/en-apps.png)

**Removing a favorite does not uninstall an app.** Uninstall opens Android's confirmation. Manage system apps through App info. Open separately launches the app outside DriveDeck's split layout.

## Settings and language

![New settings layout](images/en-settings.png)

Open **Settings → Controls → Language / 언어** to choose Follow device settings, 한국어 or English. App selections and layout are retained, and the choice stays in sync with Android's per-app language setting. Other apps' maps, song titles and notification text are not translated.

## Installation and updates

1. Use **Android 13 or newer**, and install the APK over your existing DriveDeck.
2. For a first installation, follow the [connection guide](docs/guide.en.md#1-getting-started), then choose two different apps.
3. Open **Settings → Updates → Updates** to check and download.
4. Tap Install update and confirm in Android. If asked, allow installations from DriveDeck, return and tap Install update again.

Automatic checks run on startup/resume, six hours after success or five minutes after failure. Manual checking remains immediately available. Download and installation confirmation are manual. Stable offers 1.1.0; preview offers 1.1.1-rc2. Park before installing.

## Learn more

- [Complete user guide](docs/guide.en.md)
- [Korean and English screenshot gallery](docs/screenshots.md)
- [Validation record and vehicle limitations](docs/validation.md)

This repository distributes installation files, update metadata and documentation. It does not contain application source or signing keys. Corresponding LGPL library source and replacement/relink materials are supplied in the same release's `relink.zip` asset.
