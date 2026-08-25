# sherif-cv

Personal site for **Sherif Shalaby** — Software Architect & Staff Engineer, Sofia, Bulgaria.

Live: <https://sherifmoshalaby.github.io/sherif-cv/>

    index.html      the portfolio — hero, case studies, timeline, expertise, open source
    strip/          the interactive version: an ATC flight-strip bay in Three.js
    cv.pdf          generated from index.html, not maintained separately
    og.png          social card, generated
    vendor/         three.min.js (r149) and three woff2 faces

## Structure

The front page is the professional story: who, what, the four case studies, the
timeline, the domains, how I work, and what I build outside work. The flight-strip
concept the site started as lives on at `strip/` and is linked from the hero — the
interaction is secondary to the content, deliberately.

## Constraints

- **No build step, no package manager, no bundler.** Two static HTML files.
- **Nothing remote at runtime.** No CDN, no analytics, no web fonts from a third
  party, no stock photography. Every surface is drawn in code or generated.
- **Works with JavaScript off.** The portfolio's three scripts are a nav hairline,
  a section reveal and a scroll-spy. With JS off nothing is hidden and every link
  works. `strip/` degrades to its own semantic CV document.
- **Relative paths only**, so it serves correctly from a GitHub Pages project URL.
- **`prefers-reduced-motion` is honoured** in both pages.
- **The phone number on the CV is deliberately absent.** Email and LinkedIn only.
  It is checked for in the verification pass and in the generated PDF.

## cv.pdf is generated, not written

`cv.pdf` is printed from `index.html` under its `@media print` rules, so the CV and
the site cannot drift. The generator opens every `<details>` first — CSS cannot force
that reliably. The print rules drop what belongs to the site rather than the paper:
the strip, the chrome, the narrative sections and the "why this is interesting"
asides.

Regenerate by serving the directory and printing it to A4 with `printBackground`,
14mm margins, after setting `open` on every `<details>`.

## Themes

Light is the bare `:root`, so a browser with no preference gets a complete palette.
Dark is applied by `prefers-color-scheme` OR by the control in the nav, and the
control wins in both directions. A tiny inline script in `<head>` applies the stored
choice before first paint, otherwise a visitor who picked Light on a dark OS sees a
flash of the wrong palette while the page finishes parsing.

The control cycles Auto, Light, Dark and names the active state, so "Auto" is
selectable rather than an invisible default. Contrast is measured, not asserted:
every text token holds WCAG AA against its background in both modes (lowest
measured 5.47:1).

## Architecture diagrams

The case-study diagrams are markup, not SVG. Node labels are real selectable text,
so they theme with the page, scale with the type, read out to a screen reader in
order, and survive the print stylesheet. Connectors are a rule plus a chevron drawn
from two borders.

Watch out when editing: `.case-body li` styles list items in that section, and its
bullet `::before` and `margin-bottom` leak into the diagram nodes. `.flow li` resets
both. Without the reset the first node carries a stray tick and the last sits 4px
below the row.

## Content rule

Everything factual comes from the CV or from public repository metadata. Where a
detail was missing it was left out rather than invented — including project
descriptions for repositories that carry none.

## The strip page

`strip/` renders an air-traffic-control strip bay procedurally: ten cards, one per
role, struck into paper by a print head under a desk lamp. Text fit is an executable
contract — `drawFace()` clips each field to its box and `assertFits()` throws at boot
if any field string overflows, rather than truncating in silence. `window.__framing()`
projects the strip through the camera so composition is measured, not eyeballed.

Note for anyone editing it: geometry being *in frame* is not the same as being
*visible*. An object can pass the framing assertion while sitting entirely outside
the lamp pool — that happened twice during the build. Check luminance separately, on
the specific box that matters.
