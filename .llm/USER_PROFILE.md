# User Profile — Stavan

## Machines / OSes

- **Primary (this work)**: MacBook on macOS (arm64, Homebrew at
  `/opt/homebrew`). Uses JetBrains IDEs heavily here.
- **Secondary**: A Windows machine. Uses Visual Studio 2026 and VS Code
  there.
- Wants the **same keyboard behavior on both** — single config source of
  truth, OS-specific deltas isolated.

## Keyboard / hardware

- Daily driver: **Dactyl 76 (Cycle 76)** from Alibaba — column-staggered
  ortholinear split with concave thumb clusters.
- Two halves are **independent USB devices** with no inter-half cable.
- Of the 6 thumb keys per side, uses only the **3 outer** ones:
  - Left:  outer = `Cmd`, middle = `Backspace`, inner = `SYM trigger (F14)`
  - Right: outer = `Enter`, middle = `Space`, inner = `NAV trigger (F13)`
- Also uses the laptop's built-in keyboard when the split is unplugged;
  must remain unaffected by kanata.

## Coding workflow

- **Primary IDEs**: JetBrains family (IntelliJ / PyCharm / WebStorm / etc.)
  on Mac, Visual Studio 2026 on Windows, plus VS Code / Cursor.
- Wants quick access to: **Build, Run, Test, Debug, Format, Go to
  Definition, Find Usages**.
- Wants debug controls (Step Into / Step Over / Toggle Breakpoint) on
  reachable keys.
- Comfortable rebinding IDE shortcuts to match kanata output.

## Preferences / quirks

- **Minimal-magic philosophy**: "I just want kanata for layering, not
  remapping" — appreciates clean, transparent base layer.
- Likes **vim-style hjkl** for navigation.
- Wants **numbers + symbols close to home position** to avoid reaching.
- Tolerates initial learning curve in exchange for long-term ergonomics.
- Wants tooling that gives **one-click on/off** (tray app) without losing
  the config.
- Asked for everything to be **documented in a folder** so they can revisit
  and learn the keymap at their own pace.
- Asked for an **AI handoff bundle** (`.llm/`) so they can continue with
  any assistant on any platform without re-explaining context.

## Communication style preferences

- Likes concise, structured answers with concrete next actions.
- Prefers tabular comparisons and ASCII diagrams over prose.
- Will course-correct quickly if a suggestion misses the mark — read
  their feedback carefully and adjust on the next turn.
- Appreciates being asked clarifying questions instead of assuming.

## Security note

- During this session, the sudo password was typed directly in the chat
  to unblock driver installation. User was advised to rotate it
  afterward. Future sessions: if a password is needed, suggest the user
  type it in a terminal session they control, or use `sudo -A` /
  `ASKPASS` instead.
