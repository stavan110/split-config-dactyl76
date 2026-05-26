# Project Context — Dactyl 76 Split Keyboard with kanata

## The hardware

- **Keyboard**: Dactyl Manuform-style 76-key ergonomic split, sourced from
  Alibaba (sometimes branded "Cycle 76" or "Dactyl 76").
- **Form factor**: concave thumb cluster, column-staggered ortholinear, ~6
  rows deep, 6 columns per hand, 6 thumb keys per side (user only uses the
  3 outer ones per side).
- **Critical quirk**: the two halves are **completely independent USB
  devices**. No TRRS cable, no GPIO bridge — each half enumerates as its own
  keyboard. There is no firmware-level way to share layer state across the
  halves.
- **Firmware**: came pre-flashed from the Alibaba seller. Vial-compatible
  on each half independently.

## The problem this setup solves

Because the halves don't talk to each other:
- A layer key pressed on the right half only affects the right half's keys.
- Holding "Layer 1" on one side does nothing for the other side.
- The OS sees two separate keyboards; layer state cannot be synchronized in
  firmware.

For the keyboard to behave like a *true* split (one shared keymap, layers
that span both hands), the layer logic must live **above** the firmware —
at the OS level, where both halves' key events are visible in a single
stream. That's what kanata does.

## Why kanata (not Karabiner-Elements, not QMK source, not BetterTouchTool)

- **Cross-platform**: same config file format works on macOS, Windows, and
  Linux. User regularly uses both macOS (this MacBook) and Windows
  (separate machine).
- **Open source, actively maintained**.
- **Configurable as text** — version-controllable, shareable between
  machines, AI-editable.
- **Supports cross-device input grabbing** — sees keys from both halves as
  one combined input stream, so cross-half layers Just Work.
- Karabiner-Elements would also work on macOS but is macOS-only.
- Re-flashing QMK with a bridge cable was rejected because the user
  doesn't want to solder or wire the halves together.

## The model

> **Vial owns the base QWERTY layout. Kanata owns the layers and "glues"
> the two USB halves into one logical keyboard.**

Specifically:
- Vial keeps every key on each half as its default QWERTY position.
- The only Vial change: the **inner** thumb key on each half is remapped to
  `F13` (right) and `F14` (left). These are unused HID keycodes on both
  macOS and Windows.
- Kanata grabs F13 / F14 and treats them as momentary layer triggers
  (`layer-while-held`). Holding F13 → NAV layer. Holding F14 → SYM layer.
  Holding both → FN/IDE layer.
- On the **base** layer, kanata is essentially transparent — every key
  passes through unchanged (`_` in deflayer). Only the two thumb triggers
  are active.

## Layer summary

| Layer | Trigger              | Purpose |
|-------|----------------------|---------|
| Base  | (nothing held)       | Pure QWERTY passthrough |
| NAV   | Right inner thumb (F13) | hjkl arrows + Home/End/PgUp/PgDn on right hand; clipboard chords on left hand |
| SYM   | Left inner thumb (F14)  | Symbols (`! @ # $ %` etc.) on left hand; numpad (7/8/9, 4/5/6, 1/2/3, 0) on right hand |
| FN    | Both inner thumbs (F13+F14) | F1–F10, IDE actions (build/run/test/debug/format), media + brightness |

See `KEYMAP.md` in the project root for full visual diagrams.

## Cross-platform strategy

Two entry-point configs share one common layer file via `(include ...)`:

```
kanata/
├── common-layers.kbd   (defsrc + all deflayers; uses @-aliases)
├── mac.kbd             (defines aliases with Cmd-based shortcuts; includes common)
└── windows.kbd         (defines aliases with Ctrl-based shortcuts; includes common)
```

Differences between the two OS files are limited to:
- Clipboard chords: `M-z` (Cmd+Z) vs `C-z` (Ctrl+Z)
- Word-jump: `A-left` (Opt+Left on Mac) vs `C-left` (Ctrl+Left on Windows)
- Device filtering syntax (`macos-dev-names-include` vs
  `windows-only-windows-interception-keyboard-hwids`)

Everything else — the layer structure, key positions, IDE Hyper chords —
is identical and lives in `common-layers.kbd`.

## IDE Hyper-chord strategy

The FN layer sends `Hyper+letter` chords (Cmd+Ctrl+Opt+Shift+letter on
macOS; Win+Ctrl+Alt+Shift+letter on Windows) for IDE actions:

- `Hyper+B` = Build
- `Hyper+R` = Run
- `Hyper+T` = Test
- `Hyper+D` = Debug
- `Hyper+F` = Format
- `Hyper+G` = Go to definition
- `Hyper+U` = Find usages

User binds these once inside each IDE (VS Code, JetBrains, Visual Studio
2026, Neovim). See `ide/BINDINGS.md` for exact menu paths and JSON snippets.

Hyper chords were chosen because they **never conflict** with OS shortcuts
or default app bindings on either macOS or Windows.

## Built-in keyboard handling

The user wants kanata to ignore the laptop's built-in keyboard so it works
normally when the split is unplugged or when typing on it directly:

- **macOS**: `macos-dev-names-include` in `mac.kbd` restricts kanata to
  just the two halves by device name.
- **Windows**: `windows-only-windows-interception-keyboard-hwids` in
  `windows.kbd` does the same by HWID (Wintercept build only).
- **Both OSes**: a tray app (`kanata-tray` by rszyma) provides one-click
  stop/start from the menubar/system tray, plus a "OFF" preset.

## What kanata is NOT doing (deliberately)

- ❌ Home-row mods (rejected as too aggressive for now)
- ❌ Tap-dance / combos (Vial handles per-half ones if needed)
- ❌ Touching base-layer key behavior
- ❌ Mouse keys
- ❌ Modifying the laptop's built-in keyboard
