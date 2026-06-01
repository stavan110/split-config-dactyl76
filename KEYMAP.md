# KEYMAP

> Per-half Vial-native keymap for the Dactyl 76 split keyboard.
> Each half is configured independently in Vial — there is no host-side
> software bridging the halves. See `README.md` for the rationale and
> `vial/SETUP.md` for the click-by-click Vial entry guide.

## Notation

Vial's keycode picker uses **QMK keycode names**. Where a key produces a
common symbol, this doc shows both the symbol and the canonical name:

- `MO(n)` — momentary layer: hold to activate layer `n` (on this half only).
- `MT(mod, kc)` — mod-tap: tap emits `kc`, hold emits the modifier `mod`.
- `LGUI` / `LCTL` / `LALT` / `LSFT` — left-side modifiers (Cmd / Ctrl / Opt / Shift on macOS).
- `LGUI(kc)` — emits Cmd+`kc` as a single keypress.
- `_______` / `KC_TRNS` — transparent: falls through to the layer below.

## Physical layout reference

Each half is a 5-column × 4-row letter grid plus a 3-key thumb cluster.
The outer pinky column (Tab, Shift, dash, etc.) is **not** changed by this
keymap — leave whatever your firmware ships with there.

```text
left half                                            right half
┌────┬────┬────┬────┬────┐                ┌────┬────┬────┬────┬────┐
│ 1  │ 2  │ 3  │ 4  │ 5  │                │ 6  │ 7  │ 8  │ 9  │ 0  │
├────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┤
│ Q  │ W  │ E  │ R  │ T  │                │ Y  │ U  │ I  │ O  │ P  │
├────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┤
│ A  │ S  │ D  │ F  │ G  │                │ H  │ J  │ K  │ L  │ ;  │
├────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┤
│ Z  │ X  │ C  │ V  │ B  │                │ N  │ M  │ ,  │ .  │ /  │
└────┴────┴────┴────┴────┘                └────┴────┴────┴────┴────┘
       thumbs (left, outer→inner):              thumbs (right, inner→outer):
           ┌────┬────┬────┐                       ┌────┬────┬────┐
           │Cmd │Bspc│SYM │                       │NAV │Spc │Ent │
           └────┴────┴────┘                       └────┴────┴────┘
```

## Layer plan

Two layers per half. Layer numbers are **local to each half** — Vial only
sees one half at a time, so "layer 1" on the left half is unrelated to
"layer 1" on the right half.

| Half  | Layer | Trigger                       | Purpose                          |
|-------|-------|-------------------------------|----------------------------------|
| Left  | 0     | always (BASE)                 | QWERTY typing + dedicated Cmd thumb |
| Left  | 1     | hold left INNER thumb         | Left-hand programming symbols + F1–F5 |
| Right | 0     | always (BASE)                 | QWERTY typing + Space + Enter/Shift thumb |
| Right | 1     | hold right INNER thumb        | Arrows, page/word/doc navigation + F6–F10 |

## How modifier chords work without a global FN layer

Modifiers live on **dedicated thumb keys**, not on letter tap-holds. That
means combining a modifier with a key on the **other half** works at the
USB/OS level — the modifier half emits `LGUI down`, the letter half emits
the letter, the OS combines them. No firmware coordination needed.

| Action               | Keys                                                       |
|----------------------|------------------------------------------------------------|
| Cmd+C / Cmd+V / etc. | hold LEFT-OUTER thumb (Cmd), tap `c` / `v` / …             |
| Cmd+ anything cross-hand (e.g. Cmd+P) | hold LEFT-OUTER thumb (Cmd), tap right-hand letter |
| Capital letter       | hold RIGHT-OUTER thumb (Shift), tap letter                 |
| Shift+arrow (select) | hold RIGHT-OUTER thumb (Shift) + hold RIGHT-INNER (NAV) + tap `h`/`j`/`k`/`l` |
| Redo (Cmd+Shift+Z)   | hold LEFT-OUTER (Cmd) + hold RIGHT-OUTER (Shift) + tap `z` |

