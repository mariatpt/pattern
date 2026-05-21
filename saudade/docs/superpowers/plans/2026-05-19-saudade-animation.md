# Saudade Word Animation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a single-page web animation of "SAUDADE" where letters separate from center outward in pairs, transitioning laranja→vermelho→preto, with a Porto/Douro sunset background.

**Architecture:** Single `index.html` with inline CSS and JS. Letters are `<span>` elements animated via `requestAnimationFrame` for smooth per-letter position and color interpolation. Background uses a free-license Douro sunset image with semi-transparent overlay.

**Tech Stack:** Vanilla HTML/CSS/JS, no dependencies.

---

### Task 1: Create the HTML scaffold and background

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the HTML structure with Douro background**

```html
<!DOCTYPE html>
<html lang="pt">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>saudade</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html, body { width: 100%; height: 100%; overflow: hidden; font-family: 'Segoe UI', Arial, sans-serif; }

    #bg {
      position: fixed; inset: 0;
      background: #000 url('https://images.unsplash.com/photo-1558618666-fcd25c85f82e?w=1920') center/cover no-repeat;
    }
    #overlay {
      position: fixed; inset: 0;
      background: rgba(0,0,0,0.6);
    }
    #container {
      position: fixed; inset: 0;
      display: flex; align-items: center; justify-content: center;
    }
    #word {
      position: relative;
      display: flex;
      font-size: 8rem;
      font-weight: 800;
      color: #FF7700;
      letter-spacing: 0.05em;
      user-select: none;
      text-shadow: 0 0 20px #FF7700, 0 0 40px #FF7700;
    }
    .letter {
      position: relative;
      display: inline-block;
    }
  </style>
</head>
<body>
  <div id="bg"></div>
  <div id="overlay"></div>
  <div id="container">
    <div id="word"></div>
  </div>
  <script>
    // Task 2 will fill this
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify the page loads with the Douro background**

Open `index.html` in a browser. Expected: full-screen Douro sunset image with dark overlay, empty centered space ready for letters.

- [ ] **Step 3: Commit**

```
git add index.html
git commit -m "feat: add html scaffold with Douro background and overlay"
```

### Task 2: Create each letter as a `<span>` with JS

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Write the letter creation JS**

Replace the empty `<script>` with:

```javascript
const word = document.getElementById('word');
const letters = 'SAUDADE'.split('').map((ch, i) => {
  const span = document.createElement('span');
  span.className = 'letter';
  span.textContent = ch;
  span.dataset.index = i;
  word.appendChild(span);
  return span;
});
```

- [ ] **Step 2: Verify letters appear in the center**

Open in browser. Expected: "SAUDADE" displayed in laranja (#FF7700) with glow, centered on screen over the Douro background.

- [ ] **Step 3: Commit**

```
git add index.html
git commit -m "feat: add letter spans for SAUDADE"
```

### Task 3: Implement the animation engine (loop, pairs, timing)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the animation constants and pair definitions**

Append after the letter creation code:

```javascript
const LARANJA = { r: 255, g: 119, b: 0 };
const VERMELHO = { r: 255, g: 0, b: 0 };
const PRETO = { r: 0, g: 0, b: 0 };

const PAIRS = [
  { left: 0, right: 6, delay: 0 },    // S, E
  { left: 1, right: 5, delay: 500 },   // A, D
  { left: 2, right: 4, delay: 1000 },  // U, A
];
const CENTER_IDX = 3; // D (center)
const MOVE_DURATION = 1500; // ms per letter movement
const FADE_DURATION = 1000;
const MOVE_DISTANCE = 350; // px
const PAUSE_AFTER = 2500; // ms pause before restart
```

- [ ] **Step 2: Write the color interpolation function**

```javascript
function lerpColor(c1, c2, t) {
  return {
    r: Math.round(c1.r + (c2.r - c1.r) * t),
    g: Math.round(c1.g + (c2.g - c1.g) * t),
    b: Math.round(c1.b + (c2.b - c1.b) * t),
  };
}

function getColorAtProgress(progress) {
  // 0-0.5: laranja -> vermelho
  // 0.5-1.0: vermelho -> preto
  const t = Math.min(1, Math.max(0, progress));
  if (t < 0.5) {
    return lerpColor(LARANJA, VERMELHO, t * 2);
  } else {
    return lerpColor(VERMELHO, PRETO, (t - 0.5) * 2);
  }
}

