# Windows install — Dactyl 76 with Vial (+ PowerToys for shortcut parity)

The Vial keymap is the same on every OS, but the firmware emits
**Mac-native** keycodes (Cmd via `LGUI`, Opt via `LALT`). On Windows you
install one extra app — PowerToys — and remap a handful of shortcuts so
the layout behaves the way macOS users expect: Cmd-C copies, word jumps
work, etc.

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

Open **PowerToys → Keyboard Manager → Remap a shortcut**.

Add the following shortcut remaps **one at a time** (use the "+" to add
each row). After all are added, click **OK** to save.

| Original shortcut | New shortcut    | Purpose                          |
|-------------------|-----------------|----------------------------------|
| `Win`             | `Ctrl`          | Cmd-style shortcuts → Ctrl-* on Windows. Use the "Remap a key" tab for this single-key remap. |
| `Alt + Left`      | `Ctrl + Left`   | Word jump left (was browser back) |
| `Alt + Right`     | `Ctrl + Right`  | Word jump right (was browser forward) |
| `Ctrl + Up`       | `Ctrl + Home`   | Doc top (after Win→Ctrl swap, the firmware's LGUI+Up arrives as Ctrl+Up) |
| `Ctrl + Down`     | `Ctrl + End`    | Doc bottom                       |

> The `Win → Ctrl` row goes in the **Remap a key** tab (top of Keyboard
> Manager). The four `Alt+`/`Ctrl+` rows go in the **Remap a shortcut**
> tab.

### Trade-offs

After applying these remaps:

- ✅ Cmd-C/V/X/Z from the Dactyl now triggers Ctrl-C/V/X/Z on Windows.
- ✅ Word jumps on the NAV layer (`n` / `.`) work as expected.
- ✅ Doc jumps (`y` / `p`) jump to start / end of document.
- ❌ The **Windows key on the Dactyl** no longer triggers Win-key
  shortcuts (Win+E, Win+R, Win+L, Win+Tab). Use the Win key on your
  laptop keyboard for these instead.
- ❌ **Browser Back via Alt+Left** stops working. Use the browser's
  back button or `Backspace` (in some browsers).

If those trade-offs are dealbreakers, see the **Alternative: separate
Windows layout** section at the bottom.

## 6. Smoke test

In any text editor:

| Try                                              | Expect                          |
|--------------------------------------------------|---------------------------------|
| Hold left outer thumb + tap `c` (with text selected) | Text is copied              |
| Hold right inner + tap `j`                       | Cursor moves down               |
| Hold right inner + tap `n`                       | Cursor jumps one word left      |
| Hold right inner + tap `y`                       | Cursor jumps to document top    |
| Hold right outer + tap `a`                       | Capital `A`                     |
| Quick tap right outer                            | Newline                         |

---

## Alternative: separate Windows layout (no PowerToys)

If you'd rather not install PowerToys and don't mind maintaining two
Vial layouts, you can build a Windows-native layout:

1. Open Vial on Windows.
2. **File → Load** your current Mac layout.
3. Replace every `LGUI` with `LCTL`:
   - LEFT-OUTER thumb (BASE): change `KC_LGUI` → `KC_LCTL`.
   - RIGHT-NAV `y`: change `LGUI(KC_UP)` → `LCTL(KC_HOME)`.
   - RIGHT-NAV `p`: change `LGUI(KC_DOWN)` → `LCTL(KC_END)`.
4. Replace word-jump LALT with LCTL:
   - RIGHT-NAV `n`: change `LALT(KC_LEFT)` → `LCTL(KC_LEFT)`.
   - RIGHT-NAV `.`: change `LALT(KC_RIGHT)` → `LCTL(KC_RIGHT)`.
5. Right outer thumb: change `MT(MOD_LSFT, KC_ENT)` — no change needed,
   Shift is the same on both OSes.
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
