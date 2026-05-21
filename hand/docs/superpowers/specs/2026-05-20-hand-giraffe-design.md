# Hand Giraffe — Design Spec

## Overview

A single-page web app that uses real-time hand tracking (MediaPipe Hands) to map the user's 21 hand landmarks into a giraffe shape rendered on an HTML Canvas. The giraffe moves and deforms in real-time as the user's hand moves, with yellow fill and dark brown outlines matching a giraffe aesthetic.

## Tech Stack

- **Single HTML file** — no build tooling
- **MediaPipe Hands** (CDN) — 21 landmark detection from webcam
- **HTML5 Canvas 2D** — rendering
- **Webcam** — input source

## Hand → Giraffe Landmark Mapping

| Hand Landmarks | Giraffe Part | Notes |
|---------------|-------------|-------|
| Index tip (8), PIP (6), MCP (5) | Head, ear/horn | Top region, stretched upward |
| Middle tip (12), PIP (10), MCP (9) | Neck | Longest segment — scaled to exaggerate neck length |
| Ring tip (16), PIP (14), MCP (13) | Body/torso | Central mass, widened |
| Thumb tip (4), IP (3), MCP (2), CMC (1) | Front legs | Side-anchored |
| Pinky tip (20), PIP (18), MCP (17) | Back legs + tail | Opposite side, tail drawn as extra line |
| Wrist (0) + palm center | Belly/chest anchor | Bottom of body |

Raw landmark positions are scaled and stretched to exaggerate giraffe proportions (especially neck length).

## Visual Rendering

1. **Closed polygon** connecting mapped landmarks in giraffe body order
2. **Fill**: `#ffde2a` (yellow, from reference SVG)
3. **Outline**: `#5b3300` (dark brown, thicker for body, thinner for legs)
4. **Giraffe spots**: small dark brown circles scattered on body area
5. **Details**: eye dot on head, two small horn lines
6. Internal reference to the girafe SVG for aesthetic direction

## UI Layout

- Full-screen canvas
- Small webcam picture-in-picture overlay (bottom-right corner)
- Instruction text at bottom of screen

## App States

| State | Behavior |
|-------|----------|
| Loading | Model loading indicator, camera permission prompt |
| No hand detected | Empty canvas or subtle "show your hand" prompt |
| Hand detected | Giraffe renders in real-time following landmarks |
| Camera error | Error message displayed |

## Processing Pipeline (per frame)

```
Webcam frame → MediaPipe → 21 landmarks (x, y, z)
→ Scale/stretch proportions into giraffe shape
→ Draw filled polygon + spots + details on canvas
→ Repeat at ~30fps
```

## Design Choices

- **Single HTML file**: zero setup, open in browser and it works.
- **Real-time update**: the giraffe mirrors the hand live — no freezing, no templates.
- **Giraffe-like proportions**: raw hand skeleton doesn't look like a giraffe; we exaggerate neck length and body width algorithmically.
- **SVG reference**: the girafe-01.svg file in the project provides the color palette and stylistic direction for the drawing.
