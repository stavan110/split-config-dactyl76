# Split Keyboard Cheatsheet

One-page printable reference for the Dactyl 76 Vial layout (v4).

## Thumb clusters

```text
LEFT  (outer → inner):  Cmd   | Alt   | SYM-hold
RIGHT (inner → outer):  NAV-hold | Spc | Sft
```

- **Alt is NEW in v4** on the left-middle thumb.
- RIGHT-OUTER is plain Shift in v4 — no tap-hold timing.
- Inner thumbs are local layer holds: left = symbols, right = navigation.

## Modifier chords

| What you want | Do this |
|---|---|
| Cmd + letter | Hold LEFT-OUTER + tap any letter |
| Alt + key | Hold LEFT-MIDDLE + tap any key |
| Shift + letter | Hold RIGHT-OUTER + tap letter |
| Cmd + Shift + letter | Hold LEFT-OUTER + RIGHT-OUTER + tap letter |
| Cmd + Opt + L (JetBrains reformat) | Hold LEFT-OUTER + LEFT-MIDDLE + tap `L` |
| Alt + Enter (JetBrains intent action) | Hold LEFT-MIDDLE + hold RIGHT-INNER + tap `N` |
| Shift + arrow (select) | Hold RIGHT-OUTER + hold RIGHT-INNER + tap `H/J/K/L` |
| Word jump (Mac) | Hold LEFT-MIDDLE + hold RIGHT-INNER + tap `H/L` |
| Word jump (Windows after PowerToys) | Hold LEFT-OUTER + hold RIGHT-INNER + tap `H/L` |
| Backspace | Hold RIGHT-INNER + tap `;` |
| Enter | Hold RIGHT-INNER + tap `N` (or your right pinky outer Enter, if firmware exposes one) |

> On Windows install PowerToys with the remaps in `install/windows.md`;
> after that, “Cmd” above = Ctrl, and Windows word-jump uses LEFT-OUTER +
> NAV arrow.

## LEFT-SYM layer

Hold **left inner thumb**.

```text
┌────┬────┬────┬────┬────┐
│ F1 │ F2 │ F3 │ F4 │ F5 │
├────┼────┼────┼────┼────┤
│ "  │ '  │ :  │ @  │ _  │
├────┼────┼────┼────┼────┤
│ !  │ [  │ {  │ (  │ =  │
├────┼────┼────┼────┼────┤
│ <  │ >  │ ]  │ )  │ }  │
└────┴────┴────┴────┴────┘
```

- Number row becomes F1–F5.
- Quotes / colon / at / underscore are on the top alpha row.
- Openers are on the home row; closers are below them.
- Angles are `Z/X`; equals is `G`.

## RIGHT-NAV layer

Hold **right inner thumb**.

```text
┌──────┬──────┬──────┬──────┬──────┐
│ F6   │ F7   │ F8   │ F9   │ F10  │
├──────┼──────┼──────┼──────┼──────┤
│ DocB │ Home │ PgUp │ End  │ DocE │
├──────┼──────┼──────┼──────┼──────┤
│ ←    │ ↓    │ ↑    │ →    │ Bspc │
├──────┼──────┼──────┼──────┼──────┤
│ Ent  │ Tab  │ F11  │ F12  │ Esc  │
└──────┴──────┴──────┴──────┴──────┘
```

- Arrows stay on `H/J/K/L`.
- Document begin/end are `Y/P`; Home/End/Page-Up are `U/O/I`.
- Backspace is `;`; Enter is `N`; Tab is `M`; Esc is `/`.
- F11/F12 are on `,`/`.` for debugger and IDE shortcuts.

## v3 → v4 migration

- Backspace moved from the left-middle thumb to RIGHT-NAV `;`.
- Enter moved from right-outer tap-hold to RIGHT-NAV `N`.
- Shift is no longer tap-hold; RIGHT-OUTER is plain Shift.
- Alt/Option is new on LEFT-MIDDLE.
- Word jumps use the dedicated Alt/Ctrl modifier plus RIGHT-NAV arrows.

## Smoke test

| Try | Expect |
|---|---|
| Hold left inner thumb + tap `Z` | `<` appears |
| Hold left middle thumb alone + tap an arrow in an app with Alt-arrow behavior | Alt behavior fires |
| Hold right inner thumb + tap `,` | F11 fires; test in a VS Code debug session |
| Hold right inner first, then hold left middle + tap `N` | Alt+Enter fires; test JetBrains intent action |

## Daily patterns

| Task | Chord |
|---|---|
| Copy paragraph | RIGHT-OUTER + RIGHT-INNER, select with `J/K/H/L`, then LEFT-OUTER + `C` |
| Jump to start of file | RIGHT-INNER + `Y` |
| Jump to end of file | RIGHT-INNER + `P` |
| Comment line | LEFT-OUTER + `/` |
| Type `{ }` for a block | LEFT-INNER + `D`, then LEFT-INNER + `B` |
| Backspace | RIGHT-INNER + `;` |
| Enter | RIGHT-INNER + `N` |
| JetBrains reformat | LEFT-OUTER + LEFT-MIDDLE + `L` |
| JetBrains intent action | LEFT-MIDDLE + RIGHT-INNER + `N` |

## Why the layers are split this way

- Left hand owns symbols because code punctuation clusters well there.
- Right hand owns navigation because `H/J/K/L` movement is already familiar.
- Modifiers stay as real modifiers, so Cmd/Ctrl/Alt/Shift shortcuts work in
  any app without a separate IDE-action layer.
