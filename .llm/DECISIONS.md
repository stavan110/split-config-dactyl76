# Design Decisions

Record of every decision made so far, with rationale. Future AI sessions
should NOT re-open these unless the user explicitly asks.

## D1. Hardware: two independent USB halves, no bridge

**Decision**: Accept the hardware as-is. No soldering / wiring a GPIO bridge
between halves. No reflashing with QMK split firmware.

**Rationale**: User does not want hardware modification. Software-only
solution at the OS level (kanata) is sufficient.

## D2. Remapping layer: kanata, not Karabiner-Elements

**Decision**: kanata.

**Rationale**: Cross-platform (macOS + Windows). The user uses both. Text
configs are version-controllable. Karabiner-Elements is macOS-only and uses
JSON which is more verbose / less expressive.

## D3. Layer ownership: kanata, not Vial

**Decision**: All layers live in kanata. Vial only defines base QWERTY +
sets two thumb keys to F13/F14.

**Rationale**: Vial layers are per-half. There is no mechanism for layer
state to span both halves at the firmware level. Even if both halves had
identical Vial layers, holding a layer key on one would only affect that
half. Kanata is the only layer in the stack that sees keys from both
halves at once.

(User initially preferred to keep layers in Vial. After this analysis was
laid out, the constraint was accepted.)

## D4. Layer-trigger keycodes: F13 (right) and F14 (left)

**Decision**: Inner thumb keys are remapped in Vial to F13 (right) and F14
(left). Kanata grabs these and treats them as `(layer-while-held ...)`.

**Rationale**: F13/F14 are unused on both macOS and Windows by default —
no app accidentally consumes them. They are real HID keycodes (not
custom), so they always make it through to the OS. Easier than using
unusual scancodes.

## D5. Base layer: completely passthrough

**Decision**: The base layer in kanata uses `_` for every key — kanata
forwards each key unchanged. Only F13/F14 do anything kanata-specific.

**Rationale**: The user explicitly asked for "kanata only for making it
behave like a true split, and not for anything else". No home-row mods, no
tap-dance on base, no remapping of Caps to Esc, nothing. The keyboard
types exactly as it does today when no layer is held.

## D6. Layer set: Base, NAV, SYM, FN (no separate NUM)

**Decision**: Numpad lives on the right half of the SYM layer (instead of
a dedicated NUM layer).

**Rationale**: Saves a layer-trigger slot. Symbols and numbers are
frequently used together (e.g., typing `var x = 42;`), so co-locating
them on the same hold makes sense. Right hand becomes numpad, left hand
becomes symbols.

## D7. NAV right-hand layout: vim hjkl with extensions

**Decision**:
- Home row right: `← ↓ ↑ →  PgDn` on `h j k l ;`
- Row above (QWERTY row right): `_ Home PgUp End _` on `y u i o p`
- Bottom row right: `_ ⌥← _ ⌥→ _` on `n m , . /` (word-jump on `m` / `.`)

**Rationale**: hjkl is muscle memory for vim users. Home / PgUp / End on
the row above keep your fingers on the same column as you go up/down a
row. Word-jump on `m`/`.` keeps Shift+arrow + selection workflow natural.

## D8. NAV left-hand: clipboard chords only

**Decision**: Bottom row left: `_ Undo Cut Copy Paste Redo` on
`z x c v b`. Rest of left hand stays passthrough.

**Rationale**: Most common edit operations within one thumb's hold. Keeps
the rest of the left hand free to type during nav (e.g., search-as-you-type
in editors). Shift / Cmd still work as base keys for selection / line jump.

## D9. SYM layout

**Decision**:
- Row 2 left: `! @ # $ %` on `q w e r t` (matches number row's shifted chars)
- Row 3 left: `` ` ~ { } | `` on `a s d f g`
- Row 4 left: `\ / ( ) &` on `z x c v b`
- Right hand: numpad as in D6

**Rationale**: `!@#$%` on the top of left letters mirrors the visual
position of `1234`-with-shift on the number row. Brackets/braces clustered
on home row of left. Parens on `c`/`v` are the most-typed brackets in
code, on strong fingers.

## D10. FN layer: F-keys + IDE actions + media

**Decision**: Hold both inner thumbs (F13 + F14, either order). On NAV
layer, F14 is mapped to `(layer-while-held fn)`. On SYM layer, F13 is
mapped to `(layer-while-held fn)`. This way pressing both → FN regardless
of order.

Layout:
- Number row: F1-F10
- QWERTY row left: Build / Run / Test / Debug / Format (Hyper chords)
- QWERTY row right: brightness on `u` / `i`
- Home row right: volume on `h` / `j` / `k`
- Bottom row left: F9 / F8 / F7 (BP / Step Over / Step Into) +
  Goto / Find Usages
- Bottom row right: prev / play / next track on `n` / `m` / `,`

## D11. IDE actions via Hyper chords

**Decision**: IDE actions send `Cmd+Ctrl+Opt+Shift+letter` (macOS) /
`Win+Ctrl+Alt+Shift+letter` (Windows). User binds these once per IDE.

**Rationale**: Zero conflicts with OS or app defaults. Same chord works on
both OSes. Stable across VS Code, JetBrains, Visual Studio, Neovim.

Rejected alternatives:
- ❌ Sending the IDE's *native* shortcuts (e.g., Cmd+B for Build in
  JetBrains) — too IDE-specific, would need different keys per IDE.
