# Vial — click-by-click setup

You'll do this **once per half**. Total time: ~15 minutes per half.

This guide is for the **v4** keymap — the dev-friendly redesign for daily
C# / .NET / IDE work. If you're migrating from v3 mid-walkthrough: Backspace
and Enter move off the thumbs onto the NAV layer, Alt is now a dedicated left
thumb, and Shift is now a dedicated right thumb (plain Shift, no tap-hold).

> Already familiar with Vial? Just skim `KEYMAP.md` and enter the
> per-position tables there. This guide spells out every click for first
> timers.

## Before you start

- Install Vial — see `../install/macos.md` or `../install/windows.md`.
- Plug in **one half only** (the LEFT half first). The other half stays
  unplugged so you don't get confused about which one Vial is editing.

## 1. Verify Vial sees the half

Open Vial. The top of the window has a keyboard selector. You should see
something like `Dactyl_76_L` (or whatever your firmware reports). If you
see two entries you forgot to unplug one half — unplug the other now.

## 2. Confirm firmware capability (one-time)

In Vial's **left sidebar**, you should see at minimum these tabs:

- **Keymap** — the visual layout grid
- **Layers** — layer toggles (you need at least **2**)
- **Macros** — not used in this design, but the tab should exist

If you see fewer than 2 layers in the **Layers** row at the bottom of the
Keymap tab, your firmware was built without enough layers. Stop and
reflash with a Vial-enabled firmware that has at least 2 layers.

The keymap needs plain modifiers (`KC_LALT`, `KC_LSFT`), `MO()` layer keys,
and Quantum modifier wrappers (`LGUI(KC_UP)`, `LGUI(KC_DOWN)`). v4 has **no**
tap-hold keys, so Mod-Tap / tapping-term support is not critical anymore.

## 3. Verify Layer 0 (BASE) is QWERTY — left half

Click **Layer 0** at the bottom of the Keymap tab.

For each physical position, click the on-screen key, then in the keycode
picker on the right, navigate to the listed tab and pick the keycode.

### Letter and number keys

These are probably already correct — your QWERTY is fine. Quick
verification:

| Position | Keycode | Picker location           |
|----------|---------|---------------------------|
| 1 – 5    | `KC_1` – `KC_5` | **Basic** tab → number row |
| Q W E R T | `KC_Q`–`KC_T`  | **Basic** tab → letters    |
| A S D F G | `KC_A`–`KC_G`  | **Basic** tab → letters    |
| Z X C V B | `KC_Z`–`KC_B`  | **Basic** tab → letters    |

If they aren't, set them. The keymap depends on standard QWERTY positions.

## 4. LEFT half thumb cluster

Still on **Layer 0**, set the left thumb cluster (outer → inner):

| Physical thumb | Keycode    | Picker location                          |
|----------------|------------|------------------------------------------|
| Outer (closest to your wrist) | `KC_LGUI`  | **Basic** tab → Modifiers → "Left GUI" |
| Middle         | `KC_LALT`  | **Basic** tab → Modifiers → "Left Alt"  |
| **Inner** (closest to the center of the keyboard) | `MO(1)` | **Layers** tab → click `MO` → enter `1` |

The middle thumb is the v3-Backspace position — in v4 it becomes dedicated
Alt. The `MO(1)` is the trigger for the symbol layer.

## 5. Set Layer 1 (LEFT-SYM) — left half

Click **Layer 1** at the bottom of the Keymap tab. (If you only see one
layer, click the **+** button.)

Reset the layer to all transparent first: there's a "Reset" button per
layer in some Vial versions, or you can `Shift+click` each key and pick
`KC_TRNS` (transparent) — usually shown as `_______` or an empty key. The
default for a new layer is usually already transparent.

Now fill in each position. F-row keys come from **Basic** → F-row. Symbols
come from **Basic** → "Shifted Symbols", or the **Quantum** tab if your
version groups aliases like `KC_DQUO` differently.

