# Hand Giraffe Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** A single HTML file that uses MediaPipe Hands to map 21 hand landmarks into a rendered giraffe shape with yellow fill and brown outlines.

**Architecture:** Single HTML page with inline CSS/JS. Canvas overlay on webcam feed. MediaPipe Hands CDN for landmark detection. Giraffe is drawn by mapping landmarks to giraffe body parts and rendering a filled polygon with decorative details.

**Tech Stack:** HTML5, Canvas 2D, MediaPipe Hands (CDN), Camera API

---

### Task 1: Create base HTML skeleton with canvas, video, and CDN loads

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the base HTML file**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hand Giraffe</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { background: #111; overflow: hidden; font-family: sans-serif; }
    #container { position: relative; width: 100vw; height: 100vh; }
    #canvas { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
    #video { display: none; }
    #pip { position: absolute; bottom: 20px; right: 20px; width: 200px; height: 150px; border-radius: 8px; overflow: hidden; border: 2px solid #555; z-index: 10; }
    #pip video { width: 100%; height: 100%; object-fit: cover; transform: scaleX(-1); }
    #status { position: absolute; top: 20px; left: 50%; transform: translateX(-50%); color: #fff; font-size: 18px; z-index: 10; text-shadow: 0 2px 4px rgba(0,0,0,0.8); background: rgba(0,0,0,0.6); padding: 8px 20px; border-radius: 20px; }
    #instruction { position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%); color: #888; font-size: 14px; z-index: 10; text-shadow: 0 1px 2px rgba(0,0,0,0.8); }
    .hidden { display: none !important; }
  </style>
</head>
<body>
  <div id="container">
    <canvas id="canvas"></canvas>
    <div id="pip" class="hidden"><video id="pipVideo" autoplay playsinline></video></div>
    <div id="status">Loading...</div>
    <div id="instruction">Show your hand to the camera</div>
  </div>
  <video id="video" autoplay playsinline></video>

  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils@0.3/camera_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/control_utils@0.3/control_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.4/hands.js" crossorigin="anonymous"></script>

  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const video = document.getElementById('video');
    const pip = document.getElementById('pip');
    const pipVideo = document.getElementById('pipVideo');
    const statusEl = document.getElementById('status');
    let landmarks = null;
    let camera = null;

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    // Placeholder for Task 2
    // Placeholder for Task 3
    // Placeholder for Task 4
    // Placeholder for Task 5
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify file is created**

Run: `Test-Path -LiteralPath "index.html"`
Expected: `True`

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add base HTML skeleton with canvas and CDN load"
```

---

### Task 2: Implement MediaPipe Hands setup and camera loop

**Files:**
- Modify: `index.html` (replace placeholders with real logic)

- [ ] **Step 1: Replace Task 2 placeholder with camera and hands setup**

In script section, replace `// Placeholder for Task 2` with:

```javascript
    function onResults(results) {
      if (results.multiHandLandmarks && results.multiHandLandmarks.length > 0) {
        landmarks = results.multiHandLandmarks[0];
        setStatus('Hand detected');
      } else {
        landmarks = null;
        setStatus('Show your hand');
      }
      drawGiraffe();
    }

    function setStatus(msg) {
      statusEl.textContent = msg;
    }

    async function initCamera() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        video.srcObject = stream;
        pipVideo.srcObject = stream;
        await video.play();
        pip.classList.remove('hidden');
      } catch (err) {
        setStatus('Camera not available: ' + err.message);
        return;
      }

      const hands = new Hands({
        locateFile: (file) => `https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.4/${file}`
      });

      hands.setOptions({
        maxNumHands: 1,
        modelComplexity: 1,
        minDetectionConfidence: 0.5,
        minTrackingConfidence: 0.5
      });

      hands.onResults(onResults);

      camera = new Camera(video, {
        onFrame: async () => {
          await hands.send({ image: video });
        },
        width: 640,
        height: 480
      });

      setStatus('Initializing hand tracking...');
      await camera.start();
      setStatus('Show your hand');
    }

    initCamera().catch(err => setStatus('Error: ' + err.message));
```

Replace `// Placeholder for Task 3 / Task 4 / Task 5` with empty stub:

```javascript
    function drawGiraffe() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      if (!landmarks) return;
      // Drawing will be implemented in Task 3
    }
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "feat: add MediaPipe hands camera loop"
```

---

### Task 3: Implement hand-to-giraffe landmark mapping

**Files:**
- Modify: `index.html` (replace stub `drawGiraffe` with mapping + rendering)

- [ ] **Step 1: Write the giraffe body point calculation**

Replace the `drawGiraffe` stub with:

```javascript
    function toCanvasCoords(lm) {
      return { x: lm.x * canvas.width, y: lm.y * canvas.height };
    }

    function getGiraffePoints(lms) {
      // Index finger landmarks for head
      const headTop = toCanvasCoords(lms[8]);        // index fingertip
      const headMid = toCanvasCoords(lms[6]);         // index PIP
      const headBase = toCanvasCoords(lms[5]);        // index MCP

      // Middle finger for neck
      const neckTop = toCanvasCoords(lms[12]);
      const neckMid = toCanvasCoords(lms[10]);
      const neckBase = toCanvasCoords(lms[9]);

      // Ring finger for body
      const bodyTop = toCanvasCoords(lms[16]);
      const bodyMid = toCanvasCoords(lms[14]);
      const bodyBase = toCanvasCoords(lms[13]);

      // Thumb for front legs
      const frontLegTop = toCanvasCoords(lms[4]);
      const frontLegMid = toCanvasCoords(lms[3]);
      const frontLegBot = toCanvasCoords(lms[2]);

      // Pinky for back legs/tail
      const backLegTop = toCanvasCoords(lms[20]);
      const backLegMid = toCanvasCoords(lms[18]);
      const backLegBot = toCanvasCoords(lms[17]);

      // Wrist as anchor
      const wrist = toCanvasCoords(lms[0]);

      // Exaggerate neck: stretch middle finger points upward
      const stretchY = 1.5;
      const stretchedNeckTop = { x: neckTop.x, y: neckTop.y - (neckTop.y - neckBase.y) * (stretchY - 1) };
      const stretchedNeckMid = { x: neckMid.x, y: neckMid.y - (neckMid.y - neckBase.y) * (stretchY - 1) * 0.5 };

      // Build giraffe body polygon in drawing order
      return {
        head: { top: headTop, mid: headMid, base: headBase },
        neck: { top: stretchedNeckTop, mid: stretchedNeckMid, base: neckBase },
        body: { top: bodyTop, mid: bodyMid, base: bodyBase },
        frontLeg: { top: frontLegTop, mid: frontLegMid, bot: frontLegBot },
        backLeg: { top: backLegTop, mid: backLegMid, bot: backLegBot },
        wrist
      };
    }

    function getGiraffePolygon(g) {
      // Order for a closed giraffe-shaped polygon:
      // Head top → head base → neck top → neck mid → neck base → body →
      // back leg → tail tip → back to body bottom → front leg → back to body top
      return [
        g.head.top,
        g.head.mid,
        g.head.base,
        g.neck.top,
        g.neck.mid,
        g.neck.base,
        g.body.top,
        g.body.mid,
        g.backLeg.top,
        g.backLeg.mid,
        g.backLeg.bot,
        g.body.base,
        g.wrist,
        g.frontLeg.bot,
        g.frontLeg.mid,
        g.frontLeg.top,
        g.body.base,
        g.body.mid,
        g.body.top,
        g.neck.base,
        g.neck.mid,
        g.neck.top,
        g.head.base,
        g.head.mid,
        g.head.top,
      ];
    }
```

- [ ] **Step 2: Verify the mapping function is syntactically sound**

Quick manual review: function names match, `getGiraffePolygon` references all properties from `getGiraffePoints` return value.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add hand-to-giraffe landmark mapping"
```

---

### Task 4: Implement canvas rendering (filled polygon, spots, details)

**Files:**
- Modify: `index.html` (update `drawGiraffe` to call mapping and render)

- [ ] **Step 1: Write the render function**

Replace the `drawGiraffe` placeholder with real rendering:

```javascript
    function drawGiraffe() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      if (!landmarks) return;

      const pts = getGiraffePoints(landmarks);
      const polygon = getGiraffePolygon(pts);

      // Draw filled body
      ctx.beginPath();
      ctx.moveTo(polygon[0].x, polygon[0].y);
      for (let i = 1; i < polygon.length; i++) {
        ctx.lineTo(polygon[i].x, polygon[i].y);
      }
      ctx.closePath();

      // Fill with giraffe yellow
      ctx.fillStyle = '#ffde2a';
      ctx.fill();

      // Outline in dark brown
      ctx.strokeStyle = '#5b3300';
      ctx.lineWidth = 3;
      ctx.stroke();

      // Draw legs as separate thick lines
      ctx.lineWidth = 8;
      ctx.lineCap = 'round';

      // Front leg
      ctx.beginPath();
      ctx.moveTo(pts.frontLeg.top.x, pts.frontLeg.top.y);
      ctx.lineTo(pts.frontLeg.mid.x, pts.frontLeg.mid.y);
      ctx.lineTo(pts.frontLeg.bot.x, pts.frontLeg.bot.y);
      ctx.stroke();

      // Back leg
      ctx.beginPath();
      ctx.moveTo(pts.backLeg.top.x, pts.backLeg.top.y);
      ctx.lineTo(pts.backLeg.mid.x, pts.backLeg.mid.y);
      ctx.lineTo(pts.backLeg.bot.x, pts.backLeg.bot.y);
      ctx.stroke();

      // Tail (from back leg area extending out)
      ctx.lineWidth = 3;
      ctx.beginPath();
      ctx.moveTo(pts.backLeg.mid.x, pts.backLeg.mid.y);
      const tailEnd = {
        x: pts.backLeg.mid.x - 60,
        y: pts.backLeg.mid.y + 30
      };
      ctx.lineTo(tailEnd.x, tailEnd.y);
      ctx.stroke();

      // Giraffe spots on body
      const bodyCenter = pts.body.base;
      ctx.fillStyle = '#5b3300';
      const spotPositions = [
        { x: -20, y: -20 }, { x: 15, y: -10 }, { x: -5, y: 10 },
        { x: 25, y: 15 }, { x: -15, y: 25 }
      ];
      for (const offset of spotPositions) {
        ctx.beginPath();
        ctx.arc(bodyCenter.x + offset.x, bodyCenter.y + offset.y, 6, 0, Math.PI * 2);
        ctx.fill();
      }

      // Eye dot on head
      ctx.beginPath();
      ctx.arc(pts.head.top.x + 8, pts.head.top.y - 5, 4, 0, Math.PI * 2);
      ctx.fillStyle = '#5b3300';
      ctx.fill();

      // Horns (two small lines from head top)
      ctx.strokeStyle = '#5b3300';
      ctx.lineWidth = 3;
      ctx.beginPath();
      ctx.moveTo(pts.head.top.x - 5, pts.head.top.y);
      ctx.lineTo(pts.head.top.x - 10, pts.head.top.y - 25);
      ctx.stroke();
      ctx.beginPath();
      ctx.moveTo(pts.head.top.x + 5, pts.head.top.y);
      ctx.lineTo(pts.head.top.x + 10, pts.head.top.y - 25);
      ctx.stroke();
    }
