# Dactyl 76 — Split-Keyboard Vial Setup

A pure-firmware Vial config for the **Dactyl 76 from Alibaba** — the
split keyboard whose halves each enumerate as their own USB device and
do not talk to each other.

## The model in one sentence

> **Each half holds its own 2-layer Vial keymap. Modifiers live on
> dedicated thumb keys so cross-hand chords (Cmd+P, Shift+Down, etc.)
> work at the USB/OS level without any host-side software.**

No drivers. No daemons. No menu-bar app. Same firmware on every OS.

## Why not kanata anymore

The earlier design routed both halves through [kanata](https://github.com/jtroo/kanata)
so they could share global layers. On macOS, kanata needs
Karabiner DriverKit, which causes a known regression: the **built-in
MacBook `fn` key stops working** while the Karabiner virtual keyboard is
loaded (kanata [#975](https://github.com/jtroo/kanata/issues/975)). No
software fix. The previous configs are preserved in `legacy/` for
reference.

## Folder map

| Path                  | What it is                                                              |
|-----------------------|-------------------------------------------------------------------------|
| `KEYMAP.md`           | Full per-key layout with Vial keycodes — the source of truth            |
| `CHEATSHEET.md`       | Printable one-page reference                                            |
| `CHEATSHEET.pdf`      | PDF rendering of the cheatsheet (regen with `scripts/build-cheatsheet`) |
| `vial/SETUP.md`       | Click-by-click guide for entering this layout in Vial                   |
| `install/macos.md`    | Install Vial on macOS                                                   |
| `install/windows.md`  | Install Vial + PowerToys (for cross-platform shortcut parity)           |
| `scripts/build-cheatsheet` | Render `CHEATSHEET.md` → PDF                                       |
| `legacy/`             | Old kanata + Karabiner setup, kept for reference                        |

## Quick start

1. **Install Vial** on the machine where you'll configure the keyboard:
   - macOS: `install/macos.md`
   - Windows: `install/windows.md` (also covers PowerToys for shortcut parity)
2. **Plug in one half** at a time.
3. **Follow `vial/SETUP.md`** to enter the keymap. Repeat for the other half.
4. **Print** `CHEATSHEET.pdf` (or regenerate it: `./scripts/build-cheatsheet`).

## Design summary

- **Two layers per half** — BASE + one momentary layer triggered by that
  half's inner thumb.
- **LEFT half momentary layer** = left-hand programming symbols (`" ' + |
  *`, `! [ { ( =`, `\ / ? - &`) + F1–F5.
- **RIGHT half momentary layer** = arrows on `h/j/k/l`, page/line/word/doc
  jumps, + F6–F10.
- **Modifiers on dedicated thumb keys**:
  - LEFT-OUTER thumb = `KC_LGUI` (Cmd on macOS).
  - RIGHT-OUTER thumb = `MT(MOD_LSFT, KC_ENT)` — tap = Enter, hold = Shift.
- **No home-row mods** — they're unreliable across independent USB halves.
- **No IDE Hyper chords** — use your editor's command palette / native shortcuts instead.

See `KEYMAP.md` for the full layer-by-layer breakdown.

## Living with it

- **Switching machines**: Vial saves to keyboard EEPROM. The firmware
  travels with the keyboard. Just plug into the new machine.
- **Disabling the layout**: open Vial → Configurator → Reset → restore
  factory layout, or maintain a second saved layout that is plain QWERTY
  and switch with `File → Load`.
- **Tuning the tap-hold**: the right outer thumb (tap Enter / hold Shift)
  is the only tap-hold key. If it feels off, adjust Vial → QMK Settings →
  Tapping Term (default 200 ms), and turn on Permissive Hold + Hold On
  Other Key Press.
