# Vial — click-by-click setup

You'll do this **once per half**. Total time: ~15 minutes per half.

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
- **QMK Settings** — used for the tap-hold tuning in step 6

If you see fewer than 2 layers in the **Layers** row at the bottom of the
Keymap tab, your firmware was built without enough layers. Stop and
reflash with a Vial-enabled firmware that has at least 2 layers.

## 3. Set Layer 0 (BASE) — left half

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

### Thumb cluster (left, outer → inner)

| Physical thumb | Keycode    | Picker location                          |
|----------------|------------|------------------------------------------|
| Outer (closest to your wrist) | `KC_LGUI`  | **Basic** tab → bottom row → "Left GUI" |
| Middle         | `KC_BSPC`  | **Basic** tab → "Backspace"              |
| **Inner** (closest to the center of the keyboard) | `MO(1)` | **Layers** tab → click `MO` → enter `1` |

The `MO(1)` is the most important one — that's the trigger for the symbol
layer.

## 4. Set Layer 1 (LEFT-SYM) — left half

Click **Layer 1** at the bottom of the Keymap tab. (If you only see one
layer, click the **+** button.)

Reset the layer to all transparent first: there's a "Reset" button per
layer in some Vial versions, or you can `Shift+click` each key and pick
`KC_TRNS` (transparent) — usually shown as `_______` or an empty key. The
default for a new layer is usually already transparent.

Now fill in each position. Symbols come from the **Basic** tab → "Shifted
Symbols" section, or the **Quantum** tab if your version groups them
differently.

