# Maria's Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Single-page portfolio gateway with envelope project cards linking to 4 existing subfolder projects.

**Architecture:** Single `index.html` at `mysite/` root containing all CSS in `<style>` and JS in `<script>`. 4 full-viewport sections (hero, about, contact) plus envelope project cards scattered in hero. Nav bar scrolls to sections. Envelope clicks open projects in new tabs.

**Tech Stack:** Vanilla HTML5/CSS3/JS. Google Fonts (Caveat). No build tools.

---

### Task 1: HTML Skeleton, Nav Bar & Global Styles

**Files:**
- Create: `C:\Users\cm21m\OneDrive\Ambiente de Trabalho\ERASMUS\ABADIR\Abadir Workshop may\mysite\index.html`

- [ ] **Step 1: Create the HTML skeleton with head and nav**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MARIA'S SITE</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@400;700&display=swap" rel="stylesheet" />
  <style>
    /* Reset and base */
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      background: #EDE8E4;
      color: #312783;
      overflow-x: hidden;
    }
    ::selection { background: #C23F1C; color: #fff; }

    /* Nav */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      height: 52px; background: #312783; z-index: 1000;
      display: flex; align-items: center; justify-content: flex-end;
      padding: 0 32px; gap: 24px;
    }
    nav a {
      color: #fff; text-decoration: none; font-size: 14px;
      font-weight: 500; letter-spacing: 0.05em; cursor: pointer;
      transition: opacity 0.3s;
    }
    nav a:hover { opacity: 0.7; }

    /* Section base */
    section { min-height: 100vh; padding: 80px 48px 48px; }
  </style>
</head>
<body>
  <nav>
    <a onclick="document.getElementById('about').scrollIntoView({behavior:'smooth'})">About</a>
    <a onclick="document.getElementById('contact').scrollIntoView({behavior:'smooth'})">Contact</a>
    <a onclick="document.getElementById('hero').scrollIntoView({behavior:'smooth'})">Projects</a>
  </nav>
  <!-- Sections will be added in subsequent tasks -->
</body>
</html>
```

- [ ] **Step 2: Save and preview**

Run a local server and open the file to verify the nav bar appears correctly.

In Windows, you can just double-click the HTML file or run:
```powershell
Start-Process "C:\Users\cm21m\OneDrive\Ambiente de Trabalho\ERASMUS\ABADIR\Abadir Workshop may\mysite\index.html"
```
Expected: Blue bar at top with white About / Contact / Projects links.

---

### Task 2: Hero Section — Title & Layout

**Files:**
- Modify: `index.html` (add hero section after `<nav>`)

- [ ] **Step 1: Add hero section HTML**

Inside `<body>`, after `</nav>`, add:

```html
  <section id="hero">
    <div class="hero-title">
      <span class="hero-line">MARIA'S</span>
      <span class="hero-line">SITE</span>
    </div>
    <!-- envelopes will be added in Task 3 -->
  </section>
```

- [ ] **Step 2: Add hero styles** inside the `<style>` block

```css
    #hero {
      display: flex; align-items: center; justify-content: flex-start;
      background: #EDE8E4; position: relative; overflow: hidden;
    }
    .hero-title {
      display: flex; flex-direction: column;
      margin-left: 8vw; z-index: 2;
    }
    .hero-line {
      font-family: 'Helvetica Neue', 'Arial Black', sans-serif;
      font-weight: 900; font-size: clamp(4rem, 14vw, 10rem);
      line-height: 0.9; color: #312783; letter-spacing: -0.02em;
      text-transform: uppercase;
    }
    .hero-line:first-child { margin-left: -0.05em; }
```

- [ ] **Step 3: Verify**

Open the page. Expected: "MARIA'S" above "SITE" in huge bold text, left-aligned, on off-white background.

---

### Task 3: Envelope Project Cards

**Files:**
- Modify: `index.html` (envelopes inside hero, styles, and click JS)

- [ ] **Step 1: Add envelope SVG markup** (based on provided envelope style)

Inside `#hero`, after `.hero-title`, add:

