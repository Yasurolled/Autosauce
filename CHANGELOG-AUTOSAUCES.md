# Autosauce — Change Log

This is the running, plain-English record of what changed in the Autosauce
project (the local `open-headunit` repo). It is written for a human, not for a
machine — each entry says what happened in everyday words, not just the file
list.

Newest entries are at the top. Each entry has a date, the commit's subject line
(short title), and a short "what it means" paragraph.

---

## 2026-11-01

### Docs: put the contributors list in its own file, authored by the AI co-developer

The contributors list now lives in its own file, CONTRIBUTORS.md, at
the root of the repo (the README section now points to it). This
particular commit was deliberately made *under the AI co-developer's
name* (the git author is "Claude Code", not the human's machine
account) so that Claude Code shows up as a contributor in GitHub's
own contributors list for the fork, not just in the README. The
model that did the work is stated separately, as before:
Agnes-3.0-flash, developed by Sapiens AI.

---

### App: rename the user-visible name from "Open Headunit" to "Autosauce"

The fork now has its own name, so every place a user can actually
*see* the name was renamed:

- All 49 English strings in the app's text file that refer to the app
  by name now say "Autosauce" (the launcher label, the welcome
  screen, notifications, settings descriptions, and so on).
- A few names are baked into the code rather than the text files: the
  VPN settings entry ("Autosauce Offline Mode"), the vehicle name
  Android Auto shows by default ("Autosauce"), the name shown in
  Bluetooth modem identity queries, the running-notification title,
  and the wording of two Bluetooth diagnostic messages.
- The two test files that check those baked-in names were updated in
  the same change, so the tests assert the new names.
- The "report an issue" link on the About screen now points to this
  fork's GitHub issues page instead of the upstream one.

Deliberately *not* renamed: the app's internal code structure (the
`com.andrerinas.openheadunit` package and its names in files — that
would be a huge pile of changes that no user can ever see), code
comments, and the old one-time "our name is changing" notice, which
describes the upstream project's history and still says it right.

---

### README: carry the Autosauce name, fork, trademark and AI statements

The top of the documentation now matches the fork. The project is
called **Autosauce**; a new "What this is" section says in plain
words that this repository is a *fork* of open-headunit (and that
open-headunit is itself a revival of the original headunit project).
New "Trademarks", "AI assistance" and "Contributors" sections record
that Yasurolled and Autosauce are trademarks of Yasurolled, that the
fork was developed with the help of Claude AI (Anthropic's Claude
Code) with the model stated separately — Agnes-3.0-flash, developed
by Sapiens AI — and that Claude Code counts as a contributor, with
its model named separately from the agent. Every user-facing
"Open Headunit" mention in the how-to text now reads "Autosauce";
the upstream project's own history entries keep their old names. The
two app-store badges that advertised the *upstream* app were
replaced with a link to the fork's GitHub page.

---

### License: give the fork its own copyright and trademark notice

The legal paperwork for the fork. The project keeps the same open
license it started with (the GNU Affero General Public License v3) —
that part of the license file is untouched, because the upstream
code must stay under it. A short notice was added at the very top of
that same file saying: this project is the "Autosauce" fork of
open-headunit, maintained by Yasurolled; the 2026 modifications are
copyrighted to Yasurolled; and "Yasurolled" and "Autosauce" are
trademarks of Yasurolled, with the Google trademarks still noted as
before. No legal terms changed — only who it is, who owns the
modifications, and whose trademarks are which.

---

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

_Note on the test bench (not a code commit): running the project's whole
unit-test suite on this PC shows 37 failing tests, all inside seven test
files about the P2P/WiFi-direct native side and about the Settings screen's
look. The identical 37 failures also happen on a clean copy of the code
from before any of this project's work, so they are not caused by anything
done here — this machine's test runner cannot load the native libraries those
tests need. The app itself builds, and all the other ~4,374 tests pass._
