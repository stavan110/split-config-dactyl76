# Windows install — Dactyl 76 with Vial (+ PowerToys for shortcut parity)

The Vial keymap is the same on every OS, but the firmware emits
**Mac-native** keycodes (Cmd via `LGUI`, Opt via `LALT`). In v4 you also
have a dedicated Alt on a left thumb. On Windows you install one extra app —
PowerToys — so Cmd-style shortcuts become Ctrl-style shortcuts, and doc
jumps match macOS behavior.

Word-jump is now natively typeable on Windows: after the PowerToys remap,
the keyboard's Cmd thumb becomes Ctrl, so hold left-outer + tap left/right
arrow (`h` / `l` on NAV) = Ctrl+arrow = word-jump.

> Tested on Windows 10 22H2 and Windows 11 24H2 (x64).

## 1. Install Vial

Download the latest installer from
[https://get.vial.today](https://get.vial.today) and run it.

On first launch:

1. Plug in **one half** of the Dactyl. Windows will discover it as a
   generic HID keyboard.
2. Vial should detect it within a few seconds. If not, unplug + replug.

## 2. Configure the keymap

Follow `../vial/SETUP.md` — it's the same procedure on every OS.

## 3. (Optional) Save a backup of the factory layout

Before changing anything:

- **File → Save Current Layout As…** → `dactyl_76_left_factory.vil`

Repeat for the right half.

## 4. Install PowerToys (for Cmd-style shortcut parity)

```powershell
winget install Microsoft.PowerToys
```

Or download the installer from
[github.com/microsoft/PowerToys/releases/latest](https://github.com/microsoft/PowerToys/releases/latest).

## 5. Configure PowerToys Keyboard Manager

Open **PowerToys → Keyboard Manager**.

Add these remaps **one at a time** (use the "+" to add each row). After all
are added, click **OK** to save.

| Original | New | Purpose |
|----------|-----|---------|
| `Win`   | `Ctrl` | Cmd-style shortcuts → Ctrl-* on Windows. Use the **Remap a key** tab for this single-key remap. |
| `Ctrl + Up` | `Ctrl + Home` | Doc top (after Win→Ctrl swap, the firmware's `LGUI(KC_UP)` arrives as Ctrl+Up). Use **Remap a shortcut**. |
| `Ctrl + Down` | `Ctrl + End` | Doc bottom. Use **Remap a shortcut**. |

That's it for v4. Do **not** remap `Alt+Left` / `Alt+Right`; the dedicated
Alt thumb should stay real Alt, and word-jump now comes from Ctrl+arrow.

### Trade-offs

After applying these remaps:

- ✅ Cmd-C/V/X/Z from the Dactyl now triggers Ctrl-C/V/X/Z on Windows.
- ✅ Word jumps are native: hold left outer (now Ctrl) + NAV left/right.
- ✅ Doc jumps (`y` / `p`) jump to start / end of document.
- ✅ Real Alt is still available from the left-middle thumb, so Alt+Left
  browser-back and Alt+letter shortcuts still work.
- ❌ The **Windows key on the Dactyl** no longer triggers Win-key
  shortcuts (Win+E, Win+R, Win+L, Win+Tab). Use the Win key on your
  laptop keyboard for these instead.

If that trade-off is a dealbreaker, see the **Alternative: separate
Windows layout** section at the bottom.

## 6. Smoke test

In any text editor:

| Try                                              | Expect                          |
|--------------------------------------------------|---------------------------------|
| Hold left outer thumb + tap `c` (with text selected) | Text is copied              |
| Hold right inner + tap `j`                       | Cursor moves down               |
| Hold left outer + hold right inner + tap `h` / `l` | Cursor jumps one word left / right |
| Hold right inner + tap `y`                       | Cursor jumps to document top    |
| Hold right inner + tap `;`                       | Backspace deletes a character   |
| Hold right inner + tap `,`                       | F11 fires                       |
| Hold right outer + tap `a`                       | Capital `A`                     |
| Hold left middle + hold right inner + tap `h`    | Alt+Left fires (browser back in many browsers) |

---

## Alternative: separate Windows layout (no PowerToys)

If you'd rather not install PowerToys and don't mind maintaining two
Vial layouts, you can build a Windows-native layout. This is a power-user
option, not necessary for v4 if PowerToys is acceptable.

1. Open Vial on Windows.
2. **File → Load** your current Mac layout.
3. Replace the Cmd thumb with Ctrl:
   - LEFT-OUTER thumb (BASE): change `KC_LGUI` → `KC_LCTL`.
4. Replace the doc-jump wrappers:
   - RIGHT-NAV `y`: change `LGUI(KC_UP)` → `LCTL(KC_HOME)`.
   - RIGHT-NAV `p`: change `LGUI(KC_DOWN)` → `LCTL(KC_END)`.
5. Leave the dedicated Alt and Shift thumbs alone:
   - LEFT-MIDDLE thumb stays `KC_LALT`.
   - RIGHT-OUTER thumb stays `KC_LSFT`.
6. **File → Save Current Layout As…** → `dactyl_76_left_windows.vil`.

Now you have two `.vil` files; load the appropriate one whenever you
switch machines.

## Uninstall

```powershell
winget uninstall Microsoft.PowerToys
# Then uninstall Vial via Windows Settings → Apps.
```

The keyboard keeps its layout in EEPROM. To revert to factory, load the
backup `.vil` you saved in step 3.