```html
    <!-- Envelope 1 → hand -->
    <div class="envelope env-1" onclick="window.open('hand/index.html','_blank')">
      <svg viewBox="0 0 160.68 89.8" fill="#C23F1C" class="env-svg">
        <path d="M140.76,3.8c-.49-.41-.55-.94-.9-1.28-1.42-1.38-2.73-.68-4.43-.82-2.7-.21-5.54-.73-8.1-.81-2.53-.08-5.03.22-7.54.13l-.12-1.03c-.27.05-.53.09-.8.13l.14,1.14-20.17.6c-.93.15-1.86.3-2.79.42-11.53,1.45-23.2,1.88-34.76,3.08-12.04,1.25-23.87,3.76-35.88,5.18-5.96.7-11.94,1.23-17.92,1.72-1.68,1.81-2.34,4.16-3.3,6.54-1.02-1.54-2.42-1.85-3.75-.46.07,1.93-.39,3.84-.42,5.77-.09,5.17,1.23,10.26,2.17,15.26,2.24,11.92,4.38,23.85,7.14,35.65.26.5,2.27,3.5,2.49,3.52,1.93.14,1.72-3.03,1.92-4,.07-.34.48-.6.58-.98,1.1-4.23-1.49-2.63-1.78-2.83-.05-.04-.31-.98-.35-1.19-1.52-7.29-2.77-14.63-4.17-21.94-1.62-8.42-4.3-16.96-3.67-25.62,2.15.6,2.09-1.18,2.81-2.05.09-.11,1.87-1.66,2.06-1.81,2.38-1.82,4.65-2.31,7.58-2.6l.07.79c-5.02,1.03,1.53,5.48,2.04,7.65.05.27.08.53.13.8.17.04.33.09.5.14l-.05-.32c.31-.43,2.06,1.82,2.32,2.05,6.22,5.64,15.11,10.45,22.28,14.97,3.92,2.47,8.05,4.65,11.71,7.46.6.46.99,1.11,1.68,1.49l.06.47c.2.04.39.1.58.18v-.08c1.73.55,2.49,2.16,3.59,3.14,2.86,2.56,5.82,4.89,8.91,7.16,2.62,1.92,5.1,3.96,8.19,5.05.77-1.05,1.86-1.96,1.67-3.41-.27-2.06-2.03-2.08-3.42-2.93-7.27-4.4-13.35-10.3-19.69-15.88-.01-.05-.02-.11-.03-.16-.15-.16-.29-.33-.42-.53-.12-.23-.27-.43-.41-.65l.13.88c-1.47-.93-2.86-2.22-4.3-3.18-9.59-6.35-22.15-12.19-30.38-20.03-.84-.8-3.48-3.27-3.5-4.3.06-.29.63-.19.88-.18,9.81.35,19.4-1.34,29.08-2.45.49-.06.99.18,1.37.14.78-.08,1.59-.57,2.46-.47l.08.52c.16-.05.33-.1.5-.14-.01-.06-.02-.11-.03-.17l33.41-4.11.07.49c.18-.05.36-.09.54-.12-.02-.14-.04-.28-.06-.43l33.38-1.87.18.2c.12-.03.23-.05.35-.07v-.03c.14-.23,3.13-.69,3.65-.73,2.09-.17,7.7.33,9.86.76.37.07.76-.01,1.13-.04l-1.62,2.67c-2.48,3.1-5.25,5.85-7.94,8.76-.27,1.76-1.73,3.23-3.56,3.4-.01,0-.02,0-.03,0l.06.35c-.02.42-4.6,5.07-5.31,5.83-6.73,7.1-13.91,13.03-20.71,19.74-.27,2.04-2.27,3.38-4.3,3.33.02.14.04.28.06.43-.49,2.11-5.59,3.6-5.52,5.62.04,1.24.85.68,1.31.88.2.09,1.23,1.08,1.62,1.16.1.02.2.02.29.03.96-.86,2.42-1.28,3.79-1.05-.01-.06-.02-.12-.04-.17,1.16-.26,1.56-1.38,2.32-2.1,3.09-2.93,6.39-5.57,9.58-8.37.63-.56,2.03-2.43,2.59-2.73.33-.17.75.09.77.06.04-.05-.06-.56.2-.93.97-1.39,4.51-4.51,5.95-6.02,2.31-2.43,5.15-6.01,7.56-8.08.26-.22.49-.42.86-.39l.08.6c.18-.07.37-.13.57-.18-.04-.36-.08-.72-.12-1.08.07-.37,1.26-1.67,1.62-2.07,4.54-5.19,9.77-9.88,13.98-15.06.94-1.15,3.93-5.06,3.68-6.47-.23-1.33-1.56-1.36-2.26-1.95Z"/>
      </svg>
      <span class="env-label">hand</span>
    </div>
    <!-- Envelope 2 → animation -->
    <div class="envelope env-2" onclick="window.open('animation/index.html','_blank')">
      <svg viewBox="0 0 160.68 89.8" fill="#C23F1C" class="env-svg">
        <path d="M140.76,3.8c-.49-.41-.55-.94-.9-1.28-1.42-1.38-2.73-.68-4.43-.82-2.7-.21-5.54-.73-8.1-.81-2.53-.08-5.03.22-7.54.13l-.12-1.03c-.27.05-.53.09-.8.13l.14,1.14-20.17.6c-.93.15-1.86.3-2.79.42-11.53,1.45-23.2,1.88-34.76,3.08-12.04,1.25-23.87,3.76-35.88,5.18-5.96.7-11.94,1.23-17.92,1.72-1.68,1.81-2.34,4.16-3.3,6.54-1.02-1.54-2.42-1.85-3.75-.46.07,1.93-.39,3.84-.42,5.77-.09,5.17,1.23,10.26,2.17,15.26,2.24,11.92,4.38,23.85,7.14,35.65.26.5,2.27,3.5,2.49,3.52,1.93.14,1.72-3.03,1.92-4,.07-.34.48-.6.58-.98,1.1-4.23-1.49-2.63-1.78-2.83-.05-.04-.31-.98-.35-1.19-1.52-7.29-2.77-14.63-4.17-21.94-1.62-8.42-4.3-16.96-3.67-25.62,2.15.6,2.09-1.18,2.81-2.05.09-.11,1.87-1.66,2.06-1.81,2.38-1.82,4.65-2.31,7.58-2.6l.07.79c-5.02,1.03,1.53,5.48,2.04,7.65.05.27.08.53.13.8.17.04.33.09.5.14l-.05-.32c.31-.43,2.06,1.82,2.32,2.05,6.22,5.64,15.11,10.45,22.28,14.97,3.92,2.47,8.05,4.65,11.71,7.46.6.46.99,1.11,1.68,1.49l.06.47c.2.04.39.1.58.18v-.08c1.73.55,2.49,2.16,3.59,3.14,2.86,2.56,5.82,4.89,8.91,7.16,2.62,1.92,5.1,3.96,8.19,5.05.77-1.05,1.86-1.96,1.67-3.41-.27-2.06-2.03-2.08-3.42-2.93-7.27-4.4-13.35-10.3-19.69-15.88-.01-.05-.02-.11-.03-.16-.15-.16-.29-.33-.42-.53-.12-.23-.27-.43-.41-.65l.13.88c-1.47-.93-2.86-2.22-4.3-3.18-9.59-6.35-22.15-12.19-30.38-20.03-.84-.8-3.48-3.27-3.5-4.3.06-.29.63-.19.88-.18,9.81.35,19.4-1.34,29.08-2.45.49-.06.99.18,1.37.14.78-.08,1.59-.57,2.46-.47l.08.52c.16-.05.33-.1.5-.14-.01-.06-.02-.11-.03-.17l33.41-4.11.07.49c.18-.05.36-.09.54-.12-.02-.14-.04-.28-.06-.43l33.38-1.87.18.2c.12-.03.23-.05.35-.07v-.03c.14-.23,3.13-.69,3.65-.73,2.09-.17,7.7.33,9.86.76.37.07.76-.01,1.13-.04l-1.62,2.67c-2.48,3.1-5.25,5.85-7.94,8.76-.27,1.76-1.73,3.23-3.56,3.4-.01,0-.02,0-.03,0l.06.35c-.02.42-4.6,5.07-5.31,5.83-6.73,7.1-13.91,13.03-20.71,19.74-.27,2.04-2.27,3.38-4.3,3.33.02.14.04.28.06.43-.49,2.11-5.59,3.6-5.52,5.62.04,1.24.85.68,1.31.88.2.09,1.23,1.08,1.62,1.16.1.02.2.02.29.03.96-.86,2.42-1.28,3.79-1.05-.01-.06-.02-.12-.04-.17,1.16-.26,1.56-1.38,2.32-2.1,3.09-2.93,6.39-5.57,9.58-8.37.63-.56,2.03-2.43,2.59-2.73.33-.17.75.09.77.06.04-.05-.06-.56.2-.93.97-1.39,4.51-4.51,5.95-6.02,2.31-2.43,5.15-6.01,7.56-8.08.26-.22.49-.42.86-.39l.08.6c.18-.07.37-.13.57-.18-.04-.36-.08-.72-.12-1.08.07-.37,1.26-1.67,1.62-2.07,4.54-5.19,9.77-9.88,13.98-15.06.94-1.15,3.93-5.06,3.68-6.47-.23-1.33-1.56-1.36-2.26-1.95Z"/>
      </svg>
      <span class="env-label">animation</span>
    </div>
    <!-- Envelope 3 → pattern ex -->
    <div class="envelope env-3" onclick="window.open('pattern%20ex/index.html','_blank')">
      <svg viewBox="0 0 160.68 89.8" fill="#C23F1C" class="env-svg">
        <path d="M140.76,3.8c-.49-.41-.55-.94-.9-1.28-1.42-1.38-2.73-.68-4.43-.82-2.7-.21-5.54-.73-8.1-.81-2.53-.08-5.03.22-7.54.13l-.12-1.03c-.27.05-.53.09-.8.13l.14,1.14-20.17.6c-.93.15-1.86.3-2.79.42-11.53,1.45-23.2,1.88-34.76,3.08-12.04,1.25-23.87,3.76-35.88,5.18-5.96.7-11.94,1.23-17.92,1.72-1.68,1.81-2.34,4.16-3.3,6.54-1.02-1.54-2.42-1.85-3.75-.46.07,1.93-.39,3.84-.42,5.77-.09,5.17,1.23,10.26,2.17,15.26,2.24,11.92,4.38,23.85,7.14,35.65.26.5,2.27,3.5,2.49,3.52,1.93.14,1.72-3.03,1.92-4,.07-.34.48-.6.58-.98,1.1-4.23-1.49-2.63-1.78-2.83-.05-.04-.31-.98-.35-1.19-1.52-7.29-2.77-14.63-4.17-21.94-1.62-8.42-4.3-16.96-3.67-25.62,2.15.6,2.09-1.18,2.81-2.05.09-.11,1.87-1.66,2.06-1.81,2.38-1.82,4.65-2.31,7.58-2.6l.07.79c-5.02,1.03,1.53,5.48,2.04,7.65.05.27.08.53.13.8.17.04.33.09.5.14l-.05-.32c.31-.43,2.06,1.82,2.32,2.05,6.22,5.64,15.11,10.45,22.28,14.97,3.92,2.47,8.05,4.65,11.71,7.46.6.46.99,1.11,1.68,1.49l.06.47c.2.04.39.1.58.18v-.08c1.73.55,2.49,2.16,3.59,3.14,2.86,2.56,5.82,4.89,8.91,7.16,2.62,1.92,5.1,3.96,8.19,5.05.77-1.05,1.86-1.96,1.67-3.41-.27-2.06-2.03-2.08-3.42-2.93-7.27-4.4-13.35-10.3-19.69-15.88-.01-.05-.02-.11-.03-.16-.15-.16-.29-.33-.42-.53-.12-.23-.27-.43-.41-.65l.13.88c-1.47-.93-2.86-2.22-4.3-3.18-9.59-6.35-22.15-12.19-30.38-20.03-.84-.8-3.48-3.27-3.5-4.3.06-.29.63-.19.88-.18,9.81.35,19.4-1.34,29.08-2.45.49-.06.99.18,1.37.14.78-.08,1.59-.57,2.46-.47l.08.52c.16-.05.33-.1.5-.14-.01-.06-.02-.11-.03-.17l33.41-4.11.07.49c.18-.05.36-.09.54-.12-.02-.14-.04-.28-.06-.43l33.38-1.87.18.2c.12-.03.23-.05.35-.07v-.03c.14-.23,3.13-.69,3.65-.73,2.09-.17,7.7.33,9.86.76.37.07.76-.01,1.13-.04l-1.62,2.67c-2.48,3.1-5.25,5.85-7.94,8.76-.27,1.76-1.73,3.23-3.56,3.4-.01,0-.02,0-.03,0l.06.35c-.02.42-4.6,5.07-5.31,5.83-6.73,7.1-13.91,13.03-20.71,19.74-.27,2.04-2.27,3.38-4.3,3.33.02.14.04.28.06.43-.49,2.11-5.59,3.6-5.52,5.62.04,1.24.85.68,1.31.88.2.09,1.23,1.08,1.62,1.16.1.02.2.02.29.03.96-.86,2.42-1.28,3.79-1.05-.01-.06-.02-.12-.04-.17,1.16-.26,1.56-1.38,2.32-2.1,3.09-2.93,6.39-5.57,9.58-8.37.63-.56,2.03-2.43,2.59-2.73.33-.17.75.09.77.06.04-.05-.06-.56.2-.93.97-1.39,4.51-4.51,5.95-6.02,2.31-2.43,5.15-6.01,7.56-8.08.26-.22.49-.42.86-.39l.08.6c.18-.07.37-.13.57-.18-.04-.36-.08-.72-.12-1.08.07-.37,1.26-1.67,1.62-2.07,4.54-5.19,9.77-9.88,13.98-15.06.94-1.15,3.93-5.06,3.68-6.47-.23-1.33-1.56-1.36-2.26-1.95Z"/>
      </svg>
      <span class="env-label">pattern ex</span>
    </div>
    <!-- Envelope 4 → saudade -->
    <div class="envelope env-4" onclick="window.open('saudade/index.html','_blank')">
      <svg viewBox="0 0 160.68 89.8" fill="#C23F1C" class="env-svg">
        <path d="M140.76,3.8c-.49-.41-.55-.94-.9-1.28-1.42-1.38-2.73-.68-4.43-.82-2.7-.21-5.54-.73-8.1-.81-2.53-.08-5.03.22-7.54.13l-.12-1.03c-.27.05-.53.09-.8.13l.14,1.14-20.17.6c-.93.15-1.86.3-2.79.42-11.53,1.45-23.2,1.88-34.76,3.08-12.04,1.25-23.87,3.76-35.88,5.18-5.96.7-11.94,1.23-17.92,1.72-1.68,1.81-2.34,4.16-3.3,6.54-1.02-1.54-2.42-1.85-3.75-.46.07,1.93-.39,3.84-.42,5.77-.09,5.17,1.23,10.26,2.17,15.26,2.24,11.92,4.38,23.85,7.14,35.65.26.5,2.27,3.5,2.49,3.52,1.93.14,1.72-3.03,1.92-4,.07-.34.48-.6.58-.98,1.1-4.23-1.49-2.63-1.78-2.83-.05-.04-.31-.98-.35-1.19-1.52-7.29-2.77-14.63-4.17-21.94-1.62-8.42-4.3-16.96-3.67-25.62,2.15.6,2.09-1.18,2.81-2.05.09-.11,1.87-1.66,2.06-1.81,2.38-1.82,4.65-2.31,7.58-2.6l.07.79c-5.02,1.03,1.53,5.48,2.04,7.65.05.27.08.53.13.8.17.04.33.09.5.14l-.05-.32c.31-.43,2.06,1.82,2.32,2.05,6.22,5.64,15.11,10.45,22.28,14.97,3.92,2.47,8.05,4.65,11.71,7.46.6.46.99,1.11,1.68,1.49l.06.47c.2.04.39.1.58.18v-.08c1.73.55,2.49,2.16,3.59,3.14,2.86,2.56,5.82,4.89,8.91,7.16,2.62,1.92,5.1,3.96,8.19,5.05.77-1.05,1.86-1.96,1.67-3.41-.27-2.06-2.03-2.08-3.42-2.93-7.27-4.4-13.35-10.3-19.69-15.88-.01-.05-.02-.11-.03-.16-.15-.16-.29-.33-.42-.53-.12-.23-.27-.43-.41-.65l.13.88c-1.47-.93-2.86-2.22-4.3-3.18-9.59-6.35-22.15-12.19-30.38-20.03-.84-.8-3.48-3.27-3.5-4.3.06-.29.63-.19.88-.18,9.81.35,19.4-1.34,29.08-2.45.49-.06.99.18,1.37.14.78-.08,1.59-.57,2.46-.47l.08.52c.16-.05.33-.1.5-.14-.01-.06-.02-.11-.03-.17l33.41-4.11.07.49c.18-.05.36-.09.54-.12-.02-.14-.04-.28-.06-.43l33.38-1.87.18.2c.12-.03.23-.05.35-.07v-.03c.14-.23,3.13-.69,3.65-.73,2.09-.17,7.7.33,9.86.76.37.07.76-.01,1.13-.04l-1.62,2.67c-2.48,3.1-5.25,5.85-7.94,8.76-.27,1.76-1.73,3.23-3.56,3.4-.01,0-.02,0-.03,0l.06.35c-.02.42-4.6,5.07-5.31,5.83-6.73,7.1-13.91,13.03-20.71,19.74-.27,2.04-2.27,3.38-4.3,3.33.02.14.04.28.06.43-.49,2.11-5.59,3.6-5.52,5.62.04,1.24.85.68,1.31.88.2.09,1.23,1.08,1.62,1.16.1.02.2.02.29.03.96-.86,2.42-1.28,3.79-1.05-.01-.06-.02-.12-.04-.17,1.16-.26,1.56-1.38,2.32-2.1,3.09-2.93,6.39-5.57,9.58-8.37.63-.56,2.03-2.43,2.59-2.73.33-.17.75.09.77.06.04-.05-.06-.56.2-.93.97-1.39,4.51-4.51,5.95-6.02,2.31-2.43,5.15-6.01,7.56-8.08.26-.22.49-.42.86-.39l.08.6c.18-.07.37-.13.57-.18-.04-.36-.08-.72-.12-1.08.07-.37,1.26-1.67,1.62-2.07,4.54-5.19,9.77-9.88,13.98-15.06.94-1.15,3.93-5.06,3.68-6.47-.23-1.33-1.56-1.36-2.26-1.95Z"/>
      </svg>
      <span class="env-label">saudade</span>
    </div>
```

