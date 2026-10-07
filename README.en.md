# DriveDeck

### Keep your map in view, with another app beside it.

[한국어](README.md) · **English**

DriveDeck is an Android launcher for in-car devices. It shows two installed apps side by side or stacked, and either app can be expanded from the control bar.

The launcher holds its starting screen orientation while running. Split direction and control-bar position remain adjustable in Settings.

[**Download 1.2.1 APK**](https://github.com/bajohy-totb/drivedeck-releases/releases/download/v1.2.1/DriveDeck-1.2.1.apk) · [Releases](https://github.com/bajohy-totb/drivedeck-releases/releases) · [Complete guide](docs/guide.en.md)

**Stable 1.2.1:** fixes the rare restart of the other pane's app after a tap on its widget on the home screen, and makes an app that fails to open after quick replacements rarer. **Since 1.2.0:** The Home button keeps the navigation app and opens a home screen in the other pane. The home screen holds app icons and Android widgets such as a clock, weather or music. Tapping a text field in either app opens the keyboard in that app at once, including the map's search box. Every screen was rebuilt on one design: all control-bar buttons have labels, and Settings is a list of rows in six categories. The Split ratio panel has a Swap positions button that swaps the two apps without restarting them. If you had set a default home, Home still opens it; change this under Settings → Apps & pairs → Home button. Actual Netflix playback in protected video compatibility mode has not been verified. Install over the existing app to keep your settings. Requires Android 13 or newer and built-in wireless debugging, Root, or Shizuku. Not yet checked on the real 엠스틱4 and Galaxy Z Fold8.

![DriveDeck showing a map app and another app side by side](images/en-workspace.png)

*Actual Android 13 emulator capture. The map app and the app beside it are separate apps, not bundled with DriveDeck. This is not an in-vehicle or driving-test image. [Validation scope and limitations](docs/validation.md)*

## Features

The features below describe stable 1.2.1. See also the [app size, app list and pair guide](docs/preview.en.md).

| Feature | What it does |
|---|---|
| Home screen and widgets | Home opens the home screen in the secondary pane. Tap + to add a widget or an app; hold an item to move or remove it. An app chosen from All apps opens in that pane. |
| Keyboard in either app | The keyboard opens in the app whose text field you tap. The pane tapped last takes the typing; dragging or pinching the other pane does not change that. |
| More space for apps | Full-height content without a global top bar. Optional compact app title bars. |
| Flexible arrangement | Auto, side-by-side or stacked splits; swap pane positions; left, right or bottom control bar. |
| Expand either app | The control-bar buttons named after each app expand it; Split shows both again. |
| Divider and split ratio | Drag the divider to preview a ratio in 5% steps and release to apply it. Range: 30–70%. Tap it for the Split ratio panel, where Swap positions swaps the two apps. |
| App list | Category filters, search, a grid that fits the screen and vertical scrolling. |
| App options | Hold an app for favorites, open in a pane, Open separately, App size (DPI), Protected video compatibility, App info or Uninstall app. |
| Quick favorites | The star opens up to three shortcuts and View all. Choose automatic classification or the selected pane as the destination. |
| Saved pairs | Edit up to eight pairs in Settings → Apps & pairs → App pairs. One pair can be the default home. |
| Settings | Display, Control bar, Apps & pairs, Connection, General and Updates. Menus open on the right, in the center or at the bottom. Holding Home also opens Settings. |
| Input and recovery | Physical-keyboard typing, control bars that never take keyboard focus, connection retry, pane size recovery and redrawing a map left at its old size. |
| Korean and English | Follow the device or select DriveDeck's language. |
| Updates and backup | Stable/preview downloads with Android installation confirmation. Back up layouts, favorites and saved pairs. |

![App list with category filters and search](images/en-apps.png)

**Removing a favorite or removing an app from home does not uninstall it.** Uninstall opens Android's confirmation. Manage built-in apps through App info. Open separately launches the app outside DriveDeck's split layout.

## Home screen

![Home screen with app icons and widgets in the pane beside the map](images/en-home.png)

*Actual Android 13 emulator capture. The map and the widgets come from separately installed apps, not from DriveDeck. This is not an in-vehicle or driving-test image.*

Tap **+** to add a widget or an app. Hold an item and drag it to move it, or drop it on the bar at the top to remove it. Hold and release without moving for its options; a widget can be resized in steps of width and height. After opening an app, press Home again to return to the home screen. Details are in the [Home screen section of the guide](docs/guide.en.md#3-home-screen).

## Settings and language

![Settings with six categories](images/en-settings.png)

Open **Settings → General → Language / 언어** to choose Follow device settings, 한국어 or English. App selections and layout are retained, and the choice stays in sync with Android's per-app language setting. Other apps' maps, song titles and notification text are not translated.

## Installation and updates

1. Use **Android 13 or newer**, and install the APK over your existing DriveDeck.
2. For a first installation, follow the [connection guide](docs/guide.en.md#1-getting-started), then choose two different apps.
3. Open **Settings → Updates → Updates** to check and download.
4. Tap Install update and confirm in Android. If asked, allow installations from DriveDeck, return and tap Install update again.

Automatic checks run on startup/resume, six hours after success or five minutes after failure. Manual checking remains immediately available. Download and installation confirmation are manual. Stable offers 1.2.1; preview offers 1.3.0-rc8 (the new name M4 Launcher, a new icon and the rebuilt home screen: free placement, pages, a dock, folders, backgrounds and a pull-up app list). Park before installing.

## Learn more

- [Complete user guide](docs/guide.en.md)
- [Korean and English screenshot gallery](docs/screenshots.md)
- [Validation record and vehicle limitations](docs/validation.md)

This repository distributes installation files, update metadata and documentation. It does not contain application source or signing keys. Corresponding LGPL library source and replacement/relink materials are supplied in the same release's `DriveDeck-1.2.1-relink.zip` asset.
