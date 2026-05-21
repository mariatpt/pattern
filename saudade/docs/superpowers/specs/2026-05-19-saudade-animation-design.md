# Saudade Word Animation — Design Spec

## Overview
A single-page web animation of the word "SAUDADE" where letters separate from the center outward in pairs, with color transitioning through laranja → vermelho → preto during each letter's movement. The background is a photo of the Douro river at sunset with a semi-transparent black overlay. Runs in an infinite loop.

## Background
- Full-screen photo of Douro river sunset, covering the viewport
- Semi-transparent black overlay (`rgba(0,0,0,0.6)`) over the image
- Image scales to cover (`object-fit: cover`)

## Text Animation
- Word: **SAUDADE** (7 letters)
- Font: Large bold sans-serif (~8rem), centered on screen
- Each letter is an absolutely-positioned `<span>` inside a centered container
- Letters separate in pairs from the center outward:

| Pair | Letters | Direction | Delay |
|------|---------|-----------|-------|
| 1    | S ←, E → | Outward   | 0.0s  |
| 2    | A ←, D → | Outward   | 0.5s  |
| 3    | U ←, A → | Outward   | 1.0s  |
| 4    | D (center) | Fade out | 1.5s  |

- Each letter moves ~300-400px outward over ~1.5s
- Smooth color interpolation during movement: `laranja (#FF7700)` → `vermelho (#FF0000)` → `preto (#000000)` — each letter's color progresses through all three stops over its movement duration
- Glow effect on each letter during movement (laranja → vermelho glow that fades as letter turns black)

## Loop Cycle
1. Letters start together at center, colored laranja
2. Pairs animate outward sequentially with staggered delays
3. After all letters complete (~2.5s total), there is a ~1.5s pause
4. Letters instantly reset: opacity set to 0, position reset to center, color reset to laranja, then opacity restored to 1
5. Pause ~0.5s
6. Repeat from step 2

## Technical Approach: CSS + JavaScript
- **HTML**: Single `index.html` with inline CSS and JS
- Each letter as a `<span>` inside a positioned container
- JavaScript uses `requestAnimationFrame` to drive per-letter animation:
  - Position interpolated from center → target offset (easing function)
  - Color interpolated through laranja → vermelho → preto using a multi-stop gradient function
- Or alternatively: CSS `@keyframes` triggered by JS class toggling for staggering

## No external dependencies
- No frameworks, no libraries
- Works in modern browsers (Chrome, Firefox, Safari, Edge)

## File structure
```
saudade/
  index.html          (single file, everything inline)
  docs/
    superpowers/
      specs/
        2026-05-19-saudade-animation-design.md
```
