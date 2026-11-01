# Autosauce — Change Log

This is the running, plain-English record of what changed in the Autosauce
project (the local `open-headunit` repo). It is written for a human, not for a
machine — each entry says what happened in everyday words, not just the file
list.

Newest entries are at the top. Each entry has a date, the commit's subject line
(short title), and a short "what it means" paragraph.

---

## 2026-11-01

### Ignore the Kotlin build-cache folder in git

A small housekeeping fix. When the app is compiled, the Kotlin tool leaves a
cache folder (`.kotlin/`) at the top of the project. It is just leftover build
junk, not real source code, so it is now added to the ignore list — meaning
`git` stops listing it as a loose file and no one can accidentally commit it
along with real changes.

---

### BT: complete picker, onboarding and home-bar wiring

Finishes making **Bluetooth** a usable connection option end to end. The
previous commit already put a Bluetooth button on the home screen and added the
setting; this one makes the rest of the UI agree with it:

- **Setup wizard (Onboarding):** a "Bluetooth" toggle button was added to the
  "how do you connect?" step, so a new user can pick Bluetooth as one of their
  connection types (next to USB, WiFi and Self Mode).
- **Settings:** the one-line summary of your chosen connection types, and the
  "pick your connection types" dialog, now both list Bluetooth. Two old
  comments that said "three of them" were corrected to "four of them".
- **Bottom button bar on the home screen:** the code that decides which
  connection buttons to show now understands Bluetooth, and the "shrink the
  bar to fit the screen" logic now counts the Bluetooth button too, so the row
  of buttons never overflows.
- **Help text:** the code's own comment about the connection types now reads
  "USB, Bluetooth, WiFi or Self Mode".
- **Icons:** fixed the white Bluetooth icon, whose shape data was broken
  (wrong curve commands), so it now draws as the standard Material Bluetooth
  symbol. Added the matching standard blue/white icon used by the setup-wizard
  button.
- **Words:** added the "Bluetooth" label to the app's string list.

In short: nothing new about how Bluetooth actually works — that was the last
commit. This one just makes every screen that lists the connection options
show Bluetooth too.

---

### BT: fix the "Bluetooth disabled" toast not to double-show

A one-line fix. The button that says "Bluetooth is off in your settings"
was calling the toast helper and then calling `.show()` on its result. The
helper already shows the toast by itself (and returns nothing to chain onto),
so the extra `.show()` was a mistake that stopped the app from compiling.
Removed the `.show()` so it reads like every other toast in the app.

---

### BT: add home tile, connection mode and native flow entry points

Adds **Bluetooth** as a first-class connection type to the app's settings and
puts a Bluetooth button on the home screen.

- A new `BT` value was added to the connection-types setting, so a user can
  switch Bluetooth on and off like the other options.
- A Bluetooth tile was added to the row of buttons across the bottom of the
  home screen (with its own icon and colors, including a darker variant for
  very dark themes).
- Pressing that button starts the same Bluetooth-based "driver selection"
  flow the app already uses for other connection modes. Bluetooth here is the
  way you *wake* and *pick* the connection; the actual data still rides the
  app's existing wireless path.

---

_Note on the build environment (not a code commit): this machine was missing
the Android NDK 29.0.14206865 that the project pins, so the build initially
failed. It was installed with the SDK manager (via a retry script, because the
CDN intermittently drops the download). That is an environment fix, not a
change to the app, so it is noted here for completeness but has no commit._
