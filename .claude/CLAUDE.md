# Memory

Before saving anything to memory, prompt the user and get confirmation. Do not save without asking first. New project learnings should be added to this file, not to external memory files.

# ZMK DTS case-folding

ZMK's DTS compiler lowercases node names when generating C identifiers. `vim_O` and `vim_o` both become `vim_o` in the output, causing a redefinition error.

When defining behavior variants that differ only by case (e.g. vim `o` vs `O`, `s` vs `S`), use fully distinct all-lowercase names — e.g. `vim_above` instead of `vim_O`, `vim_to_end` instead of `vim_S`.

# Glove80 binding array structure

The Glove80 binding array must contain exactly 80 keys. Wrong counts cause "not enough keys in map" build errors.

Row structure:
- Row 1 (F-keys): 10 (5 left + 5 right)
- Row 2 (numbers): 12 (6 + 6)
- Row 3 (top alpha): 12 (6 + 6)
- Row 4 (homerow): 12 (6 + 6)
- Row 5 (bottom + thumbs): 18 (6 + 3 thumbs + 3 thumbs + 6)
- Row 6 (bottom extra + thumbs + nav): 16 (5 + 3 thumbs + 3 thumbs + 5)

Row 1 is the most commonly missed — it needs 10 keys, not 5.

# ZMK layer numbering and priority

ZMK checks layers from highest number to lowest. Base layers (QWERT, GALLIUM, MAC_QWERT) must have lower numbers than all overlay layers (SYM, NUM, NAV, FUNC, Magic, VIM, MAC_VIM), otherwise the base layer's explicit bindings shadow the overlay layer and make its keys unreachable.

Current order: QWERT=0, GALLIUM=1, MAC_QWERT=2, SYM=3, NUM=4, NAV=5, FUNC=6, Magic=7, VIM=8, MAC_VIM=9.

Any new base layer must be inserted before SYM (currently 3). Any new overlay layer can be appended at the end. The layer order in the keymap block must match the `#define` values.

# Keymap comment tables

The ASCII table comments above each layer's bindings are generated automatically. Never edit them manually.

# Mac vs non-Mac vim layer split

`Home`/`End` keycodes work correctly in VS Code, terminals, and most IDEs. In macOS native text views (Safari, Mail, Notes, TextEdit), they scroll the document instead of moving the cursor.

Two parallel vim layers exist:
- `VIM` (layer 7) — uses `Home`/`End`, triggered from QWERTY/Gallium via `lt VIM R` and `lt VIM U`
- `MAC_VIM` (layer 9) — uses `LG(LEFT)`/`LG(RIGHT)`, triggered from `MAC_QWERT` (layer 8)

`MAC_QWERT` is otherwise identical to QWERTY and is toggled from the Magic layer.

Any new vim macro that moves to line start/end needs a parallel mac variant using `LG(LEFT)`/`LG(RIGHT)`.
