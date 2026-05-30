# macOS install — Dactyl 76 split keyboard with kanata + kanata-tray

This is the **end-to-end** procedure that was verified on this Mac.
It results in a menu-bar icon that auto-starts at login, runs kanata
as root in the background, and only ever touches the two split halves
(your MacBook keyboard is completely untouched).

> Tested on: macOS 15 (arm64), kanata 1.11.0, Karabiner-DriverKit-VirtualHIDDevice 6.2.0, kanata-tray 0.8.0.

---

## 1. Install kanata + the Karabiner virtual HID driver

```bash
brew install kanata kanata-tray
```

Download Karabiner-DriverKit-VirtualHIDDevice **6.2.0** (must match
kanata's bundled IPC version):

```bash
curl -LO https://github.com/pqrs-org/Karabiner-DriverKit-VirtualHIDDevice/releases/download/v6.2.0/Karabiner-DriverKit-VirtualHIDDevice-6.2.0.pkg
sudo installer -pkg Karabiner-DriverKit-VirtualHIDDevice-6.2.0.pkg -target /
sudo /Applications/.Karabiner-VirtualHIDDevice-Manager.app/Contents/MacOS/Karabiner-VirtualHIDDevice-Manager forceActivate
```

A system dialog will pop up — open **System Settings → General → Login
Items & Extensions → Driver Extensions** and toggle
`Karabiner-VirtualHIDDevice-Manager` ON.

Verify:

```bash
sudo systemextensionsctl list | grep pqrs
# expect: [activated enabled]
```

## 2. Start the Karabiner VHID user-space daemon

```bash
sudo tee /Library/LaunchDaemons/org.pqrs.Karabiner-VirtualHIDDevice-Daemon.plist > /dev/null <<'PLIST'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0"><dict>
  <key>Label</key><string>org.pqrs.Karabiner-VirtualHIDDevice-Daemon</string>
  <key>ProgramArguments</key><array>
    <string>/Library/Application Support/org.pqrs/Karabiner-DriverKit-VirtualHIDDevice/Applications/Karabiner-VirtualHIDDevice-Daemon.app/Contents/MacOS/Karabiner-VirtualHIDDevice-Daemon</string>
  </array>
  <key>RunAtLoad</key><true/>
  <key>KeepAlive</key><true/>
</dict></plist>
PLIST
sudo chown root:wheel /Library/LaunchDaemons/org.pqrs.Karabiner-VirtualHIDDevice-Daemon.plist
sudo launchctl bootstrap system /Library/LaunchDaemons/org.pqrs.Karabiner-VirtualHIDDevice-Daemon.plist
```

Verify the daemon is up:

```bash
sudo launchctl print system/org.pqrs.Karabiner-VirtualHIDDevice-Daemon | grep state
# expect: state = running
pgrep -lf VirtualHIDDevice-Daemon
```

## 3. Discover your keyboard halves' device names

Plug **both halves** in via USB, then:

```bash
sudo kanata --list
```

You should see lines like:

```
0xE3D84B0E1C969CAB   39572   2   Dactyl_76_L
0xE3D8590E1C96B475   39572   2   Dactyl_76_R
```

(Names come from the keyboard firmware — yours may differ. Copy the
`product_key` strings.)

## 4. Install the configs system-wide

```bash
sudo mkdir -p /etc/kanata
sudo cp /path/to/split-keyboard-setup/kanata/common-layers.kbd  /etc/kanata/
sudo cp /path/to/split-keyboard-setup/kanata/mac.kbd            /etc/kanata/
```

Edit `/etc/kanata/mac.kbd` and make sure the `macos-dev-names-include`
block lists your two halves:

```lisp
(defcfg
  process-unmapped-keys no
  concurrent-tap-hold   yes
  macos-dev-names-include (
    "Dactyl_76_L"
    "Dactyl_76_R"
  )
)
```

Create a stub config for the "OFF" tray preset:

```bash
sudo tee /etc/kanata/off.kbd > /dev/null <<'OFF'
(defcfg
  process-unmapped-keys no
  macos-dev-names-include ()
)
(defsrc)
(deflayer base)
OFF
```

Validate:

```bash
kanata --check -c /etc/kanata/mac.kbd
kanata --check -c /etc/kanata/off.kbd
```

## 5. Passwordless sudo + wrapper for kanata

kanata needs root to grab HID devices on macOS. We give it
*scoped* passwordless sudo so the tray app can start it without
prompting.

```bash
echo "$(whoami) ALL=(ALL) NOPASSWD: /opt/homebrew/bin/kanata, /opt/homebrew/Cellar/kanata/*/bin/kanata" | sudo tee /etc/sudoers.d/kanata
sudo chmod 440 /etc/sudoers.d/kanata
sudo visudo -c -f /etc/sudoers.d/kanata
```

Then the wrapper:

```bash
sudo tee /usr/local/bin/kanata-sudo > /dev/null <<'WRAP'
#!/bin/bash
exec /usr/bin/sudo -n /opt/homebrew/bin/kanata "$@"
WRAP
sudo chmod 755 /usr/local/bin/kanata-sudo
/usr/local/bin/kanata-sudo --version   # smoke test
```

## 6. Grant Input Monitoring + Accessibility

System Settings → Privacy & Security:

- **Input Monitoring** → `+` → press `⌘⇧G` → paste
  `/opt/homebrew/Cellar/kanata/1.11.0/bin` (use your actual version
  directory) → select `kanata` → enable it.
- **Accessibility** → same procedure.

> ⚠ When you upgrade kanata via Homebrew the version dir changes
> (e.g. 1.11.0 → 1.12.0) and the TCC grant silently invalidates.
> You'll need to re-grant after `brew upgrade kanata`.

## 7. Configure kanata-tray

```bash
mkdir -p ~/Library/Application\ Support/kanata-tray
cat > ~/Library/Application\ Support/kanata-tray/kanata-tray.toml <<'TOML'
"$schema" = "https://raw.githubusercontent.com/rszyma/kanata-tray/main/doc/config_schema.json"

[general]
allow_concurrent_presets = false
control_server_enable    = false

[defaults]
kanata_executable    = "/usr/local/bin/kanata-sudo"
tcp_port             = 5829
autorestart_on_crash = true

[presets.'Dactyl 76 — Split + Layers']
kanata_config = "/etc/kanata/mac.kbd"
autorun       = true

[presets.'OFF (passthrough only)']
kanata_config = "/etc/kanata/off.kbd"
autorun       = false
TOML
```

## 8. Autostart the tray at login (LaunchAgent)

```bash
cat > ~/Library/LaunchAgents/com.github.kanata-tray.plist <<PLIST
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0"><dict>
  <key>Label</key><string>com.github.kanata-tray</string>
  <key>ProgramArguments</key><array>
    <string>/opt/homebrew/bin/kanata-tray</string>
  </array>
  <key>RunAtLoad</key><true/>
  <key>KeepAlive</key><true/>
  <key>StandardOutPath</key><string>$HOME/Library/Logs/kanata-tray.log</string>
  <key>StandardErrorPath</key><string>$HOME/Library/Logs/kanata-tray.log</string>
  <key>EnvironmentVariables</key><dict>
    <key>PATH</key><string>/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin</string>
  </dict>
</dict></plist>
PLIST

launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.github.kanata-tray.plist
```

You should immediately see the **kanata-tray icon in the macOS menu
bar** (top-right). Click it → you'll see the two presets. The
`Dactyl 76 — Split + Layers` preset autoruns.

## 9. Do the Vial step (see `../vial/SETUP.md`)

In Vial, set:
- inner-LEFT thumb → **F14**
- inner-RIGHT thumb → **F13**

Until you do this, layers won't activate — kanata is grabbing the
halves but there are no F13/F14 events to trigger NAV/SYM.

## 10. Smoke test

After Vial setup:

- Hold the **right inner thumb** and tap `j` → should produce `↓`.
- Hold the **left inner thumb** and tap `q` → should produce `!`.

Look at `~/Library/Logs/kanata-tray.log` if anything misbehaves.

---

## Disabling temporarily

- Click the menu-bar icon → switch preset to **OFF** (or just **Stop**).
- Or unplug both halves — kanata only ever touches those two devices.

## After unplug / replug → run `kanata-restart`

kanata 1.11.0 holds device handles for its lifetime. If the keyboard is
unplugged and replugged (or you boot without it and connect it later),
kanata's existing handles are stale: keys pass through with **no
layers** but the process still looks healthy.

Bounce the tray to re-grab:

```bash
kanata-restart       # symlinked from scripts/kanata-restart
# or manually:
launchctl bootout  "gui/$(id -u)/com.github.kanata-tray"
launchctl bootstrap "gui/$(id -u)" ~/Library/LaunchAgents/com.github.kanata-tray.plist
```

**Test it's actually working** without typing-and-guessing:

```bash
# Hold inner-right thumb while this runs — you should see LayerChange events.
echo '{"RequestCurrentLayerName":{}}' | nc 127.0.0.1 5829
```

If holding the thumb produces `{"LayerChange":{"new":"nav"}}`, the full
stack (Vial → kanata → layers) is working.

## Uninstall

```bash
launchctl bootout gui/$(id -u)/com.github.kanata-tray
sudo launchctl bootout system/org.pqrs.Karabiner-VirtualHIDDevice-Daemon
rm ~/Library/LaunchAgents/com.github.kanata-tray.plist
sudo rm /Library/LaunchDaemons/org.pqrs.Karabiner-VirtualHIDDevice-Daemon.plist
sudo rm /etc/sudoers.d/kanata /usr/local/bin/kanata-sudo
sudo rm -rf /etc/kanata
brew uninstall kanata kanata-tray
# To remove the Karabiner VHID driver:
sudo /Applications/.Karabiner-VirtualHIDDevice-Manager.app/Contents/MacOS/Karabiner-VirtualHIDDevice-Manager forceDeactivate
```
