# Maria's Site — Portfolio Design

## Overview
Single-page vanilla HTML/CSS/JS portfolio gateway at the `mysite` root. Four envelope project cards link to existing subfolder projects. Editorial-minimal aesthetic with hand-drawn accents.

## Color Palette
- Deep blue: `#312783` (nav bar, hero text, accents)
- Vibrant orange: `#C23F1C` (envelopes, interactive elements)
- Off-white background: `#EDE8E4`
- White: `#FFFFFF` (secondary surfaces)

## Typography
- Main headings: Helvetica Neue Black / Arial Black (fallback)
- Body: Helvetica Neue Regular / Arial (fallback)
- Handwritten accents: Caveat (Google Fonts) for doodles, arrows, decorative notes

## Layout

### Nav Bar
- Fixed top bar, background `#312783`, height ~48px
- Three text buttons aligned right: "About" | "Contact" | "Projects"
- White text, hover changes opacity/color
- Smooth scroll to corresponding section on click

### Hero Section
- Full viewport height, background `#EDE8E4`
- "MARIA'S" / "SITE" stacked, left-aligned, massive font size, color `#312783`
- 4 envelope illustrations (SVG or CSS-drawn, color `#C23F1C`) scattered across the viewport, not overlapping the title text
- Subtle hand-drawn doodles (arrows, squiggles, thin lines) as decorative elements

### About Section
- Full viewport or content-sized section
- Background transitions from `#EDE8E4` to `#FFFFFF`
- Text: "Website for WS Interaction Design & AI with professor Rocco Modugno at Abadir"
- Editorial layout with generous whitespace

### Contact Section
- Clean layout with the email: m.teixeira@abadir.net
- Simple, minimal presentation (no form)

## Envelope Cards (Projects)
- 4 envelope illustrations, hand-drawn style matching the provided SVG aesthetic
- Each assigned to one project:
  1. hand/index.html
  2. animation/index.html
  3. pattern ex/index.html
  4. saudade/index.html
- **Hover**: slight scale up (1.05), color shift, maybe a flap-lift animation
- **Click**: scale pulse then open project in new tab (`target="_blank"`)
- Playful, tactile feel

## Interactions
- All buttons: hover color change, scale(1.05) on click
- Smooth scrolling between sections (CSS `scroll-behavior: smooth`)
- Smooth transitions everywhere (CSS `transition` on all interactive elements)
- Subtle hover effects on envelope cards

## Responsiveness
- Mobile: stack layout, smaller envelopes, hamburger menu if needed
- Tablet: adjusted grid, readable font sizes
- Desktop: full editorial layout as described

## Technical Approach
- Single `index.html` at root of `mysite/`
- All CSS in `<style>` block
- Minimal JS for scroll behavior, envelope click handling, nav interactions
- No build tools, no frameworks — lightweight vanilla approach
- Google Fonts (Caveat) loaded via `<link>`
