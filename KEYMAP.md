# KEYMAP

> Per-half Vial-native keymap for the Dactyl 76 split keyboard.
> **v4** replaces v3 with a dev-workflow layout: dedicated Alt/Option,
> plain Shift, debugger function keys, and programming symbols. Each half is
> configured independently in Vial — there is no host-side software bridging
> the halves. Each half is an independent USB device, so the halves cannot
> share layer state. See `README.md` for the design rationale and
> `CHEATSHEET.md` for the daily-use view.

## Notation

- `KC_*` — normal QMK/Vial keycode.
- `MO(1)` — momentary layer hold, local to that half only.
- `LGUI(kc)` — send key with left GUI/Cmd held.
- `LCTL`, `LALT`, `LSFT` — Ctrl, Alt/Option, Shift modifiers.
- `_______` / `KC_TRNS` — transparent, fall through to base layer.

## v4 highlights

- **Dedicated Alt/Option thumb** on LEFT-MIDDLE (`KC_LALT`) — new in v4,
  replacing the v3 Backspace thumb.
- **Dedicated Shift thumb** on RIGHT-OUTER (`KC_LSFT`) — plain Shift, no
  tap-hold timing dependency.
- **F11/F12 on RIGHT-NAV** for debugger / IDE workflows.
- **Programming symbols are direct on LEFT-SYM**: closing brackets, angles,
  colon, at-sign, and underscore are all one layer hold away.
- **Backspace and Enter live on RIGHT-NAV**: `;` = Backspace, `N` = Enter.
- v3's firmware word-jump macros are dropped; word jumps are now normal
  modifier chords using the dedicated Alt/Ctrl modifier plus NAV arrows.

## Physical layout reference

Each half is a 5-column × 4-row letter grid plus one number row and a 3-key
thumb cluster. The outer pinky column (Tab, Shift, dash, etc.) is not changed
by this keymap and can remain whatever your current firmware exposes.

Left half:

```text
┌────┬────┬────┬────┬────┐
│ 1  │ 2  │ 3  │ 4  │ 5  │
├────┼────┼────┼────┼────┤
│ Q  │ W  │ E  │ R  │ T  │
├────┼────┼────┼────┼────┤
│ A  │ S  │ D  │ F  │ G  │
├────┼────┼────┼────┼────┤
│ Z  │ X  │ C  │ V  │ B  │
└────┴────┴────┴────┴────┘
        [Cmd] [Alt] [SYM]
```

Right half:

```text
┌────┬────┬────┬────┬────┐
│ 6  │ 7  │ 8  │ 9  │ 0  │
├────┼────┼────┼────┼────┤
│ Y  │ U  │ I  │ O  │ P  │
├────┼────┼────┼────┼────┤
│ H  │ J  │ K  │ L  │ ;  │
├────┼────┼────┼────┼────┤
│ N  │ M  │ ,  │ .  │ /  │
└────┴────┴────┴────┴────┘
[NAV] [Spc] [Sft]
```

## Layer plan

| Half | Layer | Activation | Purpose |
|---|---:|---|---|
| Left | 0 | Always | Left QWERTY half, number row, Cmd + Alt thumbs |
| Left | 1 | Hold left inner thumb | LEFT-SYM: programming symbols + F1–F5 |
| Right | 0 | Always | Right QWERTY half, number row, Space + Shift thumbs |
| Right | 1 | Hold right inner thumb | RIGHT-NAV: navigation, Backspace/Enter, F6–F12 |

## How modifier chords work without a global FN layer

Because the halves cannot share layer state, layer holds are intentionally
local: left inner thumb only changes the left half, and right inner thumb only
changes the right half. Cross-half shortcuts are built from normal OS-visible
modifiers instead.

| Action | Physical chord |
|---|---|
| Cmd + letter | Hold LEFT-OUTER + tap any letter |
| Alt + key | Hold LEFT-MIDDLE + tap any key |
| Shift + letter | Hold RIGHT-OUTER + tap letter |
| Cmd + Shift + letter | Hold LEFT-OUTER + RIGHT-OUTER + tap letter |
| Cmd + Opt + L | Hold LEFT-OUTER + LEFT-MIDDLE + tap `L` |
| Alt + Enter | Hold LEFT-MIDDLE + hold RIGHT-INNER + tap `N` |
| Shift + arrow | Hold RIGHT-OUTER + hold RIGHT-INNER + tap `H/J/K/L` |
| Word jump (Mac) | Hold LEFT-MIDDLE + hold RIGHT-INNER + tap `H/L` |
| Word jump (Windows after PowerToys) | Hold LEFT-OUTER + hold RIGHT-INNER + tap `H/L` |
| Backspace | Hold RIGHT-INNER + tap `;` |
| Enter | Hold RIGHT-INNER + tap `N` (or your right pinky outer Enter, if firmware exposes one) |

