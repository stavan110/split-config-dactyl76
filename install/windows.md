# Windows install — Dactyl 76 split keyboard with kanata + kanata-tray

This mirrors the macOS setup: a **menu-tray icon** that auto-starts at
logon, runs **kanata** in the background as Administrator, and only ever
touches the two Dactyl halves (the laptop's built-in keyboard stays
untouched).

> Tested patterns: kanata 1.11.0, kanata-tray 0.8.0, Interception driver 1.0.1.
> Windows 10 22H2 / Windows 11 24H2 (x64). Adjust paths if you're on arm64.

---

## 0. Why this stack on Windows

- **kanata Wintercept build** + **Interception driver** is the only way
  to filter by device on Windows so the laptop keyboard isn't affected.
  (The default LLHOOK build intercepts ALL keyboards.)
- **kanata-tray** runs as your user but is launched elevated by Task
  Scheduler, so it can spawn `kanata.exe` (which needs admin to use
  Interception).
- No driver-extension approval dance like macOS — the Interception
  driver is signed by Microsoft and installs cleanly with a reboot.

---

## 1. Install kanata, Interception driver, kanata-tray

Open **PowerShell as Administrator**.

```powershell
# Create install directory
New-Item -ItemType Directory -Force C:\Tools\kanata, C:\Tools\kanata-tray | Out-Null
New-Item -ItemType Directory -Force C:\ProgramData\kanata | Out-Null

# 1a) kanata — we want the Wintercept + GUI build (no console window)
$kz = "$env:TEMP\kanata.zip"
Invoke-WebRequest https://github.com/jtroo/kanata/releases/download/v1.11.0/windows-binaries-x64.zip -OutFile $kz
Expand-Archive -Force $kz -DestinationPath "$env:TEMP\kanata"
Copy-Item "$env:TEMP\kanata\kanata_windows_gui_wintercept_x64.exe" C:\Tools\kanata\kanata.exe
# Also copy the LLHOOK variant for diagnostics (you can run `kanata.exe --list` from CLI)
Copy-Item "$env:TEMP\kanata\kanata_windows_tty_wintercept_x64.exe" C:\Tools\kanata\kanata_tty.exe

# 1b) Interception driver — kernel-mode driver kanata needs for per-device filtering
$iz = "$env:TEMP\Interception.zip"
Invoke-WebRequest https://github.com/oblitum/Interception/releases/download/v1.0.1/Interception.zip -OutFile $iz
Expand-Archive -Force $iz -DestinationPath "$env:TEMP\Interception"
& "$env:TEMP\Interception\Interception\command line installer\install-interception.exe" /install
# REBOOT now (the driver only takes effect after a restart)

# 1c) kanata-tray
Invoke-WebRequest https://github.com/rszyma/kanata-tray/releases/download/v0.8.0/kanata-tray.exe -OutFile C:\Tools\kanata-tray\kanata-tray.exe
```

**Reboot now.** Interception only becomes active after a restart.

After reboot, verify Interception is loaded:

```powershell
# Run as Administrator
Get-CimInstance Win32_SystemDriver |
  Where-Object { $_.Name -like '*ception*' -or $_.Name -like 'keyboard' } |
  Select-Object Name, State, StartMode
# Look for: keyboard / Running / System
```

## 2. Copy the configs to a system-wide path

Copy the configs from this repo to a shared location everyone can read:

```powershell
# Adjust the source path to where you cloned/copied this folder
$src = "C:\path\to\split-keyboard-setup\kanata"
Copy-Item $src\common-layers.kbd C:\ProgramData\kanata\
Copy-Item $src\windows.kbd       C:\ProgramData\kanata\
```

Create an "OFF" stub for the tray's disable preset:

```powershell
@'
(defcfg
  process-unmapped-keys no
  windows-only-windows-interception-keyboard-hwids ()
)
(defsrc)
(deflayer base)
'@ | Set-Content -Encoding UTF8 C:\ProgramData\kanata\off.kbd
```

Validate both configs (from Admin PowerShell):

```powershell
C:\Tools\kanata\kanata_tty.exe --check -c C:\ProgramData\kanata\windows.kbd
C:\Tools\kanata\kanata_tty.exe --check -c C:\ProgramData\kanata\off.kbd
```