| Physical key | Symbol | Vial keycode | Picker location       |
|--------------|--------|--------------|------------------------|
| 1            | F1     | `KC_F1`      | **Basic** → F-row      |
| 2            | F2     | `KC_F2`      | **Basic** → F-row      |
| 3            | F3     | `KC_F3`      | **Basic** → F-row      |
| 4            | F4     | `KC_F4`      | **Basic** → F-row      |
| 5            | F5     | `KC_F5`      | **Basic** → F-row      |
| Q            | `"`    | `KC_DQUO`    | **Basic** → Shifted Symbols |
| W            | `'`    | `KC_QUOT`    | **Basic** → punctuation |
| E            | `:`    | `KC_COLN`    | **Basic** → Shifted Symbols |
| R            | `@`    | `KC_AT`      | **Basic** → Shifted Symbols |
| T            | `_`    | `KC_UNDS`    | **Basic** → Shifted Symbols |
| A            | `!`    | `KC_EXLM`    | **Basic** → Shifted Symbols |
| S            | `[`    | `KC_LBRC`    | **Basic** → punctuation |
| D            | `{`    | `KC_LCBR`    | **Basic** → Shifted Symbols |
| F            | `(`    | `KC_LPRN`    | **Basic** → Shifted Symbols |
| G            | `=`    | `KC_EQL`     | **Basic** → punctuation |
| Z            | `<`    | `KC_LT`      | **Basic** → Shifted Symbols |
| X            | `>`    | `KC_GT`      | **Basic** → Shifted Symbols |
| C            | `]`    | `KC_RBRC`    | **Basic** → punctuation |
| V            | `)`    | `KC_RPRN`    | **Basic** → Shifted Symbols |
| B            | `}`    | `KC_RCBR`    | **Basic** → Shifted Symbols |

Thumbs on Layer 1: leave **all three** as `KC_TRNS` (transparent / `_______`).

> **If your Vial doesn't expose shifted-symbol aliases** like `KC_DQUO`,
> use the manual equivalent — pick the **Any Key** keycode (in **Quantum**
> tab) and type the alias directly. Vial/QMK will store the same output.

## 6. RIGHT half thumb cluster

Unplug the left half. Plug in the right half. Repeat step 3's QWERTY check
for the right-hand grid:

| Physical key | Keycode |
|--------------|---------|
| 6 – 0        | `KC_6` – `KC_0` |
| Y U I O P    | `KC_Y` `KC_U` `KC_I` `KC_O` `KC_P` |
| H J K L `;`  | `KC_H` `KC_J` `KC_K` `KC_L` `KC_SCLN` |
| N M `,` `.` `/` | `KC_N` `KC_M` `KC_COMM` `KC_DOT` `KC_SLSH` |

Then set the right thumb cluster (inner → outer):

| Physical thumb | Keycode    | Picker location                          |
|----------------|------------|------------------------------------------|
| **Inner** (closest to the center of the keyboard) | `MO(1)` | **Layers** tab → click `MO` → enter `1` |
| Middle         | `KC_SPC`   | **Basic** tab → "Space"                  |
| Outer          | `KC_LSFT`  | **Basic** tab → Modifiers → "Left Shift" |

The outer thumb is plain Shift now — not `MT(MOD_LSFT, KC_ENT)`.

## 7. Set Layer 1 (RIGHT-NAV) — right half

Click **Layer 1** at the bottom of the Keymap tab. Reset it to transparent
first, same as the left half, then fill in each position:

| Physical key | Action       | Keycode                      | Picker location                  |
|--------------|--------------|------------------------------|----------------------------------|
| 6            | F6           | `KC_F6`                      | **Basic** → F-row                |
| 7            | F7           | `KC_F7`                      |                                  |
| 8            | F8           | `KC_F8`                      |                                  |
| 9            | F9           | `KC_F9`                      |                                  |
| 0            | F10          | `KC_F10`                     |                                  |
| Y            | Doc Begin    | `LGUI(KC_UP)`                | **Quantum** → Modifier wrappers → LGUI, then pick `KC_UP` |
| U            | Home         | `KC_HOME`                    | **Basic**                        |
| I            | Page Up      | `KC_PGUP`                    | **Basic**                        |
| O            | End          | `KC_END`                     | **Basic**                        |
| P            | Doc End      | `LGUI(KC_DOWN)`              | **Quantum** wrappers → LGUI + `KC_DOWN` |
| H            | ←            | `KC_LEFT`                    | **Basic**                        |
| J            | ↓            | `KC_DOWN`                    | **Basic**                        |
| K            | ↑            | `KC_UP`                      | **Basic**                        |
| L            | →            | `KC_RGHT`                    | **Basic**                        |
| `;`          | Backspace    | `KC_BSPC`                    | **Basic**                        |
| N            | Enter        | `KC_ENT`                     | **Basic**                        |
| M            | Tab          | `KC_TAB`                     | **Basic**                        |
| `,`          | F11          | `KC_F11`                     | **Basic** → F-row                |
| `.`          | F12          | `KC_F12`                     | **Basic** → F-row                |
| `/`          | Esc          | `KC_ESC`                     | **Basic**                        |
| **All three thumbs** | (transparent) | `KC_TRNS` (`_______`) |                                  |

The v3 word-jump macros are gone from the NAV layer. Word-jump is now the
dedicated Alt thumb + an arrow on macOS, or the Cmd/Ctrl thumb + arrow on
Windows after the PowerToys remap.

## 8. Full smoke test

With both halves plugged in:

| What to try                          | Expected                                    |
|--------------------------------------|---------------------------------------------|
| Type your name normally              | Letters appear as usual                     |
| Hold left inner thumb + tap `d`      | `{` appears                                 |
| Hold left inner thumb + tap `v`      | `)` appears                                 |
| Hold left inner thumb + tap `z`      | `<` appears                                 |
| Hold right inner thumb + tap `j`     | Cursor moves down                           |
| Hold right inner thumb + tap `;`     | Backspace fires (deletes a character)       |
| Hold right inner thumb + tap `,`     | F11 fires (use a tool that responds to F11) |
| Hold left outer thumb + tap `c`      | Copy (Cmd+C) fires                          |
| Hold right outer thumb + tap `a`     | Capital `A` appears                         |
| Hold left middle thumb + tap `a`     | Alt+A fires (on Mac, this produces `å`)     |
| Hold left middle thumb + hold right inner thumb + tap `n` | Alt+Enter fires (test in JetBrains/VSCode in a code file with a quick-fix available) |

If any of these fail, recheck the corresponding key in Vial.

## Troubleshooting

| Symptom                                            | Fix                                                                 |
|----------------------------------------------------|----------------------------------------------------------------------|
| Holding inner thumb does nothing                   | The thumb is still set to `KC_F14` / `KC_F13` (kanata-era) — change to `MO(1)`. |
| Holding left middle thumb produces Backspace       | It's still set to v3 `KC_BSPC` — change to `KC_LALT`.                |
| Right outer thumb produces Enter as a hold         | It's still set to v3 `MT(MOD_LSFT, KC_ENT)` — change to plain `KC_LSFT`. |
| Pressing `;` produces semicolon during nav         | Confirm you're holding the right inner thumb (`MO(1)`). On Layer 1, `;` is Backspace. |
| Hold left outer + tap right hand letter doesn't fire Cmd+letter | Verify left outer is `KC_LGUI` (plain), not a mod-tap.    |
| Layer changes don't persist after unplugging       | Vial only saves to EEPROM on explicit changes — try clicking save in the menu. Some firmware needs `File → Save → To Keyboard`. |
| Symbols on LEFT-SYM produce wrong character        | Your OS is probably not US ANSI layout — check System Settings → Keyboard → Input Sources. |

## Two saved layouts (optional)

If you want to keep your previous QWERTY-only layout as a fallback,
**File → Save As…** before making changes. Vial saves the entire layout
(both layers, all settings) to a `.vil` JSON file. You can reload it via
**File → Load** any time.

Suggested naming:
- `dactyl_76_left_qwerty.vil` — your factory layout
- `dactyl_76_left_v4.vil` — this layout
- `dactyl_76_right_v4.vil` — mirror for the right half

Stash these somewhere you can sync to other machines.