The RIGHT-OUTER thumb is **tap-hold**: a quick tap sends Enter; a hold
sends Shift. This is the only tap-hold in the design; it lives on a thumb
(not a letter) where timing is unambiguous.

## 🔹 LEFT half — Layer 0 (BASE)

```text
┌────────┬────────┬────────┬────────┬────────┐
│   1    │   2    │   3    │   4    │   5    │
├────────┼────────┼────────┼────────┼────────┤
│   Q    │   W    │   E    │   R    │   T    │
├────────┼────────┼────────┼────────┼────────┤
│   A    │   S    │   D    │   F    │   G    │
├────────┼────────┼────────┼────────┼────────┤
│   Z    │   X    │   C    │   V    │   B    │
└────────┴────────┴────────┴────────┴────────┘
            ┌─────┬─────┬─────┐
            │ Cmd │Bspc │ SYM │     ← thumbs: outer, middle, inner
            └─────┴─────┴─────┘
```

| Physical key       | Vial keycode  | Notes                                  |
|--------------------|---------------|----------------------------------------|
| 1 – 0              | `KC_1` – `KC_0`| standard number row                   |
| Q W E R T          | `KC_Q` `KC_W` `KC_E` `KC_R` `KC_T` | standard QWERTY |
| A S D F G          | `KC_A` `KC_S` `KC_D` `KC_F` `KC_G` | **NO home-row mods** (intentional — keeps fast typing reliable) |
| Z X C V B          | `KC_Z` `KC_X` `KC_C` `KC_V` `KC_B` | standard QWERTY |
| LEFT-OUTER thumb   | `KC_LGUI`     | Cmd on macOS. On Windows after the PowerToys remap (see `install/windows.md`), this becomes LCtrl. |
| LEFT-MIDDLE thumb  | `KC_BSPC`     | Backspace                              |
| LEFT-INNER thumb   | `MO(1)`       | Hold for LEFT-SYM layer                |

## 🔹 LEFT half — Layer 1 (LEFT-SYM)

Hold the **left inner thumb** to activate. All keys produce direct
unshifted output — no chording with Shift needed.

```text
┌────────┬────────┬────────┬────────┬────────┐
│   F1   │   F2   │   F3   │   F4   │   F5   │
├────────┼────────┼────────┼────────┼────────┤
│   "    │   '    │   +    │   |    │   *    │
├────────┼────────┼────────┼────────┼────────┤
│   !    │   [    │   {    │   (    │   =    │
├────────┼────────┼────────┼────────┼────────┤
│   \    │   /    │   ?    │   -    │   &    │
└────────┴────────┴────────┴────────┴────────┘
            ┌─────┬─────┬─────┐
            │  ─  │  ─  │HELD │
            └─────┴─────┴─────┘
```