Both should print `config file is valid`.

## 3. Discover your keyboard halves' HWIDs

Plug **both halves in**, then in **Admin PowerShell**:

```powershell
C:\Tools\kanata\kanata_tty.exe --list
```

Look for two lines like:

```
HID\VID_9B14&PID_0002&MI_00&Col01\7&...   Dactyl_76_L
HID\VID_9B14&PID_0002&MI_00&Col01\7&...   Dactyl_76_R
```

You only need the **VID & PID portion** (`VID_9B14&PID_0002`). Substring
matching is used, so a short prefix is enough — and **it survives
USB port changes and reboots**.

## 4. Edit `windows.kbd` to lock kanata to the two halves

Open `C:\ProgramData\kanata\windows.kbd` in your editor and make the
`windows-only-windows-interception-keyboard-hwids` block list your
halves. Backslashes need to be escaped in the kanata string syntax:

```lisp
(defcfg
  process-unmapped-keys no
  concurrent-tap-hold   yes
  windows-only-windows-interception-keyboard-hwids (
    "HID\\VID_9B14&PID_0002"   ;; matches both halves of Dactyl_76 (same VID/PID)
  )
)
```

> If your halves have **different** VID/PIDs (some firmwares do), list
> them as two separate strings.

Re-validate:

```powershell
C:\Tools\kanata\kanata_tty.exe --check -c C:\ProgramData\kanata\windows.kbd
```

## 5. Configure kanata-tray

The tray reads `%APPDATA%\kanata-tray\kanata-tray.toml`. Create it:

```powershell
$cfgDir = "$env:APPDATA\kanata-tray"
New-Item -ItemType Directory -Force $cfgDir | Out-Null

@'
"$schema" = "https://raw.githubusercontent.com/rszyma/kanata-tray/main/doc/config_schema.json"

[general]
allow_concurrent_presets = false
control_server_enable    = false

[defaults]
kanata_executable    = 'C:\Tools\kanata\kanata.exe'
tcp_port             = 5829
autorestart_on_crash = true

[presets.'Dactyl 76 — Split + Layers']
kanata_config = 'C:\ProgramData\kanata\windows.kbd'
autorun       = true

[presets.'OFF (passthrough only)']
kanata_config = 'C:\ProgramData\kanata\off.kbd'
autorun       = false
'@ | Set-Content -Encoding UTF8 "$cfgDir\kanata-tray.toml"
```

## 6. Quick smoke test (manual elevation)

In **Admin PowerShell**:

```powershell
C:\Tools\kanata-tray\kanata-tray.exe
```

- A new **kanata-tray icon** should appear in the system tray
  (bottom-right). Click it → you'll see the two presets.
- `Dactyl 76 — Split + Layers` should autorun.
- **Skip to step 8 (Vial)** if layers don't react yet — until F13/F14
  are mapped on the inner thumbs, NAV/SYM can't activate.

Press Ctrl+C in the PowerShell window or close the tray icon to stop.

## 7. Autostart at logon (elevated) via Task Scheduler

This is the Windows analog of the macOS LaunchAgent. We need to run
`kanata-tray.exe` elevated at every logon, otherwise it can't spawn
kanata with admin rights.

In **Admin PowerShell**:

```powershell
$action  = New-ScheduledTaskAction -Execute 'C:\Tools\kanata-tray\kanata-tray.exe'
$trigger = New-ScheduledTaskTrigger -AtLogOn -User "$env:USERDOMAIN\$env:USERNAME"
$principal = New-ScheduledTaskPrincipal `
    -UserId "$env:USERDOMAIN\$env:USERNAME" `
    -LogonType Interactive `
    -RunLevel Highest
$settings = New-ScheduledTaskSettingsSet `
    -AllowStartIfOnBatteries `
    -DontStopIfGoingOnBatteries `
    -ExecutionTimeLimit ([TimeSpan]::Zero) `
    -RestartCount 3 -RestartInterval (New-TimeSpan -Minutes 1) `
    -StartWhenAvailable

Register-ScheduledTask -TaskName 'kanata-tray' `
    -Action $action -Trigger $trigger `
    -Principal $principal -Settings $settings `
    -Description 'kanata-tray autostart at logon (elevated)'