- ❌ Leader-key sequences (e.g., `<leader>b`) — only works in Vim/Emacs.

## D12. Cross-platform config structure: common + OS entry files

**Decision**:
```
kanata/
├── common-layers.kbd   # defsrc + deflayers; uses @-aliases
├── mac.kbd             # Cmd-based alias defs; (include "common-layers.kbd")
└── windows.kbd         # Ctrl-based alias defs; (include "common-layers.kbd")
```

**Rationale**: One source of truth for layer structure. Only OS-specific
differences (modifier choice for clipboard / word-jump, device-filter
syntax) live in the OS files.

## D13. Built-in laptop keyboard: device-name filter, not always-on

**Decision**: Use `macos-dev-names-include` (Mac) and
`windows-only-windows-interception-keyboard-hwids` (Windows) to restrict
kanata to *only* the two halves. The laptop keyboard types normally.

**Rationale**: User wanted the built-in keyboard untouched. Filtering by
device is cleaner than trying to toggle kanata on/off.

## D14. Tray app: kanata-tray (cross-platform)

**Decision**: Use rszyma's `kanata-tray` on both macOS and Windows for
start/stop/restart/config-switch from the menubar/system tray.

**Rationale**: Same UX on both OSes. Supports multiple preset configs
(useful for swapping in / out an "OFF" config when needed). Open source.

## D15. Launch at boot

**Decision**: macOS: LaunchDaemon. Windows: Task Scheduler trigger at
logon. Both run kanata as root/Administrator.

**Rationale**: Root required for input grabbing on both OSes. LaunchDaemon
+ scheduled task survive reboots.

---

## D16 — Run kanata via a user **LaunchAgent + `kanata-tray`**, not a system LaunchDaemon

**Status:** accepted, supersedes the earlier intent to install kanata as a
`/Library/LaunchDaemons/*.plist`.

**Decision.** kanata is launched by `kanata-tray`, which runs as the user
via `~/Library/LaunchAgents/com.github.kanata-tray.plist`. The tray
spawns kanata through a tiny wrapper (`/usr/local/bin/kanata-sudo`) that
calls `sudo -n /opt/homebrew/bin/kanata`. A sudoers drop-in
(`/etc/sudoers.d/kanata`) makes the sudo password-less for that exact
binary.

**Why:**
- The user wants a **menu-bar icon** they can click to pause / disable
  remapping when typing on the built-in laptop keyboard.
- A LaunchAgent autostarts at login and shows up in the GUI session
  (LaunchDaemons can't surface a tray icon).
- Passwordless sudo is scoped to **only the kanata binary**, not a
  blanket NOPASSWD, so the security impact is minimal.

**Side-effects:**
- `kanata-tray.toml` lives at
  `~/Library/Application Support/kanata-tray/kanata-tray.toml`.
- Configs are stored at `/etc/kanata/` (system-wide, root-owned, but
  readable by anyone).
- An `off.kbd` preset stub exists for an "OFF" tray option.

## D17 — Use **device-name include-list**, not a deny-list

**Status:** accepted.

`mac.kbd` uses `macos-dev-names-include ("Dactyl_76_L" "Dactyl_76_R")`.
This is allow-list semantics: kanata only ever touches the two halves.
Plugging in any other keyboard (USB or built-in) leaves it 100% untouched
with zero kanata involvement.

**Why not a `*-exclude` list?** Future USB keyboards would silently get
intercepted unless someone remembers to update the exclude list.

## D18 — Grant Input Monitoring / Accessibility to the **resolved binary**

**Status:** accepted, important gotcha.

The Homebrew kanata at `/opt/homebrew/bin/kanata` is a symlink to
`/opt/homebrew/Cellar/kanata/1.11.0/bin/kanata`. macOS TCC entries are
matched against the **resolved binary path**. When upgrading kanata
through Homebrew, the version directory changes, so the TCC grant
silently invalidates and the user has to re-grant for the new path.

**Mitigation:** doc this in `install/macos.md` upgrade notes (TODO).
