# Autosauce — Change Log

This is the running, plain-English record of what changed in the Autosauce
project (the local `open-headunit` repo). It is written for a human, not for a
machine — each entry says what happened in everyday words, not just the file
list.

Newest entries are at the top. Each entry has a date, the commit's subject line
(short title), and a short "what it means" paragraph.

---

## 2026-11-01

### Tests: repair the home-screen rule tests that Bluetooth had quietly broken

A housekeeping fix to the test suite, no change to the app itself. The
test file that checks the rules for "which home-screen button stands for
which connection" was written back when there were three connection
types (USB, WiFi and Self Mode). When Bluetooth was added as the fourth,
four of those tests kept expecting the old, smaller list of bottom-bar
buttons — so anyone running the test suite saw four failures. The app
behaviour was right all along (the bar really does carry the Bluetooth
button now); the tests just described the old three-connection world.
The four expectations were updated to include Bluetooth, and one new
test was added for the mirror case: on a unit where only Bluetooth is
on, the main button is Bluetooth and the bar is empty. The whole file
is green again (35 tests, 0 failures).

---

### Customization: add the missing color row for the Bluetooth button

Finishes the Bluetooth work on the "Colors" screen of the Settings menu.
That screen already let you pick custom colors for the Self Mode, USB and
WiFi home-screen buttons, but had no row for Bluetooth — even though the
setting for its color already existed in the code. Now there is a
"Bluetooth Button" row with its own little color swatch (a cyan circle, so
you can tell it apart from the blue/orange/purple ones), and tapping it
opens the same color picker as the other rows. Like the other rows, it
only shows when Bluetooth is one of your connection types. The "Reset all
button colors" button now also clears the Bluetooth color, and the small
home-screen picture at the top of the screen shows the chosen color too
(that part already understood Bluetooth from the earlier commits).

---

### Material You: add an opt-in "dynamic color" toggle

First step of the Material Expressive look. A new switch, found on the
Appearance screen (next to "Use Extreme Dark"), lets you make the app's
buttons and panels follow your phone's wallpaper colors — the "Material You"
feature from Android 12. It is **off by default**, so the app looks exactly
the same until you turn it on. On phones older than Android 12 the switch
does nothing (those systems have no wallpaper palette), and the app's own home
screen colors are never touched — only the standard Android controls adopt
the palette.

---

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
