# Dactyl 76 — Split-Keyboard Setup (kanata + Vial)

This folder contains everything needed to turn your two-USB-half Dactyl 76 into
a proper split keyboard with shared layers across both hands, on **macOS and
Windows**.

## The model in one sentence

> **Vial owns the base QWERTY layout. Kanata owns the layers and "glues" the
> two USB halves into one logical keyboard.**

Why kanata has to own layers: each half's Vial layers only know about that
half's keys. There is no way for one half to tell the other half "switch to
layer 1". Kanata sits above both halves at the OS level, so when you hold a
layer-trigger thumb on one side, kanata reshapes keys coming from **both**
sides.

## Folder map

| Path | What it is |
|------|------------|
| `KEYMAP.md`            | Full visual diagrams of every layer |
| `CHEATSHEET.md`        | One-page printable reference |
| `kanata/common-layers.kbd` | Shared layer definitions (cross-platform) |
| `kanata/mac.kbd`       | macOS entry config (sets Cmd-based shortcuts, includes common) |
| `kanata/windows.kbd`   | Windows entry config (sets Ctrl-based shortcuts, includes common) |
| `vial/SETUP.md`        | One-time Vial tweak (set F13 / F14 on inner thumbs) |
| `ide/BINDINGS.md`      | Hyper-key bindings to set inside VS Code / JetBrains / VS |
| `install/macos.md`     | macOS install, driver, tray app, launch at boot |
| `install/windows.md`   | Windows install, tray app, launch at boot |

## Quick start

1. **Vial**: open each half, follow `vial/SETUP.md` (5 minutes).
2. **Install kanata + driver**: follow `install/macos.md` (macOS) or
   `install/windows.md` (Windows).
3. **Run kanata** pointing at `kanata/mac.kbd` or `kanata/windows.kbd`.
4. **IDE bindings**: open `ide/BINDINGS.md` and bind the Hyper-chords inside
   each IDE you use (one-time, ~5 minutes per IDE).
5. **Learn the layers**: print the cheatsheet. Run
   `./scripts/build-cheatsheet` to render `CHEATSHEET.pdf` (and `.html`) from
   `CHEATSHEET.md`. Requires Node/npx and a Chromium-family browser
   (Chrome/Chromium/Edge/Brave). The PDF is landscape Letter, 2 pages, and
   fits a small grid of layers — print it and tape it next to your screen for
   the first few days. Refer to `KEYMAP.md` for the visual reference.

## Design summary

- **Base layer** = QWERTY, completely untouched by kanata (true passthrough).
- **NAV layer** = hold the *right* inner thumb. Right hand becomes arrows / nav,
  left hand gets clipboard shortcuts.
- **SYM layer** = hold the *left* inner thumb. Left hand becomes symbols, right
  hand becomes a numpad.
- **FN / IDE layer** = hold *both* inner thumbs. F1–F12, IDE actions (build,
  run, test, debug, format), media + brightness keys.

## Living with it

- **Built-in MacBook keyboard**: kanata ignores it via the
  `macos-dev-names-include` filter — only the two halves are remapped.
- **Tray app**: `kanata-tray` (cross-platform, see `install/*.md`) gives you a
  menubar / system-tray icon to start, stop, restart, and switch configs.
- **Disable on demand**: click the tray icon → Stop. Kanata exits cleanly and
  your halves go back to being two independent keyboards.