RIGHT-OUTER is plain `KC_LSFT` in v4. Enter moved to RIGHT-NAV `N`, and
Backspace moved to RIGHT-NAV `;`, so there is no tap-hold timing dependency.

## LEFT half — Layer 0

Base layer is ordinary left-hand QWERTY.

```text
┌────┬────┬────┬────┬────┐
│ 1  │ 2  │ 3  │ 4  │ 5  │
├────┼────┼────┼────┼────┤
│ Q  │ W  │ E  │ R  │ T  │
├────┼────┼────┼────┼────┤
│ A  │ S  │ D  │ F  │ G  │
├────┼────┼────┼────┼────┤
│ Z  │ X  │ C  │ V  │ B  │
└────┴────┴────┴────┴────┘
        [Cmd] [Alt] [SYM]
```

| Position | Keycode | Notes |
|---|---|---|
| 1–5 | `KC_1` – `KC_5` | Number row |
| Q W E R T | `KC_Q` `KC_W` `KC_E` `KC_R` `KC_T` | Normal letters |
| A S D F G | `KC_A` `KC_S` `KC_D` `KC_F` `KC_G` | Normal letters |
| Z X C V B | `KC_Z` `KC_X` `KC_C` `KC_V` `KC_B` | Normal letters |
| Thumb outer | `KC_LGUI` | Cmd on Mac. On Windows, PowerToys remaps to LCtrl. |
| Thumb middle | `KC_LALT` | Alt/Option. New in v4. |
| Thumb inner | `MO(1)` | Hold for LEFT-SYM. |

## LEFT half — Layer 1

Hold left inner thumb for LEFT-SYM.

```text
┌────┬────┬────┬────┬────┐
│ F1 │ F2 │ F3 │ F4 │ F5 │
├────┼────┼────┼────┼────┤
│ "  │ '  │ :  │ @  │ _  │
├────┼────┼────┼────┼────┤
│ !  │ [  │ {  │ (  │ =  │
├────┼────┼────┼────┼────┤
│ <  │ >  │ ]  │ )  │ }  │
└────┴────┴────┴────┴────┘
        [ ─ ] [ ─ ] [ ─ ]
```

Thumbs on Layer 1 are all `KC_TRNS` / transparent.

| Position | Output | Vial keycode | Equivalent manual chord |
|---|---|---|---|
| 1 | `F1` | `KC_F1` | — |
| 2 | `F2` | `KC_F2` | — |
| 3 | `F3` | `KC_F3` | — |
| 4 | `F4` | `KC_F4` | — |
| 5 | `F5` | `KC_F5` | — |
| Q | `"` | `KC_DQUO` | `LSFT(KC_QUOT)` |
| W | `'` | `KC_QUOT` | `KC_QUOT` |
| E | `:` | `KC_COLN` | `LSFT(KC_SCLN)` |
| R | `@` | `KC_AT` | `LSFT(KC_2)` |
| T | `_` | `KC_UNDS` | `LSFT(KC_MINS)` |
| A | `!` | `KC_EXLM` | `LSFT(KC_1)` |
| S | `[` | `KC_LBRC` | `KC_LBRC` |
| D | `{` | `KC_LCBR` | `LSFT(KC_LBRC)` |
| F | `(` | `KC_LPRN` | `LSFT(KC_9)` |
| G | `=` | `KC_EQL` | `KC_EQL` |
| Z | `<` | `KC_LT` | `LSFT(KC_COMM)` |
| X | `>` | `KC_GT` | `LSFT(KC_DOT)` |
| C | `]` | `KC_RBRC` | `KC_RBRC` |
| V | `)` | `KC_RPRN` | `LSFT(KC_0)` |
| B | `}` | `KC_RCBR` | `LSFT(KC_RBRC)` |
| Thumb outer | `KC_TRNS` | `KC_TRNS` | transparent |
| Thumb middle | `KC_TRNS` | `KC_TRNS` | transparent |
| Thumb inner | `KC_TRNS` | `KC_TRNS` | transparent while held |

## Mnemonics

- Number row becomes F1–F5.
- Quotes and common punctuation sit on the top alpha row: `" ' : @ _`.
- Openers are on the home row and closers are below them: `[ { (` / `] ) }`.
- Angles are on `Z/X`; equals stays under the index finger on `G`.
- Symbols are direct outputs — no Shift chording required.

## RIGHT half — Layer 0

Base layer is ordinary right-hand QWERTY.

```text
┌────┬────┬────┬────┬────┐
│ 6  │ 7  │ 8  │ 9  │ 0  │
├────┼────┼────┼────┼────┤
│ Y  │ U  │ I  │ O  │ P  │
├────┼────┼────┼────┼────┤
│ H  │ J  │ K  │ L  │ ;  │
├────┼────┼────┼────┼────┤
│ N  │ M  │ ,  │ .  │ /  │
└────┴────┴────┴────┴────┘
[NAV] [Spc] [Sft]
```