# Test it without rebooting:
Start-ScheduledTask -TaskName 'kanata-tray'
```

Verify with:

```powershell
Get-ScheduledTask kanata-tray | Get-ScheduledTaskInfo
Get-Process kanata-tray, kanata
```

> **UAC prompt at login?** No. Tasks created with `RunLevel Highest`
> and `LogonType Interactive` run elevated **without** showing a
> prompt, because Task Scheduler has the elevation token of the user.

## 8. Do the Vial step

Open Vial (Windows binary from https://get.vial.today) and on each half:

- **inner-LEFT thumb** → `F14`
- **inner-RIGHT thumb** → `F13`

Save to keyboard. See `../vial/SETUP.md` for screenshots/details.

This is the **only** firmware change needed; everything else lives in
kanata.

## 9. Final smoke test

After Vial:

- Hold the **right inner thumb** and tap `j` → should produce `↓`.
- Hold the **left inner thumb** and tap `q` → should produce `!`.
- Tap **both** inner thumbs together and try the **number row** → F1–F10.
- Press **Hyper+B** (Win+Ctrl+Alt+Shift+B) to test IDE binding once
  you've configured Visual Studio / VS Code keymaps
  (see `../ide/BINDINGS.md`).

If something misbehaves, view the kanata log via the tray icon (right-click
the tray → **Show kanata log**) or run the tty build from an Admin
PowerShell to see real-time output:

```powershell
C:\Tools\kanata\kanata_tty.exe -c C:\ProgramData\kanata\windows.kbd
```

---

## Disabling temporarily

- Click the **tray icon** → switch preset to **OFF** (or just hit Stop).
- Or unplug both halves — kanata is filtered to those HWIDs only.

## Uninstall

```powershell
# In Admin PowerShell
Stop-ScheduledTask -TaskName 'kanata-tray'
Unregister-ScheduledTask -TaskName 'kanata-tray' -Confirm:$false
Stop-Process -Name kanata-tray, kanata -Force -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force C:\Tools\kanata, C:\Tools\kanata-tray, C:\ProgramData\kanata
Remove-Item -Recurse -Force "$env:APPDATA\kanata-tray"

# Uninstall Interception driver
& "$env:TEMP\Interception\Interception\command line installer\install-interception.exe" /uninstall
# Reboot
```

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| `failed to grab keyboard` on startup | Wintercept build but not admin, or Interception not installed | Make sure Task Scheduler task is at "Highest" privileges; verify Interception is loaded (Step 1) |
| Laptop keyboard also gets remapped | HWID filter empty, OR you swapped to the LLHOOK build | Double-check `windows-only-windows-interception-keyboard-hwids` lists your halves' VID/PID |
| Tray icon never appears | Scheduled task not running, or it crashed | `Get-ScheduledTaskInfo kanata-tray` and check `LastTaskResult`; run `kanata-tray.exe` manually from Admin PowerShell to see error |
| Hyper+B opens File Explorer's address-bar focus or something | Win+B is taken by Windows for taskbar focus; remap that single chord in `windows.kbd` | Change `bld M-C-A-S-b` to a different letter (e.g. `M-C-A-S-y`) and re-bind in your IDE |
| Layers don't activate | Vial step wasn't done | Open Vial, set F13/F14 on inner thumbs, save to keyboard |
| Kanata can't see one of the halves in `--list` | Half not powered / cable bad | Plug-cycle, try a different USB port, confirm Windows sees it under Settings → Bluetooth & devices |

## Useful runtime commands

```powershell
# Status
Get-Process kanata-tray, kanata -ErrorAction SilentlyContinue
Get-ScheduledTask kanata-tray | Get-ScheduledTaskInfo

# Manual stop / start of the tray
Stop-Process  -Name kanata-tray, kanata -Force
Start-ScheduledTask -TaskName kanata-tray

# Re-discover HWIDs (after a firmware change)
C:\Tools\kanata\kanata_tty.exe --list

# Validate config after editing
C:\Tools\kanata\kanata_tty.exe --check -c C:\ProgramData\kanata\windows.kbd
```
