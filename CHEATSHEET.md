# Split Keyboard Cheatsheet

One-page reference for the current Kanata layers.

## Layer Access

- **BASE:** normal QWERTY/number-row typing.
- **SYM:** hold left inner thumb (`F14`).
- **NAV:** hold right inner thumb (`F13`).
- **FN:** hold the opposite inner thumb while in SYM/NAV.
- `─` = passthrough; visible `_` on SYM = underscore. Positions `1` and `0` stay passthrough on non-BASE layers.

## NAV Layer

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
```

- One-shot mods: `a s d f` = Cmd/Ctrl/Alt/Shift, then use right-hand arrows.
- Clipboard: `z x c v b` = Undo/Cut/Copy/Paste/Redo.
- Movement: `h j k l` = ← ↓ ↑ →, `n .` = word left/right, `y p` = document begin/end.

## SYM Layer

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
```

- Brackets mirror across hands: `s/l []`, `d/k {}`, `f/j ()`.
- Programming symbols are direct — no shift-chord required.

> **Cross-hand digraphs:** `g`+`h` = `==`, `b`+`n` = `&&`, `r`+`u` = `||`, `x`+`/` = `//`, `c`+`p` = `??`, `c`+`.` = `?.`.

## FN / IDE Layer

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
```

| Position | Action | Hyper chord |
|---:|---|---|
| `b` | Build | `M-C-A-S-b` |
| `r` | Run | `M-C-A-S-r` |
| `t` | Test | `M-C-A-S-t` |
| `d` | Debug | `M-C-A-S-d` |
| `f` | Format | `M-C-A-S-f` |
| `g` | Goto | `M-C-A-S-g` |
| `u` | Usages | `M-C-A-S-u` |
| `n` | Rename | `M-C-A-S-n` |
| `h/l` | Back / Forward | `M-C-A-S-[` / `M-C-A-S-]` |
| `e/o` | Recent / Open | `M-C-A-S-e` / `M-C-A-S-p` |
| `m/s` | Terminal / Sidebar | `` M-C-A-S-` `` / `M-C-A-S-s` |

## Daily Patterns

- **Select while moving:** NAV `f` (Shift) + `h/j/k/l`.
- **Line/document jumps:** NAV `a`/`s` one-shot with arrows, or use `y/p` for document begin/end.
- **Word hops:** NAV `d` (Alt) + arrows, or direct `n/.`.
- **Fast coding pairs:** SYM mirrored brackets plus cross-hand digraphs keep both hands alternating.
