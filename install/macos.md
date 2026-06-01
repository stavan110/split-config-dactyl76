# macOS install — Dactyl 76 with Vial

The Vial design is **firmware-only** — there's no daemon, no menu-bar
app, no kernel extension. The "install" on macOS is just installing the
Vial configurator app so you can flash the keymap onto the keyboard's
EEPROM.

> Tested on macOS 15 (Sequoia) and macOS 26 (arm64).

## 1. Install Vial

```bash
brew install --cask vial
```

Or download the universal `.dmg` directly from
[https://get.vial.today](https://get.vial.today).

## 2. First launch

1. Open **Vial** from Applications. You may need to right-click → Open
   the first time to bypass Gatekeeper.
2. macOS will ask for **Input Monitoring** permission. Approve it
   (System Settings → Privacy & Security → Input Monitoring → Vial → on).
3. Plug in **one half** of the Dactyl. Vial should detect it and show
   the keyboard at the top of the window.

> If Vial doesn't see the keyboard, unplug + replug the half. Some Mac
> USB ports power-cycle slowly.

## 3. Configure the keymap

Follow `../vial/SETUP.md` — it walks through every key, click-by-click.

## 4. (Optional) Save a backup of the factory layout

Before changing anything, save the factory layout:

- **File → Save Current Layout As…** → `dactyl_76_left_factory.vil`

Repeat for the right half. Stash these somewhere syncable (Dropbox,
iCloud Drive, etc.).

---

## Why there's no daemon / extension

The earlier kanata-based design needed root privileges, a kernel
extension (Karabiner DriverKit), a system LaunchDaemon, a user
LaunchAgent for the tray app, and a passwordless sudo entry. **None of
that is needed with Vial** — the layout lives on the keyboard's MCU and
the OS just sees standard HID input.

This also means:

- **Your built-in MacBook `fn` key works normally** — no virtual
  keyboard is created, so the macOS keyboard-discovery quirk that
  affected the kanata design (issue [#975](https://github.com/jtroo/kanata/issues/975))
  doesn't apply.
- **No re-grant after `brew upgrade`** — Input Monitoring is granted to
  the Vial app's bundle ID, not a kanata version directory.
- **The keyboard works on any Mac** you plug it into, with or without
  Vial installed. Vial is only needed to *change* the layout.

## Uninstall

```bash
brew uninstall --cask vial
```

The keyboard keeps its layout (it's stored on the MCU). To revert to
factory, load the backup `.vil` file you saved in step 4, or use Vial's
factory-reset option.
