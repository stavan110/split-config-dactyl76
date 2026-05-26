# Current State

_Last updated: 2026-05-25 (tray-based setup complete on macOS)_

## ✅ Completed (macOS)

- [x] Diagnosed the keyboard: two independent USB halves, no inter-half
      communication, came pre-flashed from Alibaba.
- [x] Chose kanata + kanata-tray as the cross-platform stack.
- [x] Designed the layer scheme: Base / NAV / SYM / FN.
- [x] Wrote the cross-platform kanata config:
  - `kanata/common-layers.kbd`
  - `kanata/mac.kbd`  (filled in real device names)
  - `kanata/windows.kbd`
- [x] Validated all `.kbd` configs with `kanata --check`.
- [x] Documented the keymap visually in `KEYMAP.md`.
- [x] Wrote `CHEATSHEET.md`, `ide/BINDINGS.md`, `install/macos.md`,
      `install/windows.md`, `vial/SETUP.md`.
- [x] Set up `.llm/` handoff bundle.
- [x] **Installed kanata** via Homebrew (v1.11.0 → `/opt/homebrew/bin/kanata`).
- [x] **Installed Karabiner-DriverKit-VirtualHIDDevice v6.2.0** and
      `forceActivate`d the system extension; user approved it in
      System Settings → Login Items & Extensions → Driver Extensions.
- [x] **Discovered keyboard device names** via `sudo kanata --list`:
      - `Dactyl_76_L` (hash `0xE3D84B0E1C969CAB`, vendor 39572, product 2)
      - `Dactyl_76_R` (hash `0xE3D8590E1C96B475`, vendor 39572, product 2)
- [x] Filled `macos-dev-names-include ("Dactyl_76_L" "Dactyl_76_R")` in
      `kanata/mac.kbd`. The MacBook's built-in keyboard is left
      completely untouched.
- [x] Installed the Karabiner VHID user-space daemon as a LaunchDaemon at
      `/Library/LaunchDaemons/org.pqrs.Karabiner-VirtualHIDDevice-Daemon.plist`
      and bootstrapped it; daemon socket is at
      `/Library/Application Support/org.pqrs/tmp/rootonly/vhidd_*/*.sock`.
- [x] **Granted Input Monitoring + Accessibility** for
      `/opt/homebrew/Cellar/kanata/1.11.0/bin/kanata` (the resolved binary
      path, not the symlink).
- [x] Created **passwordless sudo** entry at `/etc/sudoers.d/kanata`:
      ```
      stavanpatel ALL=(ALL) NOPASSWD: /opt/homebrew/bin/kanata, /opt/homebrew/Cellar/kanata/*/bin/kanata
      ```
- [x] Created **wrapper script** `/usr/local/bin/kanata-sudo`:
      ```
      #!/bin/bash
      exec /usr/bin/sudo -n /opt/homebrew/bin/kanata "$@"
      ```
- [x] Copied configs to **`/etc/kanata/`** (system-wide canonical path):
      - `/etc/kanata/mac.kbd`
      - `/etc/kanata/common-layers.kbd`
      - `/etc/kanata/off.kbd` (stub for "disable" preset)
- [x] **Installed `kanata-tray` v0.8.0** via Homebrew.
- [x] Wrote tray config at
      `~/Library/Application Support/kanata-tray/kanata-tray.toml` with
      two presets: `Dactyl 76 — Split + Layers` (autorun) and
      `OFF (passthrough only)`.
- [x] Created **LaunchAgent** at
      `~/Library/LaunchAgents/com.github.kanata-tray.plist` so the tray
      auto-starts at login as the user. Tray icon visible in menu bar.
- [x] Verified end-to-end: tray launches kanata via wrapper, kanata
      grabs both Dactyl halves, no IOHIDDeviceOpen errors, TCP control
      socket connected.

## 🔧 Config gotchas already fixed (don't reintroduce)

- Media keys in kanata are `prev`, `pp`, `next` (NOT `mprv`, `mply`,
  `mnxt`). Volume: `vold`, `volu`, `mute`. Brightness: `brdn`, `brup`.
