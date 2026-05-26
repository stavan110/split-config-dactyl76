# Bootstrap Prompt — Paste this into any new AI assistant session

---

I'm continuing work on my **split-keyboard setup** for a Dactyl 76 (Cycle 76)
ergonomic keyboard from Alibaba. The two halves are **independent USB
devices** with no inter-half communication (no TRRS, no bridge cable). I'm
using **kanata** to glue them into one logical split keyboard with shared
layers across both hands.

The full project lives at:

```
/Users/stavanpatel/src/split-keyboard-setup/
```

(Or copy the folder to wherever you're working — the relative paths inside
the kanata configs assume `common-layers.kbd` sits next to `mac.kbd` /
`windows.kbd`.)

**Before answering anything, please read these files** (in this order):

1. `.llm/CONTEXT.md` — project background, hardware, the kanata-only-for-layering model
2. `.llm/DECISIONS.md` — design choices already made (don't re-litigate)
3. `.llm/STATE.md` — what's done, what's pending, known issues
4. `.llm/USER_PROFILE.md` — my OSes, IDEs, ergonomic preferences
5. `KEYMAP.md` — the current layer design
6. `kanata/common-layers.kbd`, `kanata/mac.kbd`, `kanata/windows.kbd` — current config

Then answer my next question with that context loaded.

**Rules of engagement** (please follow):

- **Don't suggest moving layers into Vial** — we already analyzed that and it
  can't work because Vial layers are per-half. Kanata owns the layers,
  Vial owns the base QWERTY.
- **Don't suggest Karabiner-Elements as an alternative** — I want kanata
  specifically for cross-platform portability (macOS + Windows).
- **Don't suggest a new layout from scratch** — I've already approved the
  current design (Base / NAV / SYM / FN). Tweaks within those layers are
  welcome; full redesigns are not.
- **Keep changes surgical** — only modify what's needed for the current ask.
- **When editing kanata configs**, preserve the cross-platform split (common
  layers in `common-layers.kbd`, OS-specific aliases in `mac.kbd` /
  `windows.kbd`).
- **After making changes**, update `.llm/STATE.md` so future sessions see
  what's been done.

My next question is:

> _(write your question here)_