```

- [ ] **Step 2: Manual verification — review that all shapes are drawn with correct colors from spec (#ffde2a fill, #5b3300 outline)**

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add giraffe canvas rendering with fill, spots, and details"
```

---

### Task 5: Polish UI states and error handling

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add loading state handling**

Replace `setStatus('Initializing hand tracking...')` logic in `initCamera` to show spinner-style dots:

In the `initCamera` function, change the initial message flow to:

```javascript
      setStatus('📷 Requesting camera...');
      // ... after camera obtained
      setStatus('🖐 Loading hand model...');
      // ... after model loaded
      setStatus('🖐 Show your hand');
```

- [ ] **Step 2: Add error boundary**

Wrap `initCamera()` call with:

```javascript
    initCamera().catch(err => {
      setStatus('⚠️ ' + err.message);
      console.error('Hand Giraffe error:', err);
    });
```

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add loading states and error handling"
```

---

### Task 6: Final testing in browser

**Files:**
- Manual test: open `index.html` in a browser

- [ ] **Step 1: Start a local server**

```bash
npx serve .
```

- [ ] **Step 2: Open in browser and verify**

Navigate to `http://localhost:3000` or whatever serve outputs. Verify:
- Webcam permission prompt appears
- Hand tracking initializes (status changes from "Loading" to "Show your hand")
- When hand is shown to camera, giraffe shape appears with yellow fill and brown outlines
- Giraffe has spots, eye, horns, tail, and legs
- Shape moves in real-time with hand movement
- Picture-in-picture webcam feed visible in bottom-right

- [ ] **Step 3: Test edge cases**
- Cover the camera to make hand undetectable — status should show "Show your hand"
- Deny camera permission — status should show an error message
- Resize the browser window — giraffe should scale correctly

- [ ] **Step 4: Commit any fixes**

```bash
git add index.html
git commit -m "fix: adjustments after browser testing"
```
