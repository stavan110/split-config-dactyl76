# LLM Handoff Bundle

Paste the contents of `PROMPT.md` (or attach this whole folder) into any LLM
assistant — Claude, ChatGPT, Copilot, Cursor, Gemini, whatever — and they'll
have full context to continue work on this keyboard setup.

## Files

| File              | Purpose                                                    |
|-------------------|------------------------------------------------------------|
| `PROMPT.md`       | Copy-paste bootstrap prompt for a new AI session           |
| `CONTEXT.md`      | Complete project background: hardware, model, constraints |
| `DECISIONS.md`    | Every design choice made and why (so AIs don't re-litigate)|
| `STATE.md`        | What's done, what's pending, known issues                  |
| `USER_PROFILE.md` | Stavan's preferences, workflow, IDEs, OS mix              |

## Recommended usage

**Quick context handoff** (most common):
1. Open a new chat with any LLM
2. Paste `PROMPT.md`
3. Attach or paste `CONTEXT.md` + `STATE.md`
4. Ask your question

**Deep handoff** (when changing direction or asking for a redesign):
1. Same as above, plus attach `DECISIONS.md` and `USER_PROFILE.md`
2. The assistant will know not to suggest things you've already rejected

## Updating after each session

When you finish a working session with an AI, ask it to update `STATE.md`
with what changed. Keep `CONTEXT.md`, `DECISIONS.md`, `USER_PROFILE.md`
stable unless the underlying setup actually changes.