- [ ] **Step 2: Add envelope styles**

```css
    .envelope {
      position: absolute; width: 120px; cursor: pointer;
      transition: transform 0.3s cubic-bezier(0.34,1.56,0.64,1), opacity 0.3s;
      z-index: 3;
    }
    .envelope:hover { transform: scale(1.08); }
    .envelope:active { transform: scale(0.95); }
    .env-svg { width: 100%; height: auto; display: block; }
    .env-label {
      display: block; text-align: center; margin-top: 4px;
      font-family: 'Caveat', cursive; font-size: 16px; color: #312783;
    }
    /* Position envelopes scattered around the title */
    .env-1 { top: 15%; right: 15%; width: 100px; }
    .env-2 { top: 50%; right: 8%; width: 130px; }
    .env-3 { bottom: 20%; right: 25%; width: 110px; }
    .env-4 { top: 30%; right: 35%; width: 90px; }
```

- [ ] **Step 3: Add doodle decorative elements**

Inside `#hero`, after the envelopes, add hand-drawn accents:

```html
    <!-- Decorative doodles -->
    <svg class="doodle doodle-1" viewBox="0 0 100 30" fill="none" stroke="#312783" stroke-width="1.5">
      <path d="M5,15 Q25,0 50,15 T95,15" stroke-linecap="round"/>
    </svg>
    <svg class="doodle doodle-2" viewBox="0 0 60 60" fill="none" stroke="#C23F1C" stroke-width="1.5">
      <path d="M30,5 L30,55 M5,30 L55,30" stroke-linecap="round"/>
      <circle cx="30" cy="30" r="20" stroke-dasharray="4 4"/>
    </svg>
    <div class="handnote note-1">✦ click to explore ✦</div>
```

