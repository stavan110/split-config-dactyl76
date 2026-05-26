# IDE Bindings

The FN/IDE layer sends **Hyper chords** (`Cmd+Ctrl+Opt+Shift` on macOS,
`Win+Ctrl+Alt+Shift` on Windows) plus standard debug F-keys (F7/F8/F9).

You bind each Hyper chord **once** in each IDE you use. Below are the exact
menu paths.

| Key on FN layer | Sends     | Action                  |
|-----------------|-----------|-------------------------|
| `Q`             | `Hyper+B` | Build / Compile         |
| `W`             | `Hyper+R` | Run                     |
| `E`             | `Hyper+T` | Run tests (current file)|
| `R`             | `Hyper+D` | Start debug             |
| `T`             | `Hyper+F` | Reformat / Format       |
| `V`             | `Hyper+G` | Go to definition        |
| `B`             | `Hyper+U` | Find usages / references|
| `Z`             | `F9`      | Toggle breakpoint *(default)* |
| `X`             | `F8`      | Step over *(default)*   |
| `C`             | `F7`      | Step into *(default)*   |

`Hyper` = `⌘⌃⌥⇧` on macOS, `Win+Ctrl+Alt+Shift` on Windows.

---

## VS Code / Cursor / Windsurf

Open `Keyboard Shortcuts` (⌘K ⌘S on Mac, Ctrl+K Ctrl+S on Windows). Click the
small file icon top-right to edit `keybindings.json` and paste:

```json
[
  { "key": "cmd+ctrl+alt+shift+b", "command": "workbench.action.tasks.build" },
  { "key": "cmd+ctrl+alt+shift+r", "command": "workbench.action.debug.run" },
  { "key": "cmd+ctrl+alt+shift+t", "command": "testing.runCurrentFile" },
  { "key": "cmd+ctrl+alt+shift+d", "command": "workbench.action.debug.start" },
  { "key": "cmd+ctrl+alt+shift+f", "command": "editor.action.formatDocument" },
  { "key": "cmd+ctrl+alt+shift+g", "command": "editor.action.revealDefinition" },
  { "key": "cmd+ctrl+alt+shift+u", "command": "editor.action.referenceSearch.trigger" }
]
```

On Windows replace `cmd` with `win` (or use Ctrl+Alt+Shift if Win-chords are
intercepted by the OS — see notes at bottom).

VS Code already maps F5 / F9 / F10 / F11 to Continue / Toggle BP / Step Over
/ Step Into — your FN-layer keys map to F7/F8/F9 which match Step Into /
Step Over / Toggle BP by default. ✅

---

## JetBrains (IntelliJ / PyCharm / WebStorm / GoLand / Rider / RustRover)

Open **Settings → Keymap**. Search for each action, right-click → **Add
Keyboard Shortcut**, then physically press the Hyper chord on your keyboard
while in the IDE.

| Action                  | Hyper chord to assign |
|-------------------------|-----------------------|
| Build Project           | `Hyper+B`             |
| Run                     | `Hyper+R`             |
| Run Tests (Context)     | `Hyper+T`             |
| Debug                   | `Hyper+D`             |
| Reformat Code           | `Hyper+F`             |
| Declaration or Usages   | `Hyper+G`             |
| Find Usages             | `Hyper+U`             |

JetBrains uses F8 = Step Over and F7 = Step Into by default — matches the FN
layer. F9 = Resume Program, *not* Toggle Breakpoint by default; if you want
F9 to toggle breakpoints, search **Toggle Line Breakpoint** in the keymap
and add F9 (you'll be asked to remove it from "Resume Program" — pick a
different shortcut for Resume, e.g. `Hyper+P`).

**Export tip**: JetBrains lets you export your keymap as XML. Save the result
to `~/.dotfiles` or this folder so you can sync it across machines.

---

## Visual Studio 2026 (Windows)

**Tools → Options → Environment → Keyboard**. In the "Show commands
containing" box, search for the command, then click in "Press shortcut keys"
and press the Hyper chord.

| Command                        | Chord         |
|--------------------------------|---------------|
| `Build.BuildSolution`          | `Hyper+B`     |
| `Debug.Start`                  | `Hyper+R`     |
| `TestExplorer.RunAllTestsInContext` | `Hyper+T`|
| `Debug.StartWithoutDebugging`  | `Hyper+R`     |
| `Edit.FormatDocument`          | `Hyper+F`     |
| `Edit.GoToDefinition`          | `Hyper+G`     |
| `Edit.FindAllReferences`       | `Hyper+U`     |

VS already uses F5 = Continue, F9 = Toggle BP, F10 = Step Over, F11 = Step
Into. Your FN layer's F7/F8/F9 → Step Into / Step Over / Toggle BP. ✅

---

## Neovim (optional)

If you also use Neovim, the Hyper chords just arrive as raw keycodes —
typically as `<C-S-A-D-b>` (terminals strip Cmd). Easier to wire by setting
your FN layer's IDE positions to `<leader>b`, `<leader>r`, etc. — let me
know and I'll add a Neovim-mode variant of the FN layer to the config.

---

## Why use Hyper chords?

- **Zero conflicts** with macOS, Windows, or any application — nothing else
  uses `⌘⌃⌥⇧+letter`.
- **Cross-IDE consistency** — once you train muscle memory, the same finger
  position triggers the same action everywhere.
- **Cross-OS consistency** — `M-C-A-S` resolves to Cmd-everything on Mac and
  Win-everything on Windows, so the same kanata config works on both.