- Backslash key is `bksl` (NOT `bsls`). Pipe `|` is `S-bksl`.
- **kanata as LaunchDaemon was abandoned** in favor of the tray
  (LaunchAgent + passwordless sudo) — see DECISIONS.md D16.
- Running kanata as a child of the Copilot CLI (or any other terminal
  not granted Input Monitoring) causes macOS TCC to attribute the
  request to the parent, not kanata, → "IOHIDDeviceOpen not permitted".
  Solution: launch via `launchd` (LaunchAgent), so the parent is
  `launchd` and the responsible binary is kanata itself.

## ⏳ Blocked on user action

- [ ] **Vial setup** — set inner-LEFT thumb to **F14**, inner-RIGHT thumb
      to **F13** (see `vial/SETUP.md`). Until this is done, layers don't
      activate even though kanata is grabbing the halves.
- [ ] Optional smoke test once Vial is done: hold the right inner thumb
      and tap `j` — should produce `↓`. Hold the left inner thumb and
      tap `q` — should produce `!`.

## 📋 Pending

### macOS — nice-to-have
- [ ] **IDE bindings**: bind Hyper+B/R/T/D/F/G/U in JetBrains and VS Code
      to Build / Run / Test / Debug / Format / Goto / Find Usages
      (see `ide/BINDINGS.md`).
- [ ] **Verify media keys** work after Vial setup (FN layer arrows on
      right hand). Media on macOS via DriverKit is "best-effort".
- [ ] **Rotate user's macOS password** (was typed in chat earlier).

### Windows (separate machine)
- [ ] Follow `install/windows.md` end-to-end (mirrors the tray-based macOS
      flow: Wintercept build + Interception driver + kanata-tray launched
      elevated via Task Scheduler at logon).
- [ ] When discovering HWIDs, edit `kanata/windows.kbd` to put the real
      VID/PID prefix into `windows-only-windows-interception-keyboard-hwids`.
- [ ] Vial step is identical: F14 on inner-left thumb, F13 on inner-right.

### IDE bindings
- [ ] JetBrains (macOS): Settings → Keymap → bind the 7 Hyper chords.
- [ ] VS Code / Cursor (macOS): paste the JSON snippet from `ide/BINDINGS.md`.
- [ ] Visual Studio 2026 (Windows): Tools → Options → Environment → Keyboard.

## 🛠 Useful runtime commands

```bash
# Menu bar app status
launchctl print gui/$(id -u)/com.github.kanata-tray | grep state
pgrep -lf "kanata-tray|kanata --cfg"

# Tray log
tail -f ~/Library/Logs/kanata-tray.log

# Manual stop/start of the tray
launchctl bootout  gui/$(id -u)/com.github.kanata-tray
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.github.kanata-tray.plist

# Run kanata directly (debugging only)
/usr/local/bin/kanata-sudo -c /etc/kanata/mac.kbd

# Discover device names again (after firmware changes)
sudo kanata --list

# Validate config after editing
kanata --check -c /etc/kanata/mac.kbd
```

## 🐛 Known issues / open questions

- The exact physical matrix of the Dactyl 76 was inferred (6x6 + 6-key
  thumb cluster, 3 outer thumbs used). `defsrc` only covers the 5x4
  central block per hand + 2 thumb triggers — everything else stays
  passthrough.
- Media keys (`prev`/`pp`/`next`, `vold`/`volu`/`mute`, `brdn`/`brup`)
  on macOS via DriverKit are best-effort. Test after Vial setup.
- The `windows.kbd` config assumes Wintercept build for per-device
  filtering. If using LLHOOK, kanata will affect the built-in laptop
  keyboard too.

## 🔁 If you're an AI continuing this work

1. Read `.llm/CONTEXT.md` and `.llm/DECISIONS.md` first.
2. `ls /Users/stavanpatel/src/split-keyboard-setup/` to confirm files.
3. Check the menu bar — kanata-tray icon should be visible.
4. Check `~/Library/Logs/kanata-tray.log` and `pgrep -lf kanata` for state.
5. Help with whichever Pending item the user wants next.
6. Update this STATE.md when you finish anything.
