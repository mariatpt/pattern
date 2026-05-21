# Animação Cara Reativa ao Som — Plano de Implementação

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar uma página HTML única que mostra um SVG de cara animado por volume do microfone, com cross-fade contínuo entre dois estados.

**Architecture:** Ficheiro HTML único com SVG inline e JavaScript. Web Audio API captura o microfone, calcula RMS com smoothing exponencial e mapeia para opacidade dos grupos SVG.

**Tech Stack:** HTML5, Web Audio API (AnalyserNode), SVG inline, JavaScript puro

---

### Task 1: Criar index.html com SVG inline e estrutura base

**Files:**
- Create: `index.html`

- [ ] **Step 1: Criar o ficheiro index.html com o SVG inline e estrutura inicial**

Conteúdo completo do ficheiro:

```html
<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cara Reativa ao Som</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    background: #1a1a2e;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    font-family: sans-serif;
  }
  svg {
    width: 90vmin;
    height: auto;
  }
  #erro {
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    color: #ff6b6b;
    background: rgba(0,0,0,0.8);
    padding: 12px 24px;
    border-radius: 8px;
    display: none;
  }
</style>
</head>
<body>
<div id="erro"></div>

<svg id="monstro" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 841.89 595.28">
  <defs>
    <style>
      .cls-1 { fill: #312783; }
      #cara_1, #sobrancelhas_1, #lingua_1 { opacity: 1; }
      #cara_2, #sobrancelha2, #lingua_2 { opacity: 0; }
    </style>
  </defs>
  <g id="cara_2" data-name="cara 2">
    <g>
      <path class="cls-1" d="M438.97,348.64c-3.53-.35-12.33-5.93,11.36-13.56,10.87-3.5,34.09-3.13,43.97-1.89,12.22,1.54,48.7,7.5,48.7,7.5,0,0,53.8.36,59.1,0,9.93-.68,37.82-1.1,45.41.21,3.42.59,14.66,8.71,12.29,12.46,0,0-9.95.22-28.39-4.07l-57.21,1.91s-73.55-2-80.37-2.89c-10.79-1.39-44.03,1.4-54.85.32Z"/>
      <path class="cls-1" d="M453.79,348.49c4.05,9.87,8.1,19.73,12.15,29.6.57,1.38,1.14,2.77,1.71,4.15s2.52,1.58,2.98,0c3.03-10.44,6.07-20.88,9.1-31.32.42-1.46.85-2.92,1.27-4.38.26-.9-.48-2.03-1.49-1.95-7.05.57-14.11,1.13-21.16,1.7-1.02.08-2.05.16-3.07.25-1.97.16-1.98,3.25,0,3.09,7.05-.57,14.11-1.13,21.16-1.7,1.02-.08,2.05-.16,3.07-.25l-1.49-1.95c-3.03,10.44-6.07,20.88-9.1,31.32-.42,1.46-.85,2.92-1.27,4.38h2.98c-4.05-9.87-8.1-19.73-12.15-29.6-.57-1.38-1.14-2.77-1.71-4.15-.74-1.81-3.73-1.02-2.98.82Z"/>
      <path class="cls-1" d="M490.45,333.83c1.7-3.73,3.51-7.42,5.38-11.07,3.12-6.09,6.34-12.31,10.48-17.78.81-1.07,1.68-2.12,2.68-3.01.44-.39.44-.38,1.03-.76.06-.04.13-.07.19-.1.25-.13-.27.18.1-.03.03-.02.17-.03.18-.05.05-.1-.29.01.02,0l-1.33-.76c2.08,4.5,4.15,9.01,6.23,13.51,3.33,7.23,6.67,14.46,10,21.68l2.29,4.97.92-2.27c-3.06.64-5.91.91-9.11,1.09-6.44.36-13.03,0-19.22-1.91-3.67-1.13-7.18-2.86-10.1-5.38-1.49-1.29-3.69.88-2.18,2.18,10.13,8.78,25.22,9.3,37.89,7.58,1.18-.16,2.37-.33,3.54-.58,1.06-.22,1.31-1.42.92-2.27-2.08-4.5-4.15-9.01-6.23-13.51-3.33-7.23-6.67-14.46-10-21.68l-2.29-4.97c-.22-.47-.83-.78-1.33-.76-2.2.07-3.97,1.95-5.3,3.5-2.15,2.51-3.95,5.32-5.66,8.14-3.59,5.94-6.75,12.14-9.76,18.39-.69,1.43-1.37,2.85-2.02,4.29-.82,1.79,1.84,3.37,2.67,1.56Z"/>
      <path class="cls-1" d="M544.32,341.69c2.33-4.1,4.76-8.14,7.24-12.14,4.07-6.58,8.22-13.31,13.26-19.21.97-1.13,1.98-2.25,3.14-3.2,0,0,.16-.13.45-.32.2-.13.4-.24.61-.34-.45.21.1-.04.25-.05-.51.03.24.13-.22.02l-.92-.71c6.26,9.38,12.49,18.91,17.77,28.88,1.18,2.23,2.48,4.3,2.87,6.71l1.49-1.95h-47.28c-1.99,0-1.99,3.09,0,3.09h47.28c1.09,0,1.65-.98,1.49-1.95-.41-2.49-1.87-4.95-3.03-7.15-1.86-3.54-3.9-6.98-5.97-10.39-3.58-5.9-7.3-11.71-11.09-17.47-.28-.42-.53-.87-.84-1.27-.72-.93-1.78-.98-2.82-.62-2.17.76-3.92,2.88-5.36,4.57-4.85,5.68-8.9,12.08-12.85,18.4-2.8,4.47-5.52,8.99-8.13,13.57-.98,1.73,1.68,3.29,2.67,1.56Z"/>
      <path class="cls-1" d="M639.09,350.68c-1.97,3.38-4.02,6.72-6.12,10.02-3.31,5.21-6.67,10.63-10.88,15.17-.36.39-.74.78-1.13,1.14-.17.16-.35.31-.52.46-.33.28.06,0-.32.24-.15.09-.3.18-.45.28-.33.21-.05-.06-.08.05-.08.23-.25-.12-.03.03-.31-.22.44.2.29.1.76.5.57.7.37.33-.11-.2-.26-.38-.39-.57l-1.4-2.06c-3.44-5.05-6.88-10.1-10.33-15.16-2.64-3.87-5.28-7.75-7.92-11.62l-1.33,2.32c4.34-.41,8.7-.7,13.05-.92,7.19-.37,14.54-.71,21.71.12,1.22.14,2.64.37,3.92.77.75.24.54.18,1.18.51.45.23-.04-.05.22.15.37.29-.06-.19.15.19.94,1.75,3.6.2,2.67-1.56-1.02-1.92-3.76-2.46-5.7-2.8-3.4-.6-6.88-.73-10.32-.78-7.29-.11-14.59.28-21.85.82-1.68.12-3.36.26-5.04.41-1.07.1-2.05,1.27-1.33,2.32,5.96,8.75,11.92,17.5,17.88,26.25.75,1.1,1.44,2.29,2.27,3.33,1.46,1.84,3.66.53,5.06-.66,2.03-1.73,3.67-3.95,5.26-6.07,2.07-2.75,4-5.61,5.88-8.5,2.73-4.19,5.37-8.44,7.89-12.77,1-1.72-1.67-3.28-2.67-1.56Z"/>
      <path class="cls-1" d="M544.23,207.06c-.06,20.54-9.98,42.54-29.56,51.26-8.66,3.85-18.58,4.22-27.53,1.15-9.14-3.14-16.82-9.74-22.21-17.67-12.36-18.22-13.64-43.46-3.77-63.06,4.41-8.76,11.08-16.48,19.71-21.28s18.17-6.1,27.45-3.81c20.27,4.99,32.87,25.19,35.37,44.91.36,2.82.52,5.66.53,8.5,0,1.99,3.09,1.99,3.09,0-.06-21.61-10.67-45.01-31.37-54.05-9.39-4.1-20.06-4.66-29.77-1.28s-17.9,10.3-23.72,18.76c-13.18,19.17-14.35,45.81-3.95,66.45,4.68,9.29,11.91,17.51,21.09,22.53,9,4.92,19.58,6.43,29.55,3.97,21.46-5.29,34.81-26.47,37.56-47.33.4-3,.59-6.02.6-9.05,0-1.99-3.08-1.99-3.09,0Z"/>
      <path class="cls-1" d="M636.89,212.12c-.06,20.54-9.98,42.54-29.56,51.26-8.66,3.85-18.58,4.22-27.53,1.15-9.14-3.14-16.82-9.74-22.21-17.67-12.36-18.22-13.64-43.46-3.77-63.06,4.41-8.76,11.08-16.48,19.71-21.28,8.32-4.63,18.17-6.1,27.45-3.81,20.27,4.99,32.87,25.19,35.37,44.91.36,2.82.52,5.66.53,8.5,0,1.99,3.09,1.99,3.09,0-.06-21.61-10.67-45.01-31.37-54.05-9.39-4.1-20.06-4.66-29.77-1.28s-17.9,10.3-23.72,18.76c-13.18,19.17-14.35,45.81-3.95,66.45,4.68,9.29,11.91,17.51,21.09,22.53,9,4.92,19.58,6.43,29.55,3.97,21.46-5.29,34.81-26.47,37.56-47.33.4-3,.59-6.02.6-9.05,0-1.99-3.08-1.99-3.09,0Z"/>
      <circle class="cls-1" cx="594.9" cy="212.12" r="21.16"/>
      <circle class="cls-1" cx="500.01" cy="209.34" r="21.16"/>
    </g>
  </g>
  <g id="cara_1" data-name="cara 1">
    <g>
      <path class="cls-1" d="M89.77,380.17c-3.53-.35-12.33-5.93,11.36-13.56,10.87-3.5,34.09-3.13,43.97-1.89,12.22,1.54,48.7,7.5,48.7,7.5,0,0,53.8.36,59.1,0,9.93-.68,37.82-1.1,45.41.21,3.42.59,14.66,8.71,12.29,12.46,0,0-9.95.22-28.39-4.07l-57.21,1.91s-73.55-2-80.37-2.89c-10.79-1.39-44.03,1.4-54.85.32Z"/>
      <path class="cls-1" d="M104.59,380.02c4.05,9.87,8.1,19.73,12.15,29.6.57,1.38,1.14,2.77,1.71,4.15s2.52,1.58,2.98,0c3.03-10.44,6.07-20.88,9.1-31.32.42-1.46.85-2.92,1.27-4.38.26-.9-.48-2.03-1.49-1.95-7.05.57-14.11,1.13-21.16,1.7-1.02.08-2.05.16-3.07.25-1.97.16-1.98,3.25,0,3.09,7.05-.57,14.11-1.13,21.16-1.7,1.02-.08,2.05-.16,3.07-.25l-1.49-1.95c-3.03,10.44-6.07,20.88-9.1,31.32-.42,1.46-.85,2.92-1.27,4.38h2.98c-4.05-9.87-8.1-19.73-12.15-29.6-.57-1.38-1.14-2.77-1.71-4.15-.74-1.81-3.73-1.02-2.98.82Z"/>
      <path class="cls-1" d="M141.24,365.36c1.7-3.73,3.51-7.42,5.38-11.07,3.12-6.09,6.34-12.31,10.48-17.78.81-1.07,1.68-2.12,2.68-3.01.44-.39.44-.38,1.03-.76.06-.04.13-.07.19-.1.25-.13-.27.18.1-.03.03-.02.17-.03.18-.05.05-.1-.29.01.02,0l-1.33-.76c2.08,4.5,4.15,9.01,6.23,13.51,3.33,7.23,6.67,14.46,10,21.68l2.29,4.97.92-2.27c-3.06.64-5.91.91-9.11,1.09-6.44.36-13.03,0-19.22-1.91-3.67-1.13-7.18-2.86-10.1-5.38-1.49-1.29-3.69.88-2.18,2.18,10.13,8.78,25.22,9.3,37.89,7.58,1.18-.16,2.37-.33,3.54-.58,1.06-.22,1.31-1.42.92-2.27-2.08-4.5-4.15-9.01-6.23-13.51-3.33-7.23-6.67-14.46-10-21.68-.76-1.66-1.53-3.32-2.29-4.97-.22-.47-.83-.78-1.33-.76-2.2.07-3.97,1.95-5.3,3.5-2.15,2.51-3.95,5.32-5.66,8.14-3.59,5.94-6.75,12.14-9.76,18.39-.69,1.43-1.37,2.85-2.02,4.29-.82,1.79,1.84,3.37,2.67,1.56Z"/>
      <path class="cls-1" d="M195.11,373.22c2.33-4.1,4.76-8.14,7.24-12.14,4.07-6.58,8.22-13.31,13.26-19.21.97-1.13,1.98-2.25,3.14-3.2,0,0,.16-.13.45-.32.2-.13.4-.24.61-.34-.45.21.1-.04.25-.05-.51.03.24.13-.22.02l-.92-.71c6.26,9.38,12.49,18.91,17.77,28.88,1.18,2.23,2.48,4.3,2.87,6.71l1.49-1.95h-47.28c-1.99,0-1.99,3.09,0,3.09h47.28c1.09,0,1.65-.98,1.49-1.95-.41-2.49-1.87-4.95-3.03-7.15-1.86-3.54-3.9-6.98-5.97-10.39-3.58-5.9-7.3-11.71-11.09-17.47-.28-.42-.53-.87-.84-1.27-.72-.93-1.78-.98-2.82-.62-2.17.76-3.92,2.88-5.36,4.57-4.85,5.68-8.9,12.08-12.85,18.4-2.8,4.47-5.52,8.99-8.13,13.57-.98,1.73,1.68,3.29,2.67,1.56Z"/>
      <path class="cls-1" d="M289.88,382.21c-1.97,3.38-4.02,6.72-6.12,10.02-3.31,5.21-6.67,10.63-10.88,15.17-.36.39-.74.78-1.13,1.14-.17.16-.35.31-.52.46-.33.28.06,0-.32.24-.15.09-.3.18-.45.28-.33.21-.05-.06-.08.05-.08.23-.25-.12-.03.03-.31-.22.44.2.29.1.76.5.57.7.37.33-.11-.2-.26-.38-.39-.57l-1.4-2.06c-3.44-5.05-6.88-10.1-10.33-15.16-2.64-3.87-5.28-7.75-7.92-11.62l-1.33,2.32c4.34-.41,8.7-.7,13.05-.92,7.19-.37,14.54-.71,21.71.12,1.22.14,2.64.37,3.92.77.75.24.54.18,1.18.51.45.23-.04-.05.22.15.37.29-.06-.19.15.19.94,1.75,3.6.2,2.67-1.56-1.02-1.92-3.76-2.46-5.7-2.8-3.4-.6-6.88-.73-10.32-.78-7.29-.11-14.59.28-21.85.82-1.68.12-3.36.26-5.04.41-1.07.1-2.05,1.27-1.33,2.32,5.96,8.75,11.92,17.5,17.88,26.25.75,1.1,1.44,2.29,2.27,3.33,1.46,1.84,3.66.53,5.06-.66,2.03-1.73,3.67-3.95,5.26-6.07,2.07-2.75,4-5.61,5.88-8.5,2.73-4.19,5.37-8.44,7.89-12.77,1-1.72-1.67-3.28-2.67-1.56Z"/>
      <path class="cls-1" d="M195.02,238.59c-.06,20.54-9.98,42.54-29.56,51.26-8.66,3.85-18.58,4.22-27.53,1.15s-16.82-9.74-22.21-17.67c-12.36-18.22-13.64-43.46-3.77-63.06,4.41-8.76,11.08-16.48,19.71-21.28s18.17-6.1,27.45-3.81c20.27,4.99,32.87,25.19,35.37,44.91.36,2.82.52,5.66.53,8.5,0,1.99,3.09,1.99,3.09,0-.06-21.61-10.67-45.01-31.37-54.05-9.39-4.1-20.06-4.66-29.77-1.28s-17.9,10.3-23.72,18.76c-13.18,19.17-14.35,45.81-3.95,66.45,4.68,9.29,11.91,17.51,21.09,22.53,9,4.92,19.58,6.43,29.55,3.97,21.46-5.29,34.81-26.47,37.56-47.33.4-3,.59-6.02.6-9.05,0-1.99-3.08-1.99-3.09,0Z"/>
      <path class="cls-1" d="M287.69,243.65c-.06,20.54-9.98,42.54-29.56,51.26-8.66,3.85-18.58,4.22-27.53,1.15-9.14-3.14-16.82-9.74-22.21-17.67-12.36-18.22-13.64-43.46-3.77-63.06,4.41-8.76,11.08-16.48,19.71-21.28s18.17-6.1,27.45-3.81c20.27,4.99,32.87,25.19,35.37,44.91.36,2.82.52,5.66.53,8.5,0,1.99,3.09,1.99,3.09,0-.06-21.61-10.67-45.01-31.37-54.05-9.39-4.1-20.06-4.66-29.77-1.28-9.77,3.4-17.9,10.3-23.72,18.76-13.18,19.17-14.35,45.81-3.95,66.45,4.68,9.29,11.91,17.51,21.09,22.53,9,4.92,19.58,6.43,29.55,3.97,21.46-5.29,34.81-26.47,37.56-47.33.4-3,.59-6.02.6-9.05,0-1.99-3.08-1.99-3.09,0Z"/>
      <circle class="cls-1" cx="245.69" cy="243.65" r="21.16"/>
      <circle class="cls-1" cx="150.8" cy="240.87" r="21.16"/>
    </g>
  </g>
  <g id="sobrancelhas_1" data-name="sobrancelhas 1">
    <rect class="cls-1" x="439.14" y="120.56" width="99.94" height="11.05" rx="5.53" ry="5.53" transform="translate(64.05 -146.77) rotate(18.24)"/>
    <rect class="cls-1" x="555.57" y="117.45" width="99.94" height="11.05" rx="5.53" ry="5.53" transform="translate(-10.4 172.16) rotate(-16.04)"/>
  </g>
  <g id="sobrancelha2">
    <rect class="cls-1" x="77.21" y="150.38" width="99.94" height="11.05" rx="5.53" ry="5.53" transform="translate(-38.14 41.23) rotate(-16.04)"/>
    <rect class="cls-1" x="224.81" y="153.84" width="99.94" height="11.05" rx="5.53" ry="5.53" transform="translate(63.7 -78.01) rotate(18.24)"/>
  </g>
  <g id="lingua_2" data-name="lingua 2">
    <path class="cls-1" d="M480.44,346.13h125.1v73.1c0,28.7-23.3,52-52,52h-21.1c-28.7,0-52-23.3-52-52v-73.1h0Z"/>
  </g>
  <g id="lingua_1" data-name="lingua 1">
    <path class="cls-1" d="M127.4,379.21h125.1v.37c0,12.13-9.85,21.97-21.97,21.97h-81.15c-12.13,0-21.97-9.85-21.97-21.97v-.37h0Z"/>
  </g>
</svg>

<script>
// Audio + animation logic will go here
</script>
</body>
</html>
```

