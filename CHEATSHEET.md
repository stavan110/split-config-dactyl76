# Split Keyboard Cheatsheet

One-page printable reference for the Dactyl 76 Vial layout.

## Thumb cluster

```
LEFT (outer→inner):    Cmd     Backspace    [hold = SYM]
RIGHT (inner→outer):   [hold = NAV]    Space    Enter/Shift
```

`Enter/Shift` = tap for Enter, hold for Shift.

## Modifier chords (BASE)

| Action                 | Combo                                                      |
|------------------------|------------------------------------------------------------|
| Cmd + letter           | hold LEFT-OUTER + tap letter (either hand)                 |
| Capital                | hold RIGHT-OUTER (Shift) + tap letter                      |
| Cmd+C / V / X / Z      | hold LEFT-OUTER + tap `c` / `v` / `x` / `z`                |
| Redo (Cmd+Shift+Z)     | hold LEFT-OUTER + hold RIGHT-OUTER + tap `z`               |
| Select arrow-by-arrow  | hold RIGHT-OUTER + hold RIGHT-INNER + tap `h`/`j`/`k`/`l`  |
| Select word-by-word    | hold RIGHT-OUTER + hold RIGHT-INNER + tap `n` / `.`        |

> On Windows install PowerToys with the remaps in `install/windows.md`;
> after that, "Cmd" above = Ctrl, and word-jumps still work.

## LEFT-SYM (hold left inner thumb)

```text
┌────────┬────────┬────────┬────────┬────────┐
│   F1   │   F2   │   F3   │   F4   │   F5   │
├────────┼────────┼────────┼────────┼────────┤
│   "    │   '    │   +    │   |    │   *    │
├────────┼────────┼────────┼────────┼────────┤
│   !    │   [    │   {    │   (    │   =    │
├────────┼────────┼────────┼────────┼────────┤
│   \    │   /    │   ?    │   -    │   &    │
└────────┴────────┴────────┴────────┴────────┘
```

- All symbols are direct (no Shift chord needed).
- Brackets mirror across hands: **`s/l = []`**, **`d/k = {}`**, **`f/j = ()`**.
- For closing brackets, use the right hand — but they're not on a layer,
  so just type them via outer pinky col or via the standard `]`/`}`/`)`
  keys on your existing Vial layout.

## RIGHT-NAV (hold right inner thumb)

```text
┌────────┬────────┬────────┬────────┬────────┐
│   F6   │   F7   │   F8   │   F9   │  F10   │
├────────┼────────┼────────┼────────┼────────┤
│  DocB  │  Home  │  PgUp  │  End   │  DocE  │
├────────┼────────┼────────┼────────┼────────┤
│   ←    │   ↓    │   ↑    │   →    │  Esc   │
├────────┼────────┼────────┼────────┼────────┤
│  Wrd←  │  Tab   │  PgDn  │  Wrd→  │   ─    │
└────────┴────────┴────────┴────────┴────────┘
```

- Arrows on `h j k l` (Vim-style).
- Page / line / doc navigation on the top alpha row.
- Word jumps on `n` (left) and `.` (right).
- Esc on `;` for modal editors. Tab on `m` for in-place indent.

## Daily patterns

| Task                         | Move                                              |
|------------------------------|---------------------------------------------------|
| Copy a paragraph             | Hold RIGHT-OUTER+RIGHT-INNER, tap `j` for each line, then LEFT-OUTER + `c` |
| Jump to start of file        | Hold RIGHT-INNER, tap `y` (DocB = Cmd+Up on Mac)  |
| Comment one line (IDE-dep.)  | LEFT-OUTER + `/`  (Cmd+/ in most editors)         |
| Type `{ }` for a block       | Hold left inner thumb, tap `d` then right pinky for `}` |
| Save                         | LEFT-OUTER + `s`                                  |
| Find                         | LEFT-OUTER + `f`                                  |
| Switch tabs                  | LEFT-OUTER + RIGHT-OUTER + Tab? (use IDE shortcut)|

> No layered IDE Hyper chords in this build — use your editor's native
> command palette (`Cmd+P` / `Cmd+Shift+P`) for actions that don't have a
> direct shortcut.