| Position | Keycode | Notes |
|---|---|---|
| 6–0 | `KC_6` – `KC_0` | Number row |
| Y U I O P | `KC_Y` `KC_U` `KC_I` `KC_O` `KC_P` | Normal letters |
| H J K L ; | `KC_H` `KC_J` `KC_K` `KC_L` `KC_SCLN` | Normal letters / semicolon |
| N M , . / | `KC_N` `KC_M` `KC_COMM` `KC_DOT` `KC_SLSH` | Normal letters / punctuation |
| Thumb inner | `MO(1)` | Hold for RIGHT-NAV. |
| Thumb middle | `KC_SPC` | Space. |
| Thumb outer | `KC_LSFT` | Plain Shift. New in v4. |

## RIGHT half — Layer 1

Hold right inner thumb for RIGHT-NAV.

```text
┌──────┬──────┬──────┬──────┬──────┐
│ F6   │ F7   │ F8   │ F9   │ F10  │
├──────┼──────┼──────┼──────┼──────┤
│ DocB │ Home │ PgUp │ End  │ DocE │
├──────┼──────┼──────┼──────┼──────┤
│ ←    │ ↓    │ ↑    │ →    │ Bspc │
├──────┼──────┼──────┼──────┼──────┤
│ Ent  │ Tab  │ F11  │ F12  │ Esc  │
└──────┴──────┴──────┴──────┴──────┘
[ ─ ]  [ ─ ]  [ ─ ]
```

Thumbs on Layer 1 are all `KC_TRNS` / transparent.

| Position | Function | Vial keycode | Notes |
|---|---|---|---|
| 6 | F6 | `KC_F6` | Function row |
| 7 | F7 | `KC_F7` | Function row |
| 8 | F8 | `KC_F8` | Function row |
| 9 | F9 | `KC_F9` | Function row |
| 0 | F10 | `KC_F10` | Function row |
| Y | Document begin | `LGUI(KC_UP)` | Cmd+↑ on Mac |
| U | Home | `KC_HOME` | Line begin |
| I | Page Up | `KC_PGUP` | Page up |
| O | End | `KC_END` | Line end |
| P | Document end | `LGUI(KC_DOWN)` | Cmd+↓ on Mac |
| H | Left arrow | `KC_LEFT` | Vim-style arrows |
| J | Down arrow | `KC_DOWN` | Vim-style arrows |
| K | Up arrow | `KC_UP` | Vim-style arrows |
| L | Right arrow | `KC_RGHT` | Vim-style arrows |
| ; | Backspace | `KC_BSPC` | New position in v4 |
| N | Enter | `KC_ENT` | New position in v4 |
| M | Tab | `KC_TAB` | Tab |
| , | F11 | `KC_F11` | Debug / IDE key |
| . | F12 | `KC_F12` | Debug / IDE key |
| / | Escape | `KC_ESC` | Relocated from `;` |
| Thumb inner | `KC_TRNS` | `KC_TRNS` | transparent while held |
| Thumb middle | `KC_TRNS` | `KC_TRNS` | transparent → Space |
| Thumb outer | `KC_TRNS` | `KC_TRNS` | transparent → Shift |

## Selection workflow

Right-hand NAV keeps movement on `H/J/K/L` while the right thumb supplies Shift.

- Move cursor: hold RIGHT-INNER, tap `H/J/K/L`.
- Select by character / line: hold RIGHT-OUTER + RIGHT-INNER, tap
  `H/J/K/L`.
- Word jump on Mac: hold LEFT-MIDDLE + RIGHT-INNER, tap `H/L`.
- Word jump on Windows after PowerToys: hold LEFT-OUTER + RIGHT-INNER,
  tap `H/L`.
- Backspace: hold RIGHT-INNER, tap `;`.
- Enter: hold RIGHT-INNER, tap `N`.

This avoids same-half layer sharing assumptions: the right half handles NAV,
while left-hand modifiers remain ordinary OS modifiers.

## Cross-platform notes

The firmware emits Mac-native keycodes:

- `KC_LGUI` is Cmd on macOS.
- `KC_LALT` is Option/Alt on macOS.
- `LGUI(KC_UP)` / `LGUI(KC_DOWN)` are document begin/end on macOS.

On Windows, install the PowerToys Keyboard Manager remaps from
`install/windows.md`. The important table remaps `Win` to `Ctrl`, keeps the
Mac-style document shortcuts usable, and redirects browser-style Alt+Arrow
shortcuts to Ctrl+Arrow where needed.

Without PowerToys on Windows:

- `KC_LGUI` triggers the Windows key instead of Ctrl.
- `LGUI(KC_UP)` / `LGUI(KC_DOWN)` trigger Windows snap/minimize shortcuts.
- Word jumps should use the post-PowerToys recipe above, not a firmware macro.
