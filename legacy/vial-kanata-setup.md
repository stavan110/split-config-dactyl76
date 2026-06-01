# Vial Setup (one-time, ~5 minutes)

Kanata grabs **F13** and **F14** as layer triggers. You need to assign those
to the two inner thumb keys in Vial on each half. **Everything else stays as
your existing QWERTY** — kanata does not touch the rest.

## Steps

1. Open [vial.rocks](https://get.vial.rocks/) (web app) or the Vial desktop app.
2. **Plug in the LEFT half only.** Vial will detect it.
3. Click the **inner-most thumb key** (the one closest to the center of the
   keyboard, the hardest to reach with the thumb when at rest).
4. In the on-screen keyboard, pick **F14** (under "Function" / "Quantum"
   section). Save.
5. Verify the other thumb keys still send what you expect:
   - **Outer thumb (left)** = `Left GUI` (Cmd on Mac, Win on Windows)
   - **Middle thumb (left)** = `Backspace`
   - **Inner thumb (left)** = `F14`  ← the one you just changed
6. Unplug the left half. **Plug in the RIGHT half.**
7. Click the **inner-most thumb key** on the right.
8. Pick **F13**. Save.
9. Verify the other thumb keys on the right:
   - **Outer thumb (right)** = `Enter`
   - **Middle thumb (right)** = `Space`
   - **Inner thumb (right)** = `F13`  ← the one you just changed

## Why F13 / F14?

They are real USB HID keycodes that no normal app uses on macOS or Windows.
That means kanata can grab them cleanly and the OS never tries to do anything
with them.

## Sanity check

Open any text app and tap the **inner left thumb**. It should type nothing
visible (F14 is unbound by default). Same for the inner right thumb (F13).
If they *do* type a character, you set the wrong key in Vial — go back and
fix it.

## Optional: clean up any existing Vial layers

Since kanata now owns the layers, you can delete any layer-toggle (`MO()` /
`TG()` / `LT()`) bindings you previously set in Vial. Set every Vial layer
beyond Layer 0 to passthrough (`KC_TRNS`). This avoids confusion when
debugging.