Add doodle styles:

```css
    .doodle { position: absolute; z-index: 1; opacity: 0.5; pointer-events: none; }
    .doodle-1 { width: 120px; bottom: 30%; left: 10%; }
    .doodle-2 { width: 50px; top: 20%; left: 45%; }
    .handnote {
      position: absolute; font-family: 'Caveat', cursive; font-size: 18px;
      color: #C23F1C; opacity: 0.7; pointer-events: none;
    }
    .note-1 { bottom: 10%; right: 40%; transform: rotate(-5deg); }
```

- [ ] **Step 4: Verify**

Open the page. Expected: Hero with "MARIA'S SITE" on the left, 4 orange envelope SVGs scattered around, with doodle accents. Hover envelopes to see scale-up. Click any envelope — it opens the respective project in a new tab.

---

### Task 4: About & Contact Sections

**Files:**
- Modify: `index.html` (add sections after `#hero`)

- [ ] **Step 1: Add About and Contact section HTML**

After `</section>` (closing hero), add:

```html
  <section id="about">
    <div class="section-content">
      <h2 class="section-title">About</h2>
      <p class="section-text">
        Website for WS Interaction Design &amp; AI<br/>
        with professor Rocco Modugno<br/>
        at Abadir
      </p>
      <svg class="doodle-divider" viewBox="0 0 200 20" fill="none" stroke="#C23F1C" stroke-width="1.5">
        <path d="M10,10 Q50,0 100,10 T190,10" stroke-linecap="round"/>
      </svg>
    </div>
  </section>

  <section id="contact">
    <div class="section-content">
      <h2 class="section-title">Contact</h2>
      <p class="section-text">
        <a href="mailto:m.teixeira@abadir.net" class="contact-link">m.teixeira@abadir.net</a>
      </p>
    </div>
  </section>
```

