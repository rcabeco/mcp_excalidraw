# Excalidraw Drawing Guide — High-Fidelity Edition

## Chapter 1 — Color Theory

### Palette Construction
Choose ONE strategy per drawing:
- **Analogous** (harmonious, calm): 3 adjacent hues on the color wheel. Good for art, infographics.
- **Complementary** (high contrast, energetic): 2 opposite hues. Good for UI, arch diagrams.
- **Triadic** (vibrant, complex): 3 equidistant hues. Use sparingly — easy to overdo.

### Value Contrast Rules
- Background: L > 80% (light) or L < 20% (dark) — never midrange
- Midground content: L 40–70%
- Foreground details: L < 30% (light theme) or L > 70% (dark theme)

### Opacity Layering
- Shadow shapes: opacity 15–25%
- Highlight shapes: opacity 30–50%
- Detail strokes: opacity 70–100%
- Background gradients: opacity 60–85%

### Named Slot System
**Always use slot names when referencing palette colors.** Never hardcode hex in tool descriptions.

| Slot | Purpose |
|------|---------|
| background | Canvas/page color |
| surface | Card/panel color |
| primary | Main action color |
| accent | Highlights, CTAs |
| shadow | Depth, drop shadows |
| highlight | Glow, emphasis |
| text | All text elements |

---

## Chapter 2 — Composition Rules

### Rule of Thirds
Divide the canvas into a 3×3 grid. Place primary subjects at the 4 intersection points, not dead center. The center cell is the weakest position.

### Visual Hierarchy
- Hero element must be ≥ 3× the area of supporting elements
- Use size, color contrast, and position to signal importance
- Only ONE element can be the hero per drawing

### Depth Layers (minimum 3 planes)
1. **Background (layer 0)**: Large, low-contrast shapes. Subtle colors. Sets the scene.
2. **Midground (layer 1)**: Content elements. Medium size, medium contrast.
3. **Foreground (layer 2)**: Details, labels, highlights. Small, high contrast.

### Visual Flow
Lead the eye from top-left to your primary subject using:
- Diagonal shapes or lines pointing toward the focal point
- Directional arrows or curves
- Color gradients that guide attention

### Negative Space
**20–30% of the canvas should be empty.** More elements ≠ better drawing. Empty space creates rest, focus, and elegance. Resist filling every pixel.

---

## Chapter 3 — Per-Type Drawing Recipes

### Architecture Diagrams
- **Preset:** Technical
- **Zones:** title-bar (top, full width), legend (left rail 160px), main-grid (remainder)
- **Node size:** 80×50px minimum per service
- **Connectors:** elbow arrows with labels; dashed for async flows
- **Color rule:** monochrome palette + 1 accent color only
- **Stroke widths:** 1px grid, 2px nodes, 3px primary path

### Visual Infographics
- **Preset:** Clean
- **Zones:** hero (top 30% full-width), 3 equal columns, footer CTA
- **Hero:** use `simulate_gradient` for richness
- **Icons:** 40×40px circles as visual anchors, one per column
- **Typography:** 3 sizes only — 24px heading, 14px body, 11px label
- **Color rule:** analogous palette, consistent fill per column

### Artistic Scenes
- **Preset:** Painted
- **Zones:** sky (0–35% height), horizon band (35–50%), midground (50–80%), foreground (80–100%)
- **Minimum elements:** 80 for adequate visual richness
- **Texture:** use `freedraw` elements at low opacity for texture simulation
- **Depth:** background=layer 0, scene=layer 1, detail=layer 2, focal point=layer 3
- **Gradients:** simulate sky with `simulate_gradient` (vertical), ground with horizontal variant

### UI / Wireframe Mockups
- **Preset:** Clean
- **Zones:** nav bar (48px top), sidebar (left 20%), content area (remainder)
- **Containers:** rectangles only, no fill or very light fill
- **Text sizes:** 24px headings, 14px body, 11px labels — no other sizes
- **Input fields:** no fill, strokeWidth 1, strokeColor = surface slot
- **Spacing rule:** 8px grid — all elements snap to multiples of 8

### Maps & Floorplans
- **Preset:** Technical
- **Grid:** snap all coordinates to 40px increments
- **Rooms:** labeled rectangles, strokeWidth 1, light fill
- **Paths:** dashed lines for routes/corridors
- **Legend:** bottom-right corner, 200×150px zone
- **Scale indicator:** always include a reference element labeled with real-world units

---

## Chapter 4 — AI Anti-Patterns

| Anti-Pattern | Symptom | Fix |
|---|---|---|
| **Random color spray** | Different color per element | Stick to the 7 palette slots from plan_composition |
| **Size uniformity** | All elements the same size | Vary by depth: large bg, medium mid, small fg |
| **Center gravity** | Everything clustered in the middle | Fill zone density targets before adding detail |
| **One-pass drawing** | All elements in a single batch | The 5-phase workflow prevents this — never skip phases |
| **Ignoring feedback** | analyze_composition called but not acted on | Score gates enforce minimum improvement before next phase |
| **Stroke monotony** | Same strokeWidth everywhere | 1px background / 2px midground / 3px foreground |
| **Dense center, empty edges** | High coverage_ratio center, empty corners | Check 3×3 density grid — fill corner cells |
| **Off-palette colors** | color_adherence < 70% | Query elements, bulk-update colors to palette hex values |
