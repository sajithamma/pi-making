# Vertical layout plan (vertical.html) — 9:16 mobile

## Goal
Clone of the π approximation demo (index.html) optimized for portrait mobile (9:16). Same math and animation; only layout and aspect change.

## UI order (top → bottom)
1. **Circle** — top center (diameter = 1, diameter label below it).
2. **Stack of lines** — horizontal segments stacked **downward** (n=3 at top, n=30 at bottom); each line still “unfolds” horizontally but scaled so 0→π fits in 1 world unit.
3. **Scale** — horizontal scale 0 to π just below the line stack.
4. **Art-info** — “n = …” and length at the bottom.
5. **Final message** (when done) — above the top of the line stack (in-scene label).

## Coordinate and layout choices
- **Circle**: Center at (0, 1.5). Radius 0.5. No horizontal offset (circleOffsetX = 0). Circle and polygon share center (0, 1.5).
- **Horizontal scale**: Compress so 0..π fits in **1 world unit**. So `refX0 = -0.5`, line end = `refX0 + P_n/π` (i.e. scale factor `1/π`). Scale line and ticks from `refX0` to `refX0 + 1`; labels 0, 1, 2, 3, 3.14 at `refX0 + v/π` for v = 0,1,2,3,π.
- **Lines**: Same horizontal unfolding; `perimeterToLinePoints` uses a scale factor so x grows by `len/π` per segment. First line at `refY = 0.98`, then `LINE_GAP = -0.036` (step down). `lineYForN(n) = refY + (n-3)*LINE_GAP`.
- **Scale** y: `scaleY = lineYForN(maxN) - 0.22`.
- **Art-info** y: `scaleY - 0.12`.
- **Camera**: Orthographic. For 9:16 we want visible width/height = 9/16. Use e.g. height ≈ 2.6, width = 2.6×9/16 ≈ 1.46 so `left = -0.73`, `right = 0.73`, `top = 2.0`, `bottom = -0.55`. Content fits in that rectangle.

## What stays the same
- Logic: circle → polygon → unroll → show length → next n or done.
- Line rendering (Line2, same colors: circle orange, past lines dull white, current green).
- Labels: diameter = 1, scale labels, art-info “n = …”, final message with Pₙ and π.
- No new buttons; same auto-play and phase flow.

## File
- **vertical.html**: Single file; copy of index.html with viewport/CSS for mobile, camera bounds and aspect for 9:16, and the coordinate/layout changes above (circle center, refX0, refY, LINE_GAP, scale factor in perimeterToLinePoints, scale/art-info/final message positions, diameter label position, pi dashed line extent).