function rgbString(c) {
  return `rgb(${c.r},${c.g},${c.b})`;
}
```

- [ ] **Step 3: Write the main animation loop**

```javascript
let animating = false;
let animStartTime = 0;
let phase = 'waiting'; // 'waiting' | 'animating' | 'pausing'
let phaseStartTime = 0;

function startAnimation() {
  animating = true;
  animStartTime = performance.now();
  phase = 'animating';
  phaseStartTime = animStartTime;
  requestAnimationFrame(tick);
}

function tick(now) {
  if (!animating) return;

  const elapsed = now - phaseStartTime;

  if (phase === 'animating') {
    // Update each pair
    const maxDuration = PAIRS.length > 0
      ? Math.max(...PAIRS.map(p => p.delay + MOVE_DURATION))
      : 0;
    const centerDuration = (PAIRS.length) * 500 + FADE_DURATION;
    const totalAnimDuration = Math.max(maxDuration, centerDuration + 500);

    if (elapsed >= totalAnimDuration) {
      phase = 'pausing';
      phaseStartTime = now;
      // hide all letters
      letters.forEach(l => l.style.opacity = '0');
    } else {
      // Reset all to hidden first, then show active ones
      letters.forEach(l => {
        l.style.opacity = '0';
        l.style.transform = 'translateX(0)';
        l.style.color = rgbString(LARANJA);
        l.style.textShadow = `0 0 20px ${rgbString(LARANJA)}, 0 0 40px ${rgbString(LARANJA)}`;
      });

      // Animate pairs
      PAIRS.forEach((pair) => {
        const localT = elapsed - pair.delay;
        if (localT < 0 || localT > MOVE_DURATION) return;

        const progress = localT / MOVE_DURATION;
        const eased = 1 - Math.pow(1 - progress, 3); // ease-out cubic
        const color = getColorAtProgress(progress);
        const colorStr = rgbString(color);
        const glowStr = `0 0 20px ${colorStr}, 0 0 40px ${colorStr}`;
        const offset = eased * MOVE_DISTANCE;

        const leftLetter = letters[pair.left];
        leftLetter.style.opacity = '1';
        leftLetter.style.transform = `translateX(-${offset}px)`;
        leftLetter.style.color = colorStr;
        leftLetter.style.textShadow = glowStr;

        const rightLetter = letters[pair.right];
        rightLetter.style.opacity = '1';
        rightLetter.style.transform = `translateX(${offset}px)`;
        rightLetter.style.color = colorStr;
        rightLetter.style.textShadow = glowStr;
      });

      // Animate center D
      const centerDelay = PAIRS.length * 500;
      const centerT = elapsed - centerDelay;
      if (centerT >= 0 && centerT < FADE_DURATION) {
        const progress = centerT / FADE_DURATION;
        const eased = 1 - Math.pow(1 - progress, 3);
        const color = getColorAtProgress(progress);
        const colorStr = rgbString(color);
        const opacity = 1 - eased;
        const centerLetter = letters[CENTER_IDX];
        centerLetter.style.opacity = String(opacity);
        centerLetter.style.color = colorStr;
        centerLetter.style.textShadow = `0 0 20px ${colorStr}, 0 0 40px ${colorStr}`;
      }
    }
  } else if (phase === 'pausing') {
    if (elapsed >= PAUSE_AFTER) {
      // Reset and restart
      letters.forEach(l => {
        l.style.opacity = '1';
        l.style.transform = 'translateX(0)';
        l.style.color = rgbString(LARANJA);
        l.style.textShadow = `0 0 20px ${rgbString(LARANJA)}, 0 0 40px ${rgbString(LARANJA)}`;
      });
      phase = 'animating';
      phaseStartTime = now;
    }
  }

  requestAnimationFrame(tick);
}

startAnimation();
```

- [ ] **Step 4: Verify animation in browser**

Open `index.html`. Expected: Animation plays — S/E move outward first, then A/D, then U/A, D fades. Each letter transitions laranja→vermelho→preto. After completion, pause, then loop restarts.

- [ ] **Step 5: Commit**

```
git add index.html
git commit -m "feat: implement letter pair animation with color transition"
```

### Task 4: Polish — glow, easing, and visual tuning

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Ensure container and letters use `will-change` for GPU acceleration**

```css
#word { will-change: transform; }
.letter { will-change: transform, opacity, color; }
```

Add just before the closing `</style>` tag.

- [ ] **Step 2: Verify performance**

Open in browser, check that animation is smooth (60fps). No stuttering.

- [ ] **Step 3: Commit**

```
git add index.html
git commit -m "chore: add will-change for GPU acceleration"
```
