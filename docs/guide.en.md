# DriveDeck user guide

[Home](../README.en.md) · [한국어](guide.ko.md) · **English**

## 1. Getting started

Stable 1.1.0 supports Android 13 or newer with built-in wireless debugging, Root or Shizuku. Install the APK and configure the connection supported by your device. Select an installed app for navigation and another for the secondary pane. The same app cannot be assigned to both panes.

### Built-in connection (1.0.1)

Connect directly through Android wireless debugging. This is the default for a new installation; upgrades retain the existing connection method.

1. Connect to Wi-Fi and open **Settings → Connection → Connection method → Built-in connection → Built-in connection setup** in DriveDeck.
2. Tap **Start setup · Enable code-entry notification** and allow notifications.
3. In Android Developer options, enable **Wireless debugging** and open **Pair device with pairing code**. If Developer options is missing, tap Build number seven times in About device. Menu names vary by manufacturer.
4. Keep the code screen open, pull down notifications, and enter its six digits using DriveDeck's **Enter code** action. Leaving Android Settings may expire the code.
5. After the completion notification, return to DriveDeck and choose apps for both panes.

![Built-in connection setup](../images/en-native-adb.png)

Pairing stays on this device and is excluded from layout backups. Wireless debugging must be enabled when reconnecting the app. After a reboot, Wi-Fi change or deleting the pairing in Android, you may need to enable wireless debugging or pair again. If your device lacks wireless debugging, use Root or Shizuku.

For an incorrect code, dismiss Android's failure dialog, open a new code screen and retry. If discovery fails, manual code and pairing-port entry is available while keeping the system code screen open. The pairing port is the number after the colon on that screen.