| Physical key       | Symbol | Vial keycode  | Equivalent (manual)                 |
|--------------------|--------|---------------|-------------------------------------|
| 1                  | F1     | `KC_F1`       |                                     |
| 2                  | F2     | `KC_F2`       |                                     |
| 3                  | F3     | `KC_F3`       |                                     |
| 4                  | F4     | `KC_F4`       |                                     |
| 5                  | F5     | `KC_F5`       |                                     |
| Q                  | `"`    | `KC_DQUO`     | `LSFT(KC_QUOT)`                     |
| W                  | `'`    | `KC_QUOT`     |                                     |
| E                  | `+`    | `KC_PLUS`     | `LSFT(KC_EQL)`                      |
| R                  | `\|`   | `KC_PIPE`     | `LSFT(KC_BSLS)`                     |
| T                  | `*`    | `KC_ASTR`     | `LSFT(KC_8)`                        |
| A                  | `!`    | `KC_EXLM`     | `LSFT(KC_1)`                        |
| S                  | `[`    | `KC_LBRC`     |                                     |
| D                  | `{`    | `KC_LCBR`     | `LSFT(KC_LBRC)`                     |
| F                  | `(`    | `KC_LPRN`     | `LSFT(KC_9)`                        |
| G                  | `=`    | `KC_EQL`      |                                     |
| Z                  | `\`    | `KC_BSLS`     |                                     |
| X                  | `/`    | `KC_SLSH`     |                                     |
| C                  | `?`    | `KC_QUES`     | `LSFT(KC_SLSH)`                     |
| V                  | `-`    | `KC_MINS`     |                                     |
| B                  | `&`    | `KC_AMPR`     | `LSFT(KC_7)`                        |
| LEFT-OUTER thumb   |        | `_______`     | transparent → still emits Cmd       |
| LEFT-MIDDLE thumb  |        | `_______`     | transparent → still emits Backspace |
| LEFT-INNER thumb   |        | `_______`     | held (you're already on this layer) |

### Mnemonics

- **Mirrored brackets** with the right half: `S/L = []`, `D/K = {}`, `F/J = ()`.
- **Closing brackets** (`] } )`) live on the right half — see RIGHT-SYM below.
- **Top row letters** are the "shifted-number" symbols rotated up: `" ' + | *`.
- **Bottom row** has the less-frequent symbols: `\ / ? - &`.

## 🔹 RIGHT half — Layer 0 (BASE)

```text
┌────────┬────────┬────────┬────────┬────────┐
│   6    │   7    │   8    │   9    │   0    │
├────────┼────────┼────────┼────────┼────────┤
│   Y    │   U    │   I    │   O    │   P    │
├────────┼────────┼────────┼────────┼────────┤
│   H    │   J    │   K    │   L    │   ;    │
├────────┼────────┼────────┼────────┼────────┤
│   N    │   M    │   ,    │   .    │   /    │
└────────┴────────┴────────┴────────┴────────┘
            ┌─────┬─────┬─────┐
            │ NAV │ Spc │ Ent │     ← thumbs: inner, middle, outer
            └─────┴─────┴─────┘     (outer = tap Enter, hold Shift)
```

| Physical key        | Vial keycode                | Notes                                    |
|---------------------|-----------------------------|------------------------------------------|
| 6 – 0               | `KC_6` – `KC_0`             | standard number row                      |
| Y U I O P           | `KC_Y` `KC_U` `KC_I` `KC_O` `KC_P` | standard QWERTY                    |
| H J K L             | `KC_H` `KC_J` `KC_K` `KC_L` | standard QWERTY                          |
| `;`                 | `KC_SCLN`                   |                                          |
| N M , . /           | `KC_N` `KC_M` `KC_COMM` `KC_DOT` `KC_SLSH` |                          |
| RIGHT-INNER thumb   | `MO(1)`                     | Hold for RIGHT-NAV layer                 |
| RIGHT-MIDDLE thumb  | `KC_SPC`                    | Space                                    |
| RIGHT-OUTER thumb   | `MT(MOD_LSFT, KC_ENT)`      | **tap = Enter, hold = Shift**            |

> The RIGHT-OUTER tap-hold is the only timing-sensitive key in the design.
> A quick tap (< ~200ms) sends Enter. Hold past that to use as Shift.
> Configure Vial → QMK Settings → Tapping Term: 200 ms,
> Permissive Hold: on, Hold On Other Key Press: on.

## 🔹 RIGHT half — Layer 1 (RIGHT-NAV)

Hold the **right inner thumb** to activate.

