---
name: excalidraw-skill
description: High-fidelity canvas drawing toolkit. Use for all Excalidraw drawing tasks. Enforces a 5-phase workflow (Plan→Sketch→Build→Detail→Finalize) that produces professional-quality output. Requires a running canvas server at EXPRESS_SERVER_URL (default http://localhost:4321).
---

# Excalidraw Drawing Skill — 5-Phase Workflow

## Step 0: Detect Connection Mode

Before doing anything else, check which mode is available:

```bash
curl -s http://localhost:4321/health
```

If response contains `"status":"healthy"` → canvas server is running. Proceed.
If not → start it: `cd ~/.local/share/mcp-excalidraw && PORT=4321 node dist/server.js &`

## The 5-Phase Drawing Session

**This is a RIGID workflow. Phases are not optional. Do not skip exit gates.**

---

### Phase 1 — PLAN (Entry point for every new drawing)

1. Call `read_diagram_guide` — load color and composition rules
2. Call `plan_composition` with:
   - `description`: what you're drawing
   - `drawing_type`: arch | infographic | art | ui | map
   - `canvas_width` / `canvas_height` (default 1200×800)
   - `style_preset`: Sketchy | Clean | Painted | Technical | Isometric
3. Output the full blueprint JSON to the conversation
4. **EXIT GATE: blueprint confirmed before any elements are created**

---

### Phase 2 — SKETCH (Structure first)

1. Clear the canvas: call `clear_canvas`
2. Draw structural anchors only: zone boundary rectangles (transparent fill), major shape outlines, key text labels
3. Use `layer: 0` for all sketch elements
4. Target: 10–20% of final element count — skeleton only
5. Call `get_canvas_screenshot` to verify zone coverage visually
6. **EXIT GATE: all major zones visible in screenshot**

---

### Phase 3 — BUILD (Fill zones to density targets)

1. For each zone from the blueprint, add content elements until `current >= densityTarget.min`
2. Assign `layer: 1` for content elements, `layer: 2` for foreground highlights
3. Use ONLY colors from the blueprint palette (by slot name → hex)
4. If style calls for gradients: call `simulate_gradient` for background zones
5. Call `apply_style_preset` with the blueprint's `stylePreset` and `element_ids: "all"`
6. **EXIT GATE: all zones at ≥ 80% of their densityTarget.min**

---

### Phase 4 — DETAIL (Analysis-driven refinement)

1. Call `analyze_composition` with the planId
2. Read the score and feedback list
3. Work through feedback items top-to-bottom (highest impact first):
   - Under-density zones → add elements
   - Low color adherence → update element colors to palette hex values
   - Balance issues → add elements to lighter side
4. Add fine details: shadow shapes (opacity 15–25%), texture freedraw strokes, accent highlights
5. Re-call `analyze_composition` after each major change to track score progress
6. **EXIT GATE: composition score ≥ 75**

---

### Phase 5 — FINALIZE (Polish and export)

1. Call `suggest_refinements` with `target_score: 85` — work through returned action list
2. Call `set_viewport` with `scrollToContent: true` to frame the full composition
3. Call `get_canvas_screenshot` and present to the user
4. Export: `POST /api/export/image` with `{"format":"png","scale":4}` for 4× PNG
   - Use `scale: 8` for print-quality output
5. Call `snapshot_scene` to save a named restore point
6. **EXIT GATE: score ≥ 85, screenshot shown, export path returned to user**

---

## Handling Mid-Session Changes

Re-enter at the appropriate phase — do NOT restart:
- Color or style change → re-enter Phase 3
- Layout or zone change → re-enter Phase 2
- Detail refinement only → re-enter Phase 4
- The Phase 5 snapshot provides a rollback point at any time

---

## MCP vs REST API Quick Reference

| Operation | MCP Tool | REST API |
|-----------|----------|----------|
| Plan composition | `plan_composition` | — |
| Analyze quality | `analyze_composition` | `GET /api/composition/metrics` |
| Apply style | `apply_style_preset` | — |
| Simulate gradient | `simulate_gradient` | — |
| Suggest refinements | `suggest_refinements` | — |
| Create elements | `batch_create_elements` | `POST /api/elements/batch` (add `layer` field) |
| High-res export | `export_to_image` | `POST /api/export/image {"format":"png","scale":4}` |
| Screenshot | `get_canvas_screenshot` | — |
| Drawing guide | `read_diagram_guide` | — |
| Clear canvas | `clear_canvas` | `DELETE /api/elements/clear` |
| Snapshot | `snapshot_scene` | `POST /api/snapshots` |