See the [validation scope](validation.md) for actual-device limitations. Library notices, corresponding source and replacement materials are provided in the [same release's relink ZIP](https://github.com/bajohy-totb/drivedeck-releases/releases/tag/v1.1.0).

### Physical keyboards (1.0.1)

Connect a USB or Bluetooth keyboard, then tap a **text field in the secondary app**. Navigation touches preserve that app as the typing destination. Input works with the secondary app expanded or in a split layout. Settings and the app picker receive input while open; closing them returns input to the secondary app.

**The sidebar and favorites popup never receive keyboard focus.** Tab and cursor keys cannot select their buttons, including while menus are open. Use touch and long press to operate the bars.

Text, cursor keys, deletion, Home/End and modifiers such as Ctrl and Shift are routed to the secondary app. Actual shortcuts and language switching depend on the app, Android keyboard settings and IME. See the [validation record](validation.md) for hardware and vehicle coverage.

### Shizuku connection (1.0.1)

Unrooted Android 13+ devices can select Shizuku in 1.0.1.

1. Install [official Shizuku](https://shizuku.rikka.app/download/) 13 or newer, pair using wireless debugging, and start it.
2. In DriveDeck, select **Settings → Connection → Connection method → Shizuku**.
3. Tap **Authorize / open Shizuku**, then allow DriveDeck access.
4. Choose different apps for the two panes. If Shizuku stops, start it again to reconnect automatically.

![Shizuku connection settings](../images/en-shizuku.png)

Start Shizuku again after a device reboot. DriveDeck explains missing startup or authorization and does not repeatedly open permission prompts during automatic retries. The connection choice stays on this device and is excluded from layout backups. APK installation permission is separate from Shizuku authorization. Manufacturer display/input compatibility remains subject to the [validation scope](validation.md).

Choose **Settings → Controls → Set as default launcher** to use DriveDeck as Android's Home app. When it opens automatically depends on the device's boot and Home behavior. To return to another launcher, change the default Home app in Android settings.

## 2. Controls and layouts

| Control | Action |
|---|---|
| Home | Return to the split layout. Restore the saved app combination if a Home preset exists. |
| Back | Close the chooser or menu first; otherwise send Back to the selected pane's app. |
| App chooser | Select a destination pane, then find an app using filters, search and pages. |
| Expand button labeled with the navigation app's name | Fill the area beside the controls with navigation. |
| Split screen | Show both apps along the selected split direction. |
| Expand button labeled with the secondary app's name | Fill the area beside the controls with the secondary app. |
| Favorites | Open the quick favorites popup. |
| Settings | Open Display, Controls, Apps, Connection and Updates settings. |

![English app chooser](../images/en-apps.png)

Use the secondary app for typing and the keyboard. You can pan and zoom the navigation pane by touch, but it is not a keyboard input target. Expanding one pane stops display output from the hidden pane; it does not force-stop the other app's background work, such as a music service.

Tap the divider for ratio choices, or drag it along the split direction. A guide previews the split; releasing applies it. Navigation can occupy 30–70% of the split area.

Version 1.0.1 discards expired Back taps instead of delivering a delayed burst. Layout changes during app creation and later geometry mismatches are corrected to the current pane dimensions. Increase **App and keyboard scale** for larger text and controls; density scaling is separate from the pane’s physical pixel dimensions.

## 3. Favorites and app options

Hold an icon in the app chooser or favorites popup.

| Option | Action |
|---|---|
| Add / Remove favorite | Change the shortcut list. This does not uninstall the app. |
| Open separately | Use the app's own screen outside the split layout. The pane is restored when you return. |
| Open in navigation / secondary app | Assign that pane. Apps already used in the other pane cannot be duplicated. |
| App info | Open that app's Android permissions, storage, and other settings. |
| Uninstall app | Open Android's uninstall confirmation. Manage built-in apps through App info. |

**Settings → Connection → Help for the app** also provides Change app, Reopen, Reconnect display, Open separately, Save/Copy diagnostics, and Clear this pane. Clearing a pane removes its selection, not the installed app. Some video or protected-content apps can show black screens or close in a pane; try opening them separately.

## 4. Make the layout yours (1.1.0)

![English settings](../images/en-settings.png)

Use **Display, Controls, Apps, Connection and Updates** in the settings sidebar. The content area scrolls independently on smaller displays.

- **Display:** Automatic/horizontal/vertical splits, a 30–70% ratio, reversed order, automatic/left/right/bottom controls and 120–240dpi app/keyboard scale. Automatic choices follow the starting screen shape. Device rotation is locked while running; split direction and bar position remain adjustable.
- **Controls:** Automatic/manual day and night themes, optional app headers, selected-pane outline, status information, drawer/dialog/bottom-sheet presentation, language and default launcher. App headers are hidden by default. Device temperature appears only when a readable sensor is available.
- **Apps:** App selection, favorites and launch destination, saved combinations, Home preset, media controls and backup/restore.
- **Connection:** Connection method, reconnect, reopen/help for each app, diagnostics save/copy.
- **Updates:** Current version and the update screen.

The star opens up to three favorites plus the full list. By default, apps classified as navigation open in the navigation pane; other apps open in the secondary pane. Enable **Quick launch in selected pane** to use the current selection. The same app cannot occupy both panes.

Hold Home or open **Settings → Apps → App combinations** to save up to eight named pairs. They restore the two apps, ratio, split direction and order. Reusing a name replaces it; saving beyond eight removes the oldest. Hold a saved entry to confirm removal. The Home button's default pair is saved separately.

The app library has All, Favorites, Recent, Navigation, Music/video and Other sidebar filters with name search. Column and row counts adapt to the screen. Use the bottom arrows for more pages. Hold an app or tap ⋯ for its options.

## 5. Language

Open **Settings → Controls → Language / 언어** and choose **Follow device settings / 한국어 / English**. You can also change the language in Android's DriveDeck app settings. Unsupported device languages fall back to English.

Android persists the language selection. App choices, favorites, and layout survive screen recreation. Maps, music, app names, and notification content remain as supplied by their respective apps. Changing DriveDeck's language does not change other apps' languages.

## 6. Media and navigation guidance

Open **Settings → Apps → Media controls** to allow notification access. When a music app provides a playback session and supports the command, you can play/pause, skip to the next track, and open the playing app. Unsupported commands are disabled. Your music app remains in its selected pane.

If your navigation app supplies an ongoing guidance notification, DriveDeck can display it in a guidance strip while the navigation pane is hidden. Notification formats vary; not every navigation app is supported. This strip is separate from guidance provided by the navigation app itself.

## 7. Updates

Use **Settings → Updates → Updates** to check, download, and install. Both channels receive this final 1.1.0 release. **Receive preview versions** selects whether to receive future RC builds too. Versions before RC7 need one manual APK installation first. Switching channels does not automatically downgrade the app.

A download continues after leaving its screen while the app process remains alive. Incomplete downloads are discarded after process termination and must be restarted. Completed files are verified again and restored after a restart. The updater checks size, SHA-256, package, version, Android compatibility, and signature. Reinstalls, downgrades, and files signed with another key are rejected.

Changing channels discards the saved file from the previous channel. Automatic checks run on launcher start/resume, at least six hours after success or five minutes after failure. Manual checks remain immediately available. Downloading and confirming installation remain manual.

## 8. Backup, diagnostics, and recovery

**Back up settings** exports app choices, Home preset, named combinations, layout direction/order, split ratio, scale, favorites, recent apps, and theme to JSON. **Restore settings** validates the file, applies its settings, and reopens the screen. Installed apps, root/notification/install permissions, and the Android-managed app language are not included in the backup file.

**Save diagnostic log** writes startup, connection, and app diagnostics to Download/DriveDeck. Normal diagnostics exclude raw notifications, destinations, and track titles. **Copy navigation notification format** deliberately copies raw content and warns that a destination may be included. Diagnostics are saved or copied on your request, not automatically uploaded to this repository. Update checks and downloads connect to GitHub.

After repeated startup failures or a detected previous crash, **Safe mode** offers a normal restart, opening without launching apps, clearing pane selections, resetting settings, choosing another launcher, updates, and diagnostic export. Resetting removes settings, so back them up first.

## 9. Troubleshooting

| Symptom | What to check |
|---|---|
| Lost connection / empty panes | Check wireless debugging and pairing, Root approval, or Shizuku startup and authorization for your selected method. See [validation](validation.md) for manufacturer coverage. |
| Black screen or app closes | Try Open separately. The app may restrict external displays or protected content. |
| No keyboard | Use secondary app and check that an Android keyboard is installed and enabled. |
| No media controls | Check notification access and an active playback session in the music app. |
| No update available | Select Check for updates and check the installed version and channel. The same or an older version is not offered as an update. |
| Installation blocked | Check install permission for DriveDeck, storage, Android version, and matching signatures. |
| Freeze when muting in the vehicle | This has not been confirmed resolved. See [validation and limitations](validation.md). |

Every app, vehicle, and manufacturer combination has not been verified. See the [screenshot gallery](screenshots.md) and [release history](https://github.com/bajohy-totb/drivedeck-releases/releases).

### Connection and update-check improvements in 1.0.1

The UI remains responsive while waiting for the bridge handshake. Manual retries replace old timers, and automatic reconnection backoff is capped at 30 seconds. Automatic update checks become eligible on a later start/resume six hours after success or five minutes after failure. Manual checks remain immediately available.
