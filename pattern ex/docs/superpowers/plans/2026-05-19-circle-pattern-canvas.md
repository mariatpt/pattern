# Circle Pattern Canvas — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** A single-file HTML page that renders a full-canvas repeating pattern of circle groups on a `<canvas>`.

**Architecture:** One `index.html` file with inline CSS and JS. A `Group` class encapsulates the 3-circle geometry, centroid calculation, and drawing. A random grid-with-jitter placement algorithm fills the canvas without overlaps. A single render pass draws all groups with their random rotations.

**Tech Stack:** HTML5, Canvas 2D API, vanilla JavaScript (ES6), no dependencies.

**File:** Create `pattern ex/index.html`

---

### Task 1: HTML scaffold and canvas setup

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create the HTML file with canvas element**

```html
<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Circle Pattern</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  html, body { width: 100%; height: 100%; overflow: hidden; background: #fff; }
  canvas { display: block; width: 100%; height: 100%; }
</style>
</head>
<body>
<canvas id="canvas"></canvas>
<script>
"use strict";
</script>
</body>
</html>
```

- [ ] **Step 2: Add canvas resize logic**

Inside the `<script>` tag:

```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

window.addEventListener('resize', () => {
  resize();
  render();
});

resize();
```

- [ ] **Step 3: Verify file opens in browser**

- Open `index.html` in browser
- Expected: white fullscreen canvas, no errors in console

---

### Task 2: Group geometry constants and centroid calculation

**Files:**
- Modify: `index.html` (inside `<script>`)

- [ ] **Step 1: Define geometry constants**

```javascript
const R_SMALL = 10;      // radius of small circles (r)
const R_LARGE = 30;      // radius of large circle (3r)
const GAP = 4;           // gap between circle surfaces
const COLOR = '#3b82f6'; // blue
```

- [ ] **Step 2: Write Group centroid calculation**

```javascript
function computeCentroid(cx, cy, angle) {
  // Returns {cx, cy, angle} struct — centroid is the average of the 3 circle centers
  // Small circles side-by-side above the large one, arranged vertically
  // All measured from the centroid pivot point
  return { cx, cy, angle };
}
```

This is a stub — full geometry is implemented in Task 3.

- [ ] **Step 3: Test constants are defined**

Open `index.html`, check console for no errors.

---

### Task 3: Group geometry — positions of the 3 circles relative to centroid

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Compute local positions of the 3 circles relative to the group centroid**

```javascript
function getGroupCircles(rSmall, rLarge, gap) {
  // Arrangement: 2 small circles side by side above the large circle
  // All separated by `gap` between surfaces
  // Returns array of {x, y, radius} centered around (0,0) — the centroid

  const smallDiameter = rSmall * 2;
  const largeDiameter = rLarge * 2;

  // Vertical distance: from center of small to center of large
  // = smallRadius + gap + largeRadius
  const vertDist = rSmall + gap + rLarge;

  // Horizontal distance between the two small circles
  // = smallDiameter + gap
  const horizDist = smallDiameter + gap;

  // Centroid is the average of the 3 circle centers.
  // Place large at (0, -yOffset), small at (-horizDist/2, +yOffset) and (+horizDist/2, +yOffset)
  // Solve for yOffset such that centroid = average of the 3 = (0,0)

  // Large center: (0, -yLarge)
  // Small 1 center: (-horizDist/2, ySmall)
  // Small 2 center: (+horizDist/2, ySmall)
  // Centroid y = (-yLarge + ySmall + ySmall) / 3 = 0 => ySmall = yLarge / 2
  // But also need: ySmall - (-yLarge) = vertDist => ySmall + yLarge = vertDist
  // So: yLarge/2 + yLarge = vertDist => 1.5 * yLarge = vertDist => yLarge = (2/3) * vertDist
  //     ySmall = vertDist / 3

  const yLarge = (2 / 3) * vertDist;
  const ySmall = vertDist / 3;

  return [
    { x: 0,          y: -yLarge,          radius: rLarge },
    { x: -horizDist / 2, y: ySmall,       radius: rSmall },
    { x:  horizDist / 2, y: ySmall,       radius: rSmall },
  ];
}
```

- [ ] **Step 2: Verify geometry with a quick console test**

```javascript
const circles = getGroupCircles(R_SMALL, R_LARGE, GAP);
console.log('Circle centers:', circles);
// Check: no overlapping, gap between surfaces = GAP
// distance between small centers = horizDist = 2*rSmall + GAP
// distance from small to large = rSmall + GAP + rLarge
```

