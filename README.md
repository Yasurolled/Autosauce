# Autosauce

<sub>Source and releases: [github.com/Yasurolled/Autosauce](https://github.com/Yasurolled/Autosauce)</sub>

<p align="center">
    <img src="https://github.com/user-attachments/assets/579b7b03-23e0-4eda-a05d-c51d28a72113"
    alt="Autosauce"
    height="200">
</p>

Autosauce is an Android app that allows you to turn your Android tablet or phone into an Android Auto receiver.

## What this is

**This repository is a fork.** It starts from [open-headunit](https://github.com/andreknieriem/open-headunit) — itself a revival of the original headunit project by the great Michael Reid ([the original project](https://github.com/mikereidis/headunit)) — and adds the Autosauce work on top:

- Bluetooth as a first-class connection option (home tile, settings, color customization)
- an opt-in Material You "dynamic color" toggle on the Appearance screen
- a running plain-English change log in [`CHANGELOG-AUTOSAUCES.md`](CHANGELOG-AUTOSAUCES.md)

It is built and maintained by **Yasurolled**.

## Trademarks

**Yasurolled** and **Autosauce** are trademarks of Yasurolled.
Android, Google Play, Google Maps and Android Auto are trademarks of Google Inc.

## AI assistance

This fork was developed with the help of **Claude AI** — Anthropic's
[Claude Code](https://claude.com/claude-code) coding agent. The model
used is stated separately: **Agnes-3.0-flash, developed by Sapiens AI**.

## Contributors

The record-keeping list lives in [CONTRIBUTORS.md](CONTRIBUTORS.md):

- **Yasurolled** — fork author; Bluetooth connection work, button-color
  customization, the Material You toggle, and the fork's documentation.
- **Claude Code (Anthropic)** — AI coding agent that developed the
  Autosauce work alongside the fork author. The model it ran on is
  stated separately from the agent itself: **Agnes-3.0-flash
  (Sapiens AI)**.

## NOTE!
**Android Auto 17.4 and newer can prevent legacy wireless triggers from launching projection, including Self-Mode and automated startup via Wireless Helper. Rooted devices have a workaround: re-enable Android Auto's wireless startup receiver.**
Choose from the five options below according to your Android Auto version and setup:
1. **USB Wireless Android Auto Dongle (Recommended):** Hardware dongles provide seamless, hardware-level plug-and-play.
2. **Native Mode:** Direct Wi-Fi Direct or Headunit Hotspot handshake.
3. **Headunit Server (Developer Mode):** An option for wireless projection and Self-Mode without root. On the device running Android Auto, open its developer settings and tap "Start Headunit Server".
4. **Wireless Helper:** The legacy startup flow works with Android Auto up to **17.3**; for **17.4+** on rooted devices, see option 5.
5. **Root workaround (AA 17.4+):** Re-enable `WirelessStartupReceiver` with one root command to restore the previous wireless startup flow, including Wireless Helper, without starting the developer headunit server. See [the command and setup below](#5-root-workaround-for-android-auto-174).

## Screenshots
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/6ed31626-480e-404f-ba7a-753b14230a02" />
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/ce67584e-fcf6-4308-b11a-116780df47f7" />
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/fc2870be-a173-4113-8187-4835bd564842" />


## How to use
**Check out the [Wiki](https://github.com/andreknieriem/open-headunit/wiki) for detailed documentation, setup guides and troubleshooting!**

### Wired USB Connection
- Connect your Android device (phone) to the tablet running Autosauce via USB cable.
- Make sure that Android Auto is installed on your phone.
- Set your phone to Host-Mode if nescessary and select Android Auto
- Click the USB Button in Autosauce, find your phone and click the right button to allow connection
- Click on your phone in the list and wait for Android Auto to start

### Wireless Connection Options
Choose from the five options below depending on your Android Auto version and setup:

#### 1. USB Wireless Android Auto Dongle (Most Reliable)
- A standard hardware USB Wireless Android Auto Dongle (plugged into the headunit) handles the entire wireless negotiation independently.
- Provides seamless plug-and-play connection regardless of phone Android Auto version.

#### 2. Native Mode (Wi-Fi Direct / Headunit Hotspot)
- Directly communicates with Android Auto's native wireless protocol without helper apps.
- Supports **Wi-Fi Direct (P2P)** or the **Headunit Hotspot** transport.
- Configure under Autosauce Settings -> **Android Auto Mode** -> **Native Mode**.

#### Optional automatic Wi-Fi scan pause

In advanced settings → **Wireless Connection** → **Pause automatic Wi-Fi searches**, enable
**Pause during wireless connections**. It is off by default and applies immediately. The setup
screen puts installation and app approval under **Common setup**, vendor startup under
**On FYT devices**, and manual debugging instructions under **On other devices**.

- **Android 13+:** in Native mode (Wi-Fi Direct or Hotspot), pauses Android's periodic connectivity and PNO searches through the global
  autojoin gate. Client modes keep automatic reconnection enabled. It reads the original value before changing it and restores that value on disconnect.
- **Android 11–12L Wi-Fi Direct:** disables global autojoin through Shizuku while keeping
  the P2P group. These versions expose no state getter: on release, searches are enabled again,
  even if they were disabled beforehand. Command acceptance is not a measured scan state.
- **Android 8–10 Wi-Fi Direct:** disables the internal connectivity manager through Shizuku.
  Apply on connection, then renew every five minutes while connected. Android network requests
  can re-enable searches between renewals. The long interval limits repeated firmware roaming
  cleanup on Android 8–9; this is best-effort, not a guarantee of continuous suppression.
  Release enables the manager again; the previous
  value cannot be read. Reboot resets these temporary legacy Direct controls.
- **Android 8–12L Hotspot:** with ordinary Wi-Fi off, pauses the remaining scan-always path
  and restores its recorded value. The app never turns off a live Wi-Fi Direct radio.
- **Android 7 and older:** this option is unavailable; existing wireless behavior is unchanged.

Android 11+ can start Shizuku through wireless debugging; the screen also points to its USB setup
for devices without that option. Android 8–10 uses USB/computer setup, with an explicit on-device
startup option on supported FYT systems. FYT startup asks for normal ADB RSA authorization and
keeps its network ADB listener open, including after app restart. Use **Close wireless ADB**
to restore paused Wi-Fi searches and then stop the listener opened by this app. If restoration
cannot finish, ADB stays open for recovery. Closing can also stop Shizuku and disconnect USB debugging.
Existing ADB ports and persistent settings are left alone. An interrupted explicit close is
retried when the app reopens. On Android 8–10 FYT systems, a previously configured FYT start is
attempted when a Native wireless transport opens, provided this option stays enabled and Shizuku
is not already running. App launch and boot alone do not start it. The attempt runs alongside the
Android Auto handshake and does not delay projection. Each connection gets at most one attempt;
disconnecting cancels unfinished startup. USB, self and wireless client sessions are excluded.
Explicitly closing ADB disables automatic startup until the FYT start button is used again.
Android may still require debugging authorization; failure does not block Android Auto.

On app-process death, a separate shell helper attempts rollback. A device-local recovery record
survives interruption of both processes and is retried when Shizuku is available again. Pending
recovery is shown on the home screen and in settings, with automatic retries while an authorized
helper is available. For a legacy scan-always recovery, Android can also restore scanning through
a system confirmation without restarting Shizuku. Where the state can be read, the app verifies
restoration before clearing its recovery record. Legacy Direct clears the record after Android
accepts the enable request. On paths with readable state, if searches are re-enabled elsewhere,
the app leaves them enabled for that connection instead of repeatedly forcing them off. Other apps' explicit scans, Wi-Fi Direct discovery and firmware roaming are outside
this control; this option cannot promise to eliminate every radio interruption.

#### FYT external Bluetooth module transport (experimental)
On FYT units where the phone pairs for calls with a separate Bluetooth module (for example **DUDUAUTO** on DUDUOS), Native Mode can carry its Android Auto Bluetooth handshake through `/dev/auto_serial`, the relay the stock Car Link app (`com.syu.carlink`) uses, while calls remain on the module and the vehicle microphone/speakers. Tested so far only on DUDUOS with a BLINK module.

Requirements:
- An FYT unit that exposes `/dev/auto_serial` (Car Link's external-module backend, `sys.fyt.bluetooth_type` above zero).
- Nothing extra where the node is app-openable, as it is for the stock Car Link. Where it is not (node mode or SELinux), root access for Autosauce, used as a fallback.
- The stock `com.syu.carlink` package disabled, because two readers on the same terminal split protocol messages.
- The phone paired with the module; pairing and phone priority remain managed by the module.

Setup:
1. Disable the stock client: `adb shell pm disable-user --user 0 com.syu.carlink` (prefix with `su -c` if the shell refuses).
2. In Autosauce, select **Native Mode** and enable **Connect through the FYT external Bluetooth module**.
3. Keep the phone paired with the module and connect normally.

On a detected FYT module unit with this transport off, Native Mode refuses to start and names this setting, instead of listening on a radio the phone is not paired with. If the channel cannot be opened (Car Link still enabled or running, or the node not app-openable and root denied), the reason is shown on screen and retried with a growing delay.

Android Bluetooth driver selection, preferred-phone and wake-list controls are intentionally hidden on this route because Android cannot see or dial phones bonded to the external module. To roll back, turn this transport off and re-enable the stock client with `adb shell pm enable com.syu.carlink` (prefix with `su -c` if needed).

#### 3. Headunit Server (No-Root Option for Self-Mode on AA 17.4+)
- Starts the native Android Auto developer server directly on your phone or on the same device (Self-Mode).
- **Setup:**
  1. Open Android Auto settings on your phone (or tablet in Self-Mode).
  2. Scroll down and tap **Version** 10 times to unlock Developer settings.
  3. Tap the three-dot menu in the top right corner and choose **Start headunit server**.
  4. In Autosauce, tap the **WiFi** button to connect (or use Self-Mode).

#### 4. Wireless Helper
- Our companion app triggers the wireless connection automatically in the background.
- **Compatibility:** Android Auto **v17.3 and below**, or **v17.4+** with the root workaround in option 5.
- **Download:** [Wireless Helper on Google Play Store](https://play.google.com/store/apps/details?id=com.andrerinas.wirelesshelper)
- **Setup:** Set Autosauce Wireless Mode to **Helper Mode**, ensure both devices are in the same network or Wi-Fi Direct group, and start the service in the Wireless Helper app.

#### 5. Root Workaround for Android Auto 17.4+
On the device running Android Auto (normally the phone), open a **root shell**, for example by running `su` in a terminal and granting root access. Then run:

```sh
pm enable --user 0 \
com.google.android.projection.gearhead/com.google.android.apps.auto.wireless.setup.receiver.WirelessStartupReceiver
```

Re-enabling this receiver restores the previous wireless startup flow on affected Android Auto versions. Keep using the same Wireless Helper / Helper Mode setup as before; the developer headunit server is not required for this workaround. Root is needed on the device running Android Auto, not on a separate head unit.

`--user 0` targets the primary Android user. This is an internal Android Auto component: the workaround requires it to exist in the installed version, and future updates may change its name or behavior.

### Connect Wirelessly via Intent (Power Users)
You can trigger a wireless connection attempt using an Android Intent. This is useful for automation tools like **Tasker**, **MacroDroid**, or via **ADB**.

**URI Scheme:** `headunit://connect?ip=<PHONE_IP>`

**Example ADB Command:**
```bash
adb shell am start -a android.intent.action.VIEW -d "headunit://connect?ip=192.168.1.25"
```

## Known Issues
- **Google Maps in Portrait Mode:** Touch interactions (searching, scrolling) within Google Maps may not work as expected when using Portrait Mode on some devices. **Fix:** Try reducing the **Pixel density (DPI)** setting to **below 200** (e.g., 190) in the app settings. This often restores full functionality.
- **Wireless Connection Drops:** If the connection drops frequently, disable **"WiFi Assistant"** or **"Switch between networks"** in your phone's WiFi settings to prevent it from killing the connection due to "no internet." Check battery saving options.
- **Self-mode on Android 10 (Q) and below:** Google has disabled the automatic wireless projection startup for Android 10 and below in Android Auto versions 16.4 and higher. While Self-mode still works on newer Android versions, it is normally impossible to trigger projection on Android 10 and below directly with recent Google app updates. **Workaround:** You can still use Self-mode on these devices by starting the built-in Android Auto Headunit Server and connecting via Wi-Fi mode (loopback). See the [Troubleshooting Guide](https://github.com/andreknieriem/open-headunit/wiki/Troubleshooting#self-mode-on-android-10-q-and-below) for step-by-step instructions.
- **WiFi-Direct needs long to connect:** A user finds that this is related to Google Assistant instead of Gemini for AA. If you use Gemini on newer AA versions it just runs smooth again. No idea why this happens.
- **Stuck on Android is starting** Check your video codec in the settings and set it to h264 if you have a device which does not support h265. Some devices have a broken h265 decoder and this will cause the app to stuck on "Android is starting" and never start the projection.

## Planned
- more customization options for the UI and the app itself
- Info/Help descriptions to the settings for better understanding

## Changelog

There is now one place that records everything: [CHANGELOG-AUTOSAUCES.md](CHANGELOG-AUTOSAUCES.md) — the Autosauce fork's changes in plain English at the top, and the upstream open-headunit release history carried over from this old section at the bottom.


## Contributing

Creating release apk needs a keystore file. You can create your own keystore file using the following command in root folder:
`keytool -genkey -v -keystore headunit-release-key.jks -alias headunit-revived -keyalg RSA -keysize 2048 -validity 10000`

After that you need to set the env variables depending on your OS:
MAC:
open ~/.zshrc or ~/.bashrc

`sudo nano ~/.zshrc or sudo nano ~/.bashrc`
`export HEADUNIT_KEYSTORE_PASSWORD="YOUR_KEYSTORE_PASSWORD"
export HEADUNIT_KEY_PASSWORD="YOUR_KEY_PASSWORD"`

## Original Headunit
Headunit for Android Auto (tm)

A new car or a $600+ headunit is NOT required to enjoy the integration and distraction reduced environment of Android Auto.

This headunit app can turn a Android 4.1+ tablet supporting USB Host mode into a basic Android Auto compatible headunit.

Android, Google Play, Google Maps, Google Play Music and Android Auto are trademarks of Google Inc.

Yasurolled and Autosauce are trademarks of Yasurolled.
