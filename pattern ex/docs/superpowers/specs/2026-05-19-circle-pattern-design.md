# Circle Pattern Canvas — Design Spec

## Overview
A single-file HTML web tool that renders a full-canvas repeating pattern. The pattern unit is a static group of three circles (1 large + 2 small) arranged vertically, each group placed at a random position with a random fixed rotation.

## Geometry of a Group
- Small circle radius: `r`
- Large circle radius: `R = 3r`
- Gap between all circle surfaces: `g` (circles never touch or overlap)
- The two small circles sit side by side above the large one, forming a triangular/vertical composition
- The group rotates around its **centroid** (the average of the three circle centers)
- The centroid is the rotation pivot — ensures centered, balanced rotation

## Positioning & Distribution
- Canvas resizes to fill the browser window (handles `resize` events)
- Groups are positioned in a grid with random jitter to fill the canvas without overlaps
- Collision detection ensures no circles from different groups overlap (minimum gap `g` enforced between all circles globally)
- Number of groups is calculated automatically based on canvas size and group bounding box

## Rotation
- Each group receives a random rotation angle (fixed, no animation)
- Angles are uniformly distributed between 0 and 2π

## Color
- All circles in all groups share a single configurable color (default: blue)

## Rendering
- Pure Canvas 2D API (`<canvas>` element)
- Single render pass (no animation loop)
- Anti-aliased circle rendering
- Background color: white (or configurable)

## File Structure
- Single `index.html` file
- All HTML, CSS, and JavaScript inline
- Zero external dependencies

## Constraints Summary
| Requirement | Implementation |
|---|---|
| 1 large circle per group | Radius `3r` |
| 2 small circles per group | Radius `r` each, above the large one |
| Space between all circles | Gap `g` enforced, no touching/overlap |
| 3 circles = single shape | Group drawn as unit, rotates as unit |
| No rotation animation | Static with random angle |
| Centered rotation | Pivot = group centroid |
| Fixed distances within group | `r` and `g` are constants |