---

### Task 4: Group drawing function with rotation

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Write function to draw one group at a given position and angle**

```javascript
function drawGroup(ctx, cx, cy, angle, circles, color) {
  ctx.save();
  ctx.translate(cx, cy);
  ctx.rotate(angle);
  ctx.fillStyle = color;

  for (const c of circles) {
    ctx.beginPath();
    ctx.arc(c.x, c.y, c.radius, 0, Math.PI * 2);
    ctx.fill();
  }

  ctx.restore();
}
```

- [ ] **Step 2: Test by drawing one group at canvas center**

```javascript
function render() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  const circles = getGroupCircles(R_SMALL, R_LARGE, GAP);
  drawGroup(ctx, canvas.width / 2, canvas.height / 2, Math.PI / 4, circles, COLOR);
}

render();
```

Open in browser — expect one blue group of 3 circles at center, rotated 45°.

---

### Task 5: Grid-with-jitter placement algorithm

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Compute bounding box of a group**

```javascript
function getGroupBounds(circles) {
  let minX = Infinity, maxX = -Infinity, minY = Infinity, maxY = -Infinity;
  for (const c of circles) {
    minX = Math.min(minX, c.x - c.radius);
    maxX = Math.max(maxX, c.x + c.radius);
    minY = Math.min(minY, c.y - c.radius);
    maxY = Math.max(maxY, c.y + c.radius);
  }
  const w = maxX - minX;
  const h = maxY - minY;
  return { w, h };
}
```

- [ ] **Step 2: Generate group placements with grid + jitter**

```javascript
function generatePlacements(canvasW, canvasH, circles) {
  const bounds = getGroupBounds(circles);
  const spacingX = bounds.w + GAP;
  const spacingY = bounds.h + GAP;

  const cols = Math.ceil(canvasW / spacingX) + 1;
  const rows = Math.ceil(canvasH / spacingY) + 1;

  const jitterX = spacingX * 0.3;
  const jitterY = spacingY * 0.3;

  const placements = [];

  for (let row = 0; row < rows; row++) {
    for (let col = 0; col < cols; col++) {
      const bx = col * spacingX + (row % 2) * (spacingX / 2);
      const by = row * spacingY;
      const px = bx + (Math.random() - 0.5) * jitterX;
      const py = by + (Math.random() - 0.5) * jitterY;
      const angle = Math.random() * Math.PI * 2;
      placements.push({ cx: px, cy: py, angle });
    }
  }

  return placements;
}
```

- [ ] **Step 3: Update render to use placements**

```javascript
function render() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  const circles = getGroupCircles(R_SMALL, R_LARGE, GAP);
  const placements = generatePlacements(canvas.width, canvas.height, circles);

  for (const p of placements) {
    drawGroup(ctx, p.cx, p.cy, p.angle, circles, COLOR);
  }
}
```

- [ ] **Step 4: Test in browser**

Open `index.html` — expect canvas filled with rotated groups, no overlaps.

---

### Task 6: Refine and polish

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add a padding/margin so groups near edges don't clip**

```javascript
function generatePlacements(canvasW, canvasH, circles) {
  const bounds = getGroupBounds(circles);
  const padX = bounds.w / 2;
  const padY = bounds.h / 2;
  const spacingX = bounds.w + GAP;
  const spacingY = bounds.h + GAP;

  const cols = Math.ceil((canvasW + padX * 2) / spacingX) + 1;
  const rows = Math.ceil((canvasH + padY * 2) / spacingY) + 1;

  const jitterX = spacingX * 0.3;
  const jitterY = spacingY * 0.3;

  const placements = [];

  for (let row = 0; row < rows; row++) {
    for (let col = 0; col < cols; col++) {
      const bx = -padX + col * spacingX + (row % 2) * (spacingX / 2);
      const by = -padY + row * spacingY;
      const px = bx + (Math.random() - 0.5) * jitterX;
      const py = by + (Math.random() - 0.5) * jitterY;
      const angle = Math.random() * Math.PI * 2;
      placements.push({ cx: px, cy: py, angle });
    }
  }

  return placements;
}
```

- [ ] **Step 2: Test in browser**

Resize window — groups should re-flow to fill the new canvas size.

- [ ] **Step 3: Final visual check**

Open `index.html` and verify:
- Groups fill the entire canvas
- No circles touch or overlap
- Each group has a random rotation
- One color (blue) throughout
- All gaps are consistent within groups
- Resize works correctly
