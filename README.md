# π from Inscribed Polygons

A single-page **teaching visualization** that shows how the perimeter of regular polygons inscribed in a circle approaches **π** as the number of sides increases. Built with **Three.js** (orthographic view, no build step).

---

## What It Does

- **Circle** with diameter 1 (so circumference = π) is shown on the left.
- An **inscribed polygon** (triangle → square → pentagon → …) appears inside the circle.
- The **same perimeter** (one continuous line) **unfolds** segment by segment into a horizontal line above a scale.
- Each new polygon adds a **new row**: previous perimeters stay visible as stacked lines (bright blue), and the **current** perimeter is drawn in green.
- A **scale from 0 to 3.14** and a **dotted vertical line at 3.14** show how the line length grows toward π.
- The animation runs on its own (no on-screen controls) and loops: circle → polygon → unfold → show length → next polygon → … → “π reached ≈ 3.14” → repeat.

---

## Purpose & Intent

- **Teaching:** Give an intuitive, visual answer to “why does the perimeter of inscribed polygons tend to π?”
- **One idea:** The polygon is “split” into n sides; when that perimeter is “expanded” into a straight line, you see one length that grows with n.
- **Visual proof:** More sides → longer line → line approaches the 3.14 mark. The scale and the dotted line at 3.14 make that comparison explicit.
- **Minimal UI:** No controls, no error number, no big panels—just the math story in the picture.

---

## How It Works

### Math

- **Circle:** radius `r = 0.5` (diameter 1) ⇒ circumference = π.
- **Inscribed regular n-gon:** vertices on the circle; perimeter  
  **Pₙ = 2n·r·sin(π/n)**.  
  As n → ∞, Pₙ → π.
- **Unfolding:** The polygon’s perimeter is laid out as a horizontal segment from 0 to Pₙ on the scale, so “length = Pₙ” is visible.

### Phases (narrative)

1. **Circle** — Only the circle (diameter 1 → circumference π).
2. **Polygon** — n-gon inside the circle (“polygon split into n sides”).
3. **Unroll** — The same perimeter morphs from the closed polygon into the horizontal line, **one segment at a time** (no separate line; one geometry that unfolds).
4. **Show length** — Full horizontal line; Pₙ and phase message explain “more sides → longer line → approaches π.”
5. **Next n** — Current line is saved as a completed (blue) line on its own row; n increases and we go back to “polygon” for the next shape.
6. **Done** — When n reaches max, overlay: “π ≈ 3.14”; then the cycle restarts.

### Layout & scale

- **Circle** is placed so its **right edge** is at **0** on the scale (circle left of the lines, no overlap).
- **Lines** start at **0** on the scale and end at **Pₙ**, so the scale directly measures perimeter length.
- **Stacking:** Each n has its own horizontal row with a small vertical gap (`LINE_GAP`), so you can compare growth at a glance.
- **Orthographic camera:** No perspective distortion—the circle stays a circle, and lengths on the scale match what you see.

### Tech (high level)

- **Three.js** (ES modules from CDN), **OrbitControls**, **CSS2DRenderer** for labels.
- **One perimeter line** that either is the closed polygon (with circle offset) or the unfolded horizontal line (and in between, a morph by segment).
- **Completed lines** are stored in a group and drawn with a different material (bright blue); current perimeter is green.
- **Scale labels** (0, 1, 2, 3, 3.14) are placed in 3D just below the scale with `CSS2DObject` so they stay aligned with the scene.

---

## Rules & Aesthetics We Followed

- **One perimeter, one story**  
  The polygon and the horizontal line are the **same** perimeter: it unfolds into the line; we don’t draw a second, separate line.

- **Scale = measurement**  
  - Lines **start at 0** so length is read correctly.  
  - Scale from **0 to π** with 0.1 step **ticks** (lines only); **numbers** only at **0, 1, 2, 3, and 3.14**.  
  - Scale and labels live **in the art area**, just below the line (e.g. via 3D position + CSS2D).

- **Circle vs lines**  
  - Circle moved **left** so its right edge is at scale 0; stacked lines don’t overlap the circle.  
  - **Orthographic** view so the circle is a true circle and lengths are visually correct (no “line looks longer than 3.14” illusion).

- **Stacked history**  
  - Each n gets its **own row** (small vertical gap).  
  - **Old lines:** bright color (e.g. blue), **not faded**, so growth is easy to compare.  
  - **Current line:** green.  
  - **Dotted vertical line at 3.14** so you can see when the line “touches” π.

- **Minimal UI**  
  - **No controls** in the layout; animation runs by itself (default speed and max n in code).  
  - **No error figure** in the overlay; only n, Pₙ, and π (and phase message).  
  - Info (n, Pₙ, π, phase) is **inside the art area** (e.g. one small label in the scene), not in a separate panel “below” the viz.

- **Narrative flow**  
  Circle first → polygon inside → perimeter unfolds one by one → length shown → next polygon; repeat until “π reached ≈ 3.14,” then loop. The intent is to make the **purpose** (perimeter → line → approaches π) obvious from the animation and layout alone.

---

## How to Run

- Open **`index.html`** in a modern browser (ES modules + Three.js from CDN).  
- No install or build step.  
- Optional: serve the folder locally (e.g. `npx serve .`) if your environment requires it for modules.

---

## File

- **`index.html`** — Single file: HTML, CSS, and script (Three.js scene, phases, scale, labels, no external JS files).