| Physical key | Symbol | Vial keycode | Picker location       |
|--------------|--------|--------------|------------------------|
| 1            | F1     | `KC_F1`      | **Basic** → F-row      |
| 2            | F2     | `KC_F2`      | **Basic** → F-row      |
| 3            | F3     | `KC_F3`      | **Basic** → F-row      |
| 4            | F4     | `KC_F4`      | **Basic** → F-row      |
| 5            | F5     | `KC_F5`      | **Basic** → F-row      |
| Q            | `"`    | `KC_DQUO` (or `LSFT(KC_QUOT)`) | **Basic** → Shifted Symbols |
| W            | `'`    | `KC_QUOT`    | **Basic** → punctuation |
| E            | `+`    | `KC_PLUS`    | **Basic** → Shifted Symbols |
| R            | `\|`   | `KC_PIPE`    | **Basic** → Shifted Symbols |
| T            | `*`    | `KC_ASTR`    | **Basic** → Shifted Symbols |
| A            | `!`    | `KC_EXLM`    | **Basic** → Shifted Symbols |
| S            | `[`    | `KC_LBRC`    | **Basic** → punctuation |
| D            | `{`    | `KC_LCBR`    | **Basic** → Shifted Symbols |
| F            | `(`    | `KC_LPRN`    | **Basic** → Shifted Symbols |
| G            | `=`    | `KC_EQL`     | **Basic** → punctuation |
| Z            | `\`    | `KC_BSLS`    | **Basic** → punctuation |
| X            | `/`    | `KC_SLSH`    | **Basic** → punctuation |
| C            | `?`    | `KC_QUES`    | **Basic** → Shifted Symbols |
| V            | `-`    | `KC_MINS`    | **Basic** → number row neighborhood |
| B            | `&`    | `KC_AMPR`    | **Basic** → Shifted Symbols |

Thumbs on Layer 1: leave **all three** as `KC_TRNS` (transparent / `_______`).

> **If your Vial doesn't expose shifted-symbol aliases** like `KC_DQUO`,
> use the manual equivalent — pick the **Any Key** keycode (in **Quantum**
> tab) and type `LSFT(KC_QUOT)`. Both produce the same output.

## 5. Save and test (left half)

Vial auto-saves to the keyboard EEPROM as you make changes. To verify:

1. Tap each layer-1 key while holding the **left inner thumb**. The
   symbol should appear in any text field.
2. Tap normal letters without holding the thumb — confirm normal typing
   still works.
3. Hold the **left outer thumb** + tap `c` in any text editor → should
   trigger Copy. (Won't actually copy anything because no text was
   selected — just confirm the system shortcut fires; e.g. it doesn't
   type "c".)

## 6. Tune the right outer thumb's tap-hold (when you get to the right half)

This step is needed only on the RIGHT half because that's where the
tap-hold lives. Skip until you have the right half plugged in.

Once on the right half:

1. Set the right outer thumb to `MT(MOD_LSFT, KC_ENT)` via the **Layers**
   or **Mod-Tap** picker. (In Vial: click the key, choose **MT**, then
   pick `MOD_LSFT` for the mod and `KC_ENT` for the tap.)
2. Open the **QMK Settings** tab in Vial's left sidebar.
3. Find **Tapping Term** — set it to `200`.
4. Find **Permissive Hold** — enable it.
5. Find **Hold On Other Key Press** — enable it.
6. Save settings.

Test:

- Quick tap of right outer thumb → should send Enter (e.g., insert a
  newline in a text editor).
- Hold right outer thumb + tap a letter → should send the capital letter.

If quick taps occasionally produce Shift instead of Enter, increase
**Tapping Term** by 25 ms. If holds sometimes produce Enter, decrease it
by 25 ms.

## 7. Now do the right half

Unplug the left half. Plug in the right half. Repeat steps 2 through 6,
using the RIGHT half table:

### Right half — Layer 0 (BASE)

| Physical key | Keycode |
|--------------|---------|
| 6 – 0        | `KC_6` – `KC_0` |
| Y U I O P    | `KC_Y` `KC_U` `KC_I` `KC_O` `KC_P` |
| H J K L `;`  | `KC_H` `KC_J` `KC_K` `KC_L` `KC_SCLN` |
| N M `,` `.` `/` | `KC_N` `KC_M` `KC_COMM` `KC_DOT` `KC_SLSH` |
| **Inner** thumb | `MO(1)` |
| Middle thumb | `KC_SPC` |
| **Outer** thumb | `MT(MOD_LSFT, KC_ENT)` |

### Right half — Layer 1 (RIGHT-NAV)

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
| L            | →            | `KC_RIGHT`                   | **Basic**                        |
| `;`          | Esc          | `KC_ESC`                     | **Basic**                        |
| N            | Word ←       | `LALT(KC_LEFT)`              | **Quantum** wrappers → LALT + `KC_LEFT` |
| M            | Tab          | `KC_TAB`                     | **Basic**                        |
| `,`          | Page Down    | `KC_PGDN`                    | **Basic**                        |
| `.`          | Word →       | `LALT(KC_RIGHT)`             | **Quantum** wrappers → LALT + `KC_RIGHT` |
| `/`          | (transparent) | `_______`                   |                                  |
| **All three thumbs** | (transparent) | `_______`            |                                  |

## 8. Full smoke test

With both halves plugged in:

| What to try                          | Expected                                    |
|--------------------------------------|---------------------------------------------|
| Type your name normally              | Letters appear as usual                     |
| Hold left inner thumb + tap `d`      | `{` appears                                 |
| Hold right inner thumb + tap `j`     | Cursor moves down                           |
| Hold left outer thumb + tap `c`      | Copy (Cmd+C) fires                          |
| Hold right outer thumb + tap `a`     | Capital `A` appears                         |
| Quick tap right outer thumb          | Newline (Enter)                             |
| Hold right inner + hold right outer + tap `l` | Selects one char to the right     |

If any of these fail, recheck the corresponding key in Vial.

## Troubleshooting

| Symptom                                            | Fix                                                                 |
|----------------------------------------------------|----------------------------------------------------------------------|
| Holding inner thumb does nothing                   | The thumb is still set to `KC_F14` / `KC_F13` (kanata-era) — change to `MO(1)`. |
| Right outer thumb sometimes types Shift instead of Enter | Increase **Tapping Term** by 25 ms in QMK Settings.            |
| Right outer thumb sometimes types Enter instead of Shift | Decrease **Tapping Term** by 25 ms; enable **Hold On Other Key Press**. |
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
- `dactyl_76_left_v3.vil` — this layout
- (mirror for right half)

Stash these somewhere you can sync to other machines.
