# KEYMAP

> These diagrams show the 5 kanata-managed columns per hand plus the two inner
> thumb keys (`F14` left, `F13` right). Everything outside this `defsrc` remains
> outside the keymap.
>
> `─` = passthrough. A visible `_` on SYM is the underscore key. The grids show
> the characters/actions produced; Kanata may emit shifted tokens internally.
> Pinky guardrail: positions `1` and `0` stay passthrough on every non-BASE
> layer.

Physical positions, left hand then right hand:

```text
1    2    3    4    5            6    7    8    9    0
q    w    e    r    t            y    u    i    o    p
a    s    d    f    g            h    j    k    l    ;
z    x    c    v    b            n    m    ,    .    /
                          f14    f13
```

## 🔹 BASE — normal typing

BASE is intentionally transparent: normal QWERTY/number-row typing, with the
inner thumbs acting as layer holds.

```text
┌────────┬────────┬────────┬────────┬────────┐                ┌────────┬────────┬────────┬────────┬────────┐
│   1    │   2    │   3    │   4    │   5    │                │   6    │   7    │   8    │   9    │   0    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   Q    │   W    │   E    │   R    │   T    │                │   Y    │   U    │   I    │   O    │   P    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   A    │   S    │   D    │   F    │   G    │                │   H    │   J    │   K    │   L    │   ;    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   Z    │   X    │   C    │   V    │   B    │                │   N    │   M    │   ,    │   .    │   /    │
└────────┴────────┴────────┴────────┴────────┘                └────────┴────────┴────────┴────────┴────────┘
                         ┌────────┐        ┌────────┐
                         │  SYM*  │        │  NAV*  │
                         └────────┘        └────────┘
                              F14                F13
```

- Hold **left inner thumb (`F14`)** for **SYM**.
- Hold **right inner thumb (`F13`)** for **NAV**.
- Hold the opposite inner thumb while in SYM/NAV for **FN**.

## 🔹 NAV — movement, selection, clipboard

```text
┌────────┬────────┬────────┬────────┬────────┐                ┌────────┬────────┬────────┬────────┬────────┐
│   ─    │   ─    │   ─    │   ─    │   ─    │                │   ─    │   ─    │   ─    │   ─    │   ─    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   ─    │   ─    │   ─    │   ─    │   ─    │                │  DocB  │  Home  │  PgUp  │  End   │  DocE  │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│  Cmd   │  Ctrl  │  Alt   │ Shift  │   ─    │                │   ←    │   ↓    │   ↑    │   →    │   ─    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│  Undo  │  Cut   │  Copy  │ Paste  │  Redo  │                │  Wrd←  │   ─    │  PgDn  │  Wrd→  │   ─    │
└────────┴────────┴────────┴────────┴────────┘                └────────┴────────┴────────┴────────┴────────┘
                         ┌────────┐        ┌────────┐
                         │  FN→   │        │  HELD  │
                         └────────┘        └────────┘
                              F14                F13
```

- Left hand gives one-shot modifiers on the home row: **Cmd / Ctrl / Alt / Shift** on `a s d f`.
  - On Windows, the Cmd position sends Ctrl.
  - Compose them with right-hand arrows for selection, line jumps, and word/document movement.
- Right hand keeps motion clustered: arrows on `h j k l`, Home/PgUp/End on `u i o`, document begin/end on `y p`, word jumps on `n .`, and PgDn on `,`.
- Clipboard is left-bottom-row: **Undo / Cut / Copy / Paste / Redo** on `z x c v b`.

## 🔹 SYM — programming symbols

```text
┌────────┬────────┬────────┬────────┬────────┐                ┌────────┬────────┬────────┬────────┬────────┐
│   ─    │   @    │   #    │   $    │   %    │                │   `    │   ^    │   ~    │   :    │   ─    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   "    │   '    │   +    │   |    │   *    │                │   :    │   |    │   <    │   >    │   ?    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   !    │   [    │   {    │   (    │   =    │                │   =    │   )    │   }    │   ]    │   ;    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   \    │   /    │   ?    │   -    │   &    │                │   &    │   _    │   ,    │   .    │   /    │
└────────┴────────┴────────┴────────┴────────┘                └────────┴────────┴────────┴────────┴────────┘
                         ┌────────┐        ┌────────┐
                         │  HELD  │        │  FN→   │
                         └────────┘        └────────┘
                              F14                F13
```

- **No shift-chords:** the layer emits programming symbols directly (`@`, `{`, `?`, etc.).
- **Mirrored brackets:** `s/l = []`, `d/k = {}`, `f/j = ()`.
- **Cross-hand digraphs:** `g`+`h` = `==`, `b`+`n` = `&&`, `r`+`u` = `||`, `x`+`/` = `//`, `c`+`p` = `??`, `c`+`.` = `?.`.
- **Pinky never extends:** `1` and `0` remain passthrough here too.

## 🔹 FN — function keys + IDE Hyper chords

```text
┌────────┬────────┬────────┬────────┬────────┐                ┌────────┬────────┬────────┬────────┬────────┐
│   ─    │   F2   │   F3   │   F4   │   F5   │                │   F6   │   F7   │   F8   │   F9   │   ─    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   F1   │   ─    │ Recent │  Run   │  Test  │                │   ─    │ Usages │   ─    │  Open  │  F10   │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   ─    │  Side  │ Debug  │ Format │  Goto  │                │  Back  │   ─    │   ─    │  Fwd   │   ─    │
├────────┼────────┼────────┼────────┼────────┤                ├────────┼────────┼────────┼────────┼────────┤
│   ─    │   ─    │   ─    │   ─    │ Build  │                │ Rename │Terminal│   ─    │   ─    │   ─    │
└────────┴────────┴────────┴────────┴────────┘                └────────┴────────┴────────┴────────┴────────┘
                         ┌────────┐        ┌────────┐
                         │   ─    │        │   ─    │
                         └────────┘        └────────┘
                              F14                F13
```

The IDE actions send Hyper chords (`M-C-A-S-*`) so every editor can bind them
without colliding with normal shortcuts.

| Action | Position | Sends |
|---|---:|---|
| Build | `b` | `M-C-A-S-b` |
| Run | `r` | `M-C-A-S-r` |
| Test | `t` | `M-C-A-S-t` |
| Debug | `d` | `M-C-A-S-d` |
| Format | `f` | `M-C-A-S-f` |
| Goto | `g` | `M-C-A-S-g` |
| Usages / references | `u` | `M-C-A-S-u` |
| Rename | `n` | `M-C-A-S-n` |
| Back | `h` | `M-C-A-S-[` |
| Forward | `l` | `M-C-A-S-]` |
| Recent Files | `e` | `M-C-A-S-e` |
| Quick Open | `o` | `M-C-A-S-p` |
| Terminal | `m` | `` M-C-A-S-` `` |
| Sidebar | `s` | `M-C-A-S-s` |

Function keys live on the top rows: `q = F1`, `2-5 = F2-F5`, `6-9 = F6-F9`, and `p = F10`.
