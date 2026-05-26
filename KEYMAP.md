# KEYMAP

> All diagrams show the 5 main rows + 6 active columns + the **3 outer thumb
> keys** per side that you actually use. Any extra row 0 / row 5 / inner-thumb
> keys you don't use stay completely passthrough.
>
> `──` = passthrough (the base-layer key still works while this layer is held)
> `HELD` = the thumb you're currently holding to activate this layer
> `FN→`  = pressing this with the other layer held jumps to the FN layer

---

## 🔹 BASE — default (kanata is invisible; you type normal QWERTY)

```
┌────┬────┬────┬────┬────┬────┐                ┌────┬────┬────┬────┬────┬────┐
│ `  │ 1  │ 2  │ 3  │ 4  │ 5  │                │ 6  │ 7  │ 8  │ 9  │ 0  │ -  │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│Tab │ Q  │ W  │ E  │ R  │ T  │                │ Y  │ U  │ I  │ O  │ P  │ \  │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│Esc │ A  │ S  │ D  │ F  │ G  │                │ H  │ J  │ K  │ L  │ ;  │ '  │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│Sft │ Z  │ X  │ C  │ V  │ B  │                │ N  │ M  │ ,  │ .  │ /  │Sft │
└────┴────┴────┴────┴────┴────┘                └────┴────┴────┴────┴────┴────┘
                   ┌────┬────┬────┐        ┌────┬────┬────┐
                   │ ⌘  │ ⌫  │SYM*│        │NAV*│ ␣  │ ⏎  │
                   └────┴────┴────┘        └────┴────┴────┘
                     outer ← inner            inner → outer
```

`SYM*` and `NAV*` are the inner thumb keys. In Vial they are set to **F14**
(left = SYM) and **F13** (right = NAV) — kanata grabs F13/F14 and turns them
into momentary layer triggers.

---

## 🔹 NAV — hold *right* inner thumb (F13)

```
┌────┬────┬────┬────┬────┬────┐                ┌────┬────┬────┬────┬────┬────┐
│ ── │ ── │ ── │ ── │ ── │ ── │                │ ── │ ── │ ── │ ── │ ── │ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ ── │ ── │ ── │ ── │ ── │                │ ── │Home│PgUp│End │ ── │ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ ── │ ── │ ── │ ── │ ── │                │ ←  │ ↓  │ ↑  │ →  │PgDn│ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │Undo│Cut │Copy│Pst │Redo│                │ ── │WrdL│ ── │WrdR│ ── │ ── │
└────┴────┴────┴────┴────┴────┘                └────┴────┴────┴────┴────┴────┘
                   ┌────┬────┬────┐        ┌────┬────┬────┐
                   │ ⌘  │ ⌫  │ FN→│        │HELD│ ␣  │ ⏎  │
                   └────┴────┴────┘        └────┴────┴────┘
```

- Hold base **Shift** + Arrow = select (Shift still works while NAV is held).
- Hold base **⌘** thumb + Arrow = line jump (⌘ thumb still works while NAV is held).
- `WrdL` / `WrdR` jump previous/next word (⌥+← / ⌥+→ on Mac, Ctrl+← / Ctrl+→ on Windows).
- Undo/Cut/Copy/Paste/Redo are Cmd-chords on Mac, Ctrl-chords on Windows.

---

## 🔹 SYM — hold *left* inner thumb (F14)

```
┌────┬────┬────┬────┬────┬────┐                ┌────┬────┬────┬────┬────┬────┐
│ ── │ ── │ ── │ ── │ ── │ ── │                │ ── │ ── │ ── │ ── │ ── │ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ !  │ @  │ #  │ $  │ %  │                │ ^  │ 7  │ 8  │ 9  │ *  │ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ `  │ ~  │ {  │ }  │ |  │                │ +  │ 4  │ 5  │ 6  │ -  │ =  │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ \  │ /  │ (  │ )  │ &  │                │ 0  │ 1  │ 2  │ 3  │ .  │ ── │
└────┴────┴────┴────┴────┴────┘                └────┴────┴────┴────┴────┴────┘
                   ┌────┬────┬────┐        ┌────┬────┬────┐
                   │ ⌘  │ ⌫  │HELD│        │ FN→│ ␣  │ ⏎  │
                   └────┴────┴────┘        └────┴────┴────┘
```

- Left hand: all the symbols you reach for in code, close to home position.
- Right hand: full numpad — `7 8 9`, `4 5 6`, `1 2 3`, `0` on the pinky.
- Operators on the outer right column: `*  -  .` for quick math.

---

## 🔹 FN / IDE — hold *both* inner thumbs (F13 + F14)

```
┌────┬────┬────┬────┬────┬────┐                ┌────┬────┬────┬────┬────┬────┐
│ ── │ F1 │ F2 │ F3 │ F4 │ F5 │                │ F6 │ F7 │ F8 │ F9 │F10 │F11 │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │ ── │ ── │ ── │ ── │ ── │                │Brt-│Brt+│ ── │ ── │ ── │F12 │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │Bld │Run │Tst │Dbg │Fmt │                │Vol-│Vol+│Mute│ ── │ ── │ ── │
├────┼────┼────┼────┼────┼────┤                ├────┼────┼────┼────┼────┼────┤
│ ── │Brk │StO │StI │Goto│Ref │                │Prev│Play│Next│ ── │ ── │ ── │
└────┴────┴────┴────┴────┴────┘                └────┴────┴────┴────┴────┴────┘
                   ┌────┬────┬────┐        ┌────┬────┬────┐
                   │ ⌘  │ ⌫  │HELD│        │HELD│ ␣  │ ⏎  │
                   └────┴────┴────┘        └────┴────┴────┘
```

**Top row F1–F11 + F12** is the standard F-row, useful in any app that uses
F-keys (browsers, debuggers, etc.).

### IDE actions (Hyper = ⌘⌃⌥⇧ on Mac, Win+Ctrl+Alt+Shift on Windows)

| Position (left hand) | Sends     | Bind in IDE to              |
|----------------------|-----------|-----------------------------|
| Row 3 col 1 (`Q`)    | Hyper+B   | Build / Compile             |
| Row 3 col 2 (`W`)    | Hyper+R   | Run                         |
| Row 3 col 3 (`E`)    | Hyper+T   | Run tests (file)            |
| Row 3 col 4 (`R`)    | Hyper+D   | Start debug                 |
| Row 3 col 5 (`T`)    | Hyper+F   | Format / Reformat code      |
| Row 4 col 1 (`Z`)    | F9        | Toggle breakpoint *(standard)* |
| Row 4 col 2 (`X`)    | F8        | Step over *(standard)*      |
| Row 4 col 3 (`C`)    | F7        | Step into *(standard)*      |
| Row 4 col 4 (`V`)    | Hyper+G   | Go to definition            |
| Row 4 col 5 (`B`)    | Hyper+U   | Find usages / references    |

See `../ide/BINDINGS.md` for exact menu paths per IDE.

---

## Layer-trigger cheatsheet

| Want | Hold |
|------|------|
| Arrows / nav / clipboard | Right inner thumb (F13) |
| Symbols / numpad         | Left inner thumb (F14)  |
| F-keys / IDE / media     | Both inner thumbs (F13 + F14, either order) |
| Just type                | Don't hold anything     |