```text
┌────────┬────────┬────────┬────────┬────────┐
│   F6   │   F7   │   F8   │   F9   │  F10   │
├────────┼────────┼────────┼────────┼────────┤
│  DocB  │  Home  │  PgUp  │  End   │  DocE  │
├────────┼────────┼────────┼────────┼────────┤
│   ←    │   ↓    │   ↑    │   →    │  Esc   │
├────────┼────────┼────────┼────────┼────────┤
│  Wrd←  │  Tab   │  PgDn  │  Wrd→  │   ─    │
└────────┴────────┴────────┴────────┴────────┘
            ┌─────┬─────┬─────┐
            │HELD │  ─  │  ─  │
            └─────┴─────┴─────┘
```

| Physical key        | Action            | Vial keycode             | Notes                                              |
|---------------------|-------------------|--------------------------|----------------------------------------------------|
| 6                   | F6                | `KC_F6`                  |                                                    |
| 7                   | F7                | `KC_F7`                  |                                                    |
| 8                   | F8                | `KC_F8`                  |                                                    |
| 9                   | F9                | `KC_F9`                  |                                                    |
| 0                   | F10               | `KC_F10`                 |                                                    |
| Y                   | Doc Begin         | `LGUI(KC_UP)`            | Cmd+Up on Mac = top of document. On Windows after PowerToys, becomes Ctrl+Up — remap to Ctrl+Home in PowerToys for parity. |
| U                   | Home              | `KC_HOME`                | line begin on most platforms; doc top on some Mac apps. |
| I                   | Page Up           | `KC_PGUP`                |                                                    |
| O                   | End               | `KC_END`                 |                                                    |
| P                   | Doc End           | `LGUI(KC_DOWN)`          | Cmd+Down on Mac.                                   |
| H                   | ←                 | `KC_LEFT`                |                                                    |
| J                   | ↓                 | `KC_DOWN`                |                                                    |
| K                   | ↑                 | `KC_UP`                  |                                                    |
| L                   | →                 | `KC_RIGHT`               |                                                    |
| `;`                 | Esc               | `KC_ESC`                 | handy for modal editors                            |
| N                   | Word Left         | `LALT(KC_LEFT)`          | Opt+Left on Mac. On Windows, PowerToys remap turns this into Ctrl+Left. |
| M                   | Tab               | `KC_TAB`                 | when navigating, lets you indent without leaving the layer |
| `,`                 | Page Down         | `KC_PGDN`                |                                                    |
| `.`                 | Word Right        | `LALT(KC_RIGHT)`         | mirror of Word Left                                |
| `/`                 | (transparent)     | `_______`                | falls through to `/` on BASE                       |
| RIGHT-INNER thumb   |                   | `_______`                | held (you're already on this layer)                |
| RIGHT-MIDDLE thumb  |                   | `_______`                | still emits Space                                  |
| RIGHT-OUTER thumb   |                   | `_______`                | still emits Enter / Shift                          |

### Selection workflow

```
Hold RIGHT-OUTER (Shift) + Hold RIGHT-INNER (NAV) + tap h/j/k/l
                                                    └── one or more
```

All three keys are on the right half so timing is reliable. Tap any arrow
to extend the selection one position; combine with `LALT` (`n`/`.`) for
word-by-word selection by adding Shift via the right outer thumb.

## Cross-platform notes

The firmware emits **Mac-native** keycodes (`LGUI` = Cmd, `LALT` = Opt).
This works out of the box on macOS. For Windows, install
[PowerToys](https://learn.microsoft.com/windows/powertoys/install) and
configure Keyboard Manager with the remaps listed in
`install/windows.md`. Without those remaps, on Windows:

- LGUI shortcuts (Cmd+C, etc.) trigger the Win key — generally not what you want.
- `LALT(KC_LEFT)` / `LALT(KC_RIGHT)` invoke browser-back / forward, not word jump.
- `LGUI(KC_UP)` / `LGUI(KC_DOWN)` invoke Win-key shortcuts.

If the PowerToys trade-offs (losing native Alt+Left for browser-back,
giving up LWin shortcuts) bother you, the alternative is to maintain a
separate Vial layout for Windows — see `install/windows.md` for the
suggested per-OS overrides.