- [ ] **Step 2: Add About and Contact styles**

```css
    #about { background: #fff; }
    #contact { background: #EDE8E4; }
    .section-content {
      max-width: 720px; margin: 0 auto; padding-top: 10vh;
    }
    .section-title {
      font-family: 'Helvetica Neue', 'Arial Black', sans-serif;
      font-weight: 900; font-size: clamp(2rem, 5vw, 4rem);
      color: #312783; margin-bottom: 32px;
      text-transform: uppercase;
    }
    .section-text {
      font-size: clamp(1.1rem, 2vw, 1.5rem);
      line-height: 1.7; color: #312783;
    }
    .contact-link {
      color: #C23F1C; text-decoration: none;
      border-bottom: 2px solid transparent;
      transition: border-color 0.3s;
    }
    .contact-link:hover { border-color: #C23F1C; }
    .doodle-divider { width: 200px; margin-top: 48px; }
```

- [ ] **Step 3: Verify**

Scroll down. Expected: About section (white bg) with section title and text. Contact section (off-white bg) with email link in orange. Doodle divider line below About text.

---

### Task 5: Responsive Design

**Files:**
- Modify: `index.html` (add media queries in `<style>`)

- [ ] **Step 1: Add responsive styles**

```css
    /* Tablet */
    @media (max-width: 900px) {
      section { padding: 72px 32px 32px; }
      .hero-title { margin-left: 5vw; }
      .env-1 { top: 12%; right: 10%; width: 80px; }
      .env-2 { top: 45%; right: 5%; width: 100px; }
      .env-3 { bottom: 25%; right: 15%; width: 90px; }
      .env-4 { top: 28%; right: 25%; width: 75px; }
    }
    /* Mobile */
    @media (max-width: 600px) {
      nav { padding: 0 16px; gap: 16px; }
      nav a { font-size: 12px; }
      section { padding: 64px 20px 32px; }
      .hero-title { margin-left: 4vw; }
      .envelope { width: 70px !important; }
      .env-1 { top: 10%; right: 5%; }
      .env-2 { top: 40%; right: 3%; }
      .env-3 { bottom: 30%; right: 10%; }
      .env-4 { top: 22%; right: 15%; }
      .env-label { font-size: 12px; }
      .doodle-1 { width: 80px; }
      .handnote { font-size: 14px; }
      .section-content { padding-top: 5vh; }
    }
```

- [ ] **Step 2: Verify responsiveness**

Resize browser. Expected: Layout adapts — envelopes shrink and reposition, nav links stay visible, text sizes adjust.

---

### Task 6: Final Polish & Commit

**Files:**
- Modify: `index.html` (final review)

- [ ] **Step 1: Review the complete file for consistency**

Check that:
- Nav buttons scroll to correct sections (Projects → `#hero`, About → `#about`, Contact → `#contact`)
- All 4 envelope paths point to correct project folders
- Envelope hover/click interactions work smoothly
- Typography uses correct font stack
- Colors match spec (`#312783`, `#C23F1C`, `#EDE8E4`)
- No broken links or paths

- [ ] **Step 2: Open in browser and test all interactions**

```powershell
Start-Process "C:\Users\cm21m\OneDrive\Ambiente de Trabalho\ERASMUS\ABADIR\Abadir Workshop may\mysite\index.html"
```

Expected: Smooth experience across all sections and interactions.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add maria's site portfolio with envelope project cards"
```