- [ ] **Step 2: Verificar que o HTML abre no browser**

Run: Abrir `index.html` no browser.
Expected: SVG da cara visível, centrado em fundo escuro, a mostrar o estado 1 (cara, sobrancelhas, língua versão 1).

- [ ] **Step 3: Commit**

```bash
git init
git add index.html
git commit -m "feat: add index.html with inline SVG structure"
```

### Task 2: Adicionar captura de áudio e animação

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Adicionar JavaScript para captura de áudio e cross-fade**

Substituir o comentário `<!-- Audio + animation logic will go here -->` pelo seguinte script:

```html
<script>
const erroEl = document.getElementById('erro');

// Smoothing factor (0-1). Lower = smoother/slower response
const SMOOTHING = 0.15;
// Threshold: ignore background noise below this RMS
const THRESHOLD = 8;
// Max RMS value for normalization (typically 255)
const MAX_RMS = 255;

let smoothVolume = 0;
let audioCtx, analyser, dataArray;
let animId = null;

async function iniciarAudio() {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    audioCtx = new AudioContext();
    const source = audioCtx.createMediaStreamSource(stream);
    analyser = audioCtx.createAnalyser();
    analyser.fftSize = 256;
    source.connect(analyser);
    dataArray = new Uint8Array(analyser.frequencyBinCount);
    erroEl.style.display = 'none';
    animId = requestAnimationFrame(loop);
  } catch (err) {
    erroEl.textContent = 'Microfone não disponível: ' + err.message;
    erroEl.style.display = 'block';
  }
}

function loop() {
  if (!analyser) { animId = requestAnimationFrame(loop); return; }
  analyser.getByteFrequencyData(dataArray);

  // Calculate RMS from frequency data
  let sum = 0;
  for (let i = 0; i < dataArray.length; i++) {
    const val = dataArray[i] / 255;
    sum += val * val;
  }
  const rms = Math.sqrt(sum / dataArray.length);

  // Exponential moving average smoothing
  smoothVolume = smoothVolume * (1 - SMOOTHING) + rms * SMOOTHING;

  // Normalize with threshold
  const t = smoothVolume > THRESHOLD / 255
    ? Math.min((smoothVolume - THRESHOLD / 255) / (1 - THRESHOLD / 255), 1)
    : 0;

  // Apply cross-fade opacity
  document.getElementById('cara_1').style.opacity = 1 - t;
  document.getElementById('cara_2').style.opacity = t;
  document.getElementById('sobrancelhas_1').style.opacity = 1 - t;
  document.getElementById('sobrancelha2').style.opacity = t;
  document.getElementById('lingua_1').style.opacity = 1 - t;
  document.getElementById('lingua_2').style.opacity = t;

  animId = requestAnimationFrame(loop);
}

// Auto-start audio on page load
iniciarAudio();
</script>
```

- [ ] **Step 2: Testar comportamento**

Run: Abrir `index.html` no browser, permitir o microfone quando solicitado. Fazer sons a diferentes volumes.
Expected: Cara, sobrancelhas e língua fazem cross-fade suave entre versão 1 (silêncio) e versão 2 (som alto).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Web Audio API capture with smoothing and cross-fade"
```

### Task 3: Ajustar parâmetros de sensibilidade

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Se necessário, ajustar constantes de sensibilidade**

Localizar no `index.html` as constantes:
- `SMOOTHING` (default 0.15) — reduzir para resposta mais rápida, aumentar para mais suavidade
- `THRESHOLD` (default 8) — aumentar se a cara reagir a ruído ambiente, reduzir se for preciso pouco som para ativar

Abrir o ficheiro, testar com diferentes valores e ajustar conforme necessidade.

- [ ] **Step 2: Verificar comportamento final**

Run: Abrir `index.html` e confirmar que a animação reage corretamente ao som.
Expected: Comportamento suave e responsivo.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "chore: tune smoothing and threshold parameters"
```
