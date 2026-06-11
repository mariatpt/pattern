# Chris Birthday Gift — Mini-Portfolio Design

## Overview
Single-page HTML/CSS/JS birthday mini-portfolio for Chris ("Priminho"), hosted as a standalone page in a new subfolder (`priminho/`) within the existing `mysite` repo. Purple + yellow palette, interactive balloons with confetti burst on click.

## Color Palette
- Purple: `#312783` (hero bg, message bg, text accents)
- Yellow: `#FFD700` (hero text, about bg, highlights)
- White: `#FFFFFF` (secondary text surfaces)
- Confetti: mix of purple, yellow, white, orange particles

## Typography
- Headings: Helvetica Neue Black / Arial Black (fallback)
- Body: Helvetica Neue Regular / Arial (fallback)

## Layout

### Hero Section
- Full viewport height, background `#312783`, text `#FFD700`
- Floating balloons (SVG/CSS-drawn, purple + yellow) with gentle bob animation
- Click balloon → pop animation + confetti particle burst (JS)

**Content:**
```
BUON COMPLEANNO, PRIMINHO!
Chris • 13 Giugno
Fondatore della miglior bevanda del mercato — Mbare
```

### About Section
- Full/section height, background `#FFD700`, text `#312783`
- Bio text:
  - "Chris è mega affettuoso, molto divertente e la persona più chill che conosca."
  - French fries icon/pictogram + "Ama le patatine fritte"
  - "Fondatore di Mbare"

### Message Section
- Background `#312783`, text `#FFD700` or `#FFFFFF`
- Maria's personal message (in English):
  "Happy birthday, Priminho! I really like you. Thank you for making my Erasmus so much more special with your calm, affectionate way of being, and with your carefree approach to life (and for me, who's always stressed, it's hilarious — never change). I carry you in my heart and hope to see you in Portugal or Germany ahaha"

### Footer
- Background `#312783`, minimal copyright/credit text

## Interactive Feature: Balloons + Confetti
- 6–8 balloons float in hero using CSS keyframe animation (bobbing)
- Each balloon has an inline SVG with string
- On click (`balloon.addEventListener('click', ...)`):
  1. Balloon pops (scale up + fade out, optional pop sound)
  2. 30–50 confetti particles burst from balloon position
  3. Confetti: small colored squares/dots with random velocity, rotation, gravity (CSS animation or JS canvas-free DOM particles)
  4. Particles fade after ~2s and are removed from DOM

## File Structure
```
mysite/
  priminho/
    index.html       (single page with embedded CSS + JS)
```

## Constraints
- Vanilla HTML/CSS/JS only (no libraries, no build step)
- Responsive at 900px and 600px breakpoints
- No external dependencies or fonts beyond system fonts
