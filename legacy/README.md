# Legacy: kanata + Karabiner setup

This directory is kept as historical reference for the previous design.
It is **no longer recommended** for new installs.

## Why this is here

The original design (commits up to `572c3bd`) routed both Dactyl 76 halves
through [kanata](https://github.com/jtroo/kanata) on the host so they could
share global layers (NAV, SYM, FN). On macOS, kanata requires the
[Karabiner DriverKit Virtual HID Device](https://github.com/pqrs-org/Karabiner-DriverKit-VirtualHIDDevice)
to inject key events.

That virtual keyboard caused a known interop bug with macOS's per-keyboard
`fn` handling:

> kanata issue [#975](https://github.com/jtroo/kanata/issues/975) and
> Karabiner-Elements [#3000](https://github.com/pqrs-org/Karabiner-Elements/issues/3000):
> while the Karabiner virtual keyboard is loaded, the built-in MacBook
> keyboard's `fn` key stops triggering F1/F11/arrow shortcuts system-wide.

There is no software workaround. To fix it, the entire design was moved
into Vial firmware. See the top-level `README.md` for the current
Vial-native setup.

## What's in here

| Path | Was |
|------|-----|
| `kanata/common-layers.kbd` | Cross-platform layer definitions (4 layers: BASE/NAV/SYM/FN) |
| `kanata/mac.kbd`           | macOS entry config (Cmd-based shortcuts) |
| `kanata/windows.kbd`       | Windows entry config (Ctrl-based shortcuts) |
| `vial-kanata-setup.md`     | The old "set F13/F14 on inner thumbs" Vial step |
| `ide/BINDINGS.md`          | The Hyper-chord IDE bindings (only relevant with kanata) |
| `kanata-restart`           | Helper to bounce the kanata-tray LaunchAgent after replug |

## When you might want to revive this

- You don't use a MacBook (no built-in fn-key bug to worry about), and you
  want global cross-hand layer activation that pure Vial cannot provide.
- You're on Linux (kanata works without the Karabiner driver).
- You're willing to live with the macOS fn-key reset dance.

If that's you, the configs here still work as-is on Linux/Windows. On
macOS the layers will work but the built-in fn key will be broken while
the Karabiner driver is loaded.
