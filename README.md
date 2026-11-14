# Lesson 04 — Fruit Sort: You Are the Machine

Unsupervised-learning mini-game. The student acts as the clustering model: pick K and one feature, then sort 12 emoji fruits into named group boxes on a wooden table. No timer, no score — sort until it looks right, then check.

File: `lesson-04-fruit-sort.html` — single self-contained page (inline `<style>` + `<script>`, no dependencies). Open it directly in a browser.

## How to play

1. Pick **K** — how many groups (2, 3, or 4).
2. Pick **one feature** — shape, color, or taste. Sort by that only.
3. Grab fruits from the **🧺 basket** (top-right button; opens a slide-in drawer).
4. Arrange them into the named **group boxes** on the table.
5. Press **Finish ✓** to check. Fruits left in the basket don't count.

Two ways to move a fruit:

- **Drag** it from the basket (or another box) and drop it on a group box — or back onto the 🧺 button to return it to the basket.
- **Click** a fruit to select it, then click a destination box (highlighted with an accent ring). `Esc` deselects.

## Rules the checker enforces

- Every group box must contain at least one fruit.
- At least one fruit must be on the table (all-in-basket is rejected).
- Placement is compared against the answer key for the current feature × K. A wrong fruit yields a hint naming which group it belongs in.

`Reset` returns all fruits to the basket and re-rolls group colors.

## Features × K answer key

| Feature | K=2 | K=3 | K=4 |
|---|---|---|---|
| Shape | Round / Not round | Round and big / Round and small / Not round | Round and big / Round and small / Long / Pear-berry |
| Color | Warm / Cool | Reddish / Yellow-orange / Green-purple | Red / Orange / Yellow / Green-purple |
| Taste | Sweet / Sour or mild | Sweet / Sweet-sour / Sour or mild | Very sweet / Crisp-juicy sweet / Tangy / Sour or mild |

Edit the `ANSWER` object in the `<script>` to change group names or membership. Each entry is `{ name, members[] }` keyed by `ANSWER[feature][K]`.

## Customization knobs

- **Fruit size:** `.chip .em { font-size: 44px }` — emoji scale with `font-size`. Trim `.chip` padding if large glyphs feel loose.
- **Group colors:** `GROUP_COLORS` array (6 saturated accents). Shuffled per round via `pickGroupColors(k)`, first K used. Each box sets `--gc`, consumed by the `h2` dot, box border, and dashed drop-zone border. Boxes are transparent so the table shows through; titles sit on white pills.
- **Scene height:** `#tableScene` is `flex: 1` inside a full-viewport column layout, so the table stretches to the screen bottom.
- **Fruits:** `FRUITS` array of `{ id, name, emoji }`. Chips render emoji only; `name` survives in `aria-label`s and checker messages.

## Implementation notes

- Pointer-drag uses a cloned `.chip.flying` ghost + `elementFromPoint` hit-testing against `[data-drop]` zones (group boxes, `#basketList`, `#basketBtn`).
- Click-to-move path is guarded by `lastDragEnd` (400 ms) so a drop doesn't immediately re-trigger as a click.
- Basket drawer toggles via `setDrawer()`; the 🧺 button doubles as a basket drop target and as "move selected fruit to basket" when a chip is selected.
- Responsive: below 640 px the grid collapses to one column and the drawer goes near-full-width.
