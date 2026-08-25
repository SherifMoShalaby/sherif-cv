# Handover

A personal CV site for **Sherif Shalaby**, Software Architect (Sofia, Bulgaria),
built as a set of air-traffic-control flight progress strips.

Each strip is an index card — organisation, role, period, domain — struck into
paper by a print head under a desk lamp. Opening a strip opens its logbook page,
which carries the full text for that role. The last strip hands over the watch.

Live: `index.html` — open it directly, no build step.

## How it works

Ten cards: the name card, seven roles, core skills, and the handover. One
gesture moves one strip: the outgoing card is pulled out of the light and the
next is fed in from the slot and struck. Scroll, arrow keys, or the bay marks
down the right edge. `Enter` or a click opens the log; `Escape` returns it.

## Constraints this is built under

- **One self-contained static file.** No build step, no package manager, no
  bundler. Three.js r149 is vendored in `vendor/`.
- **Nothing remote.** No CDN, no analytics, no web fonts from a third party, no
  photographs and no purchased models. Every surface in the scene is drawn in
  code or generated procedurally.
- **The CV is the content, in real markup.** `#cv` holds the whole document. With
  JavaScript off it *is* the site and reads as a plain CV. With JavaScript on it
  is visually hidden and becomes the single source both the strips and the
  logbook pages are built from, so the text can never be written twice or drift.
- **Works under a subpath.** Every path is relative, so it serves correctly from
  a GitHub Pages project URL.
- **`prefers-reduced-motion` is honoured.** The sequence skips to its finished
  state rather than animating.
- **The phone number on the CV is deliberately not in this repo.** Email and
  LinkedIn only.

## The fit contract

A strip is a fixed pre-printed form, so text that does not fit is a real failure,
not a styling preference. `drawFace()` clips each field to its box, and
`assertFits()` runs at boot: it measures every field string on every card against
its box and throws loudly on overflow rather than truncating in silence. It logs
`strip fit: 10 cards, all fields inside their boxes` when it passes.

Abbreviations on the strips are real names or contiguous parts of real names.
No coined codes.

## Verifying a change

The scene is measured, not eyeballed. `window.__framing()` projects the strip
through the camera and reports whether the watched station is inside the frame,
in normalised device coordinates. Geometry being in frame is not the same as
being visible — an object can pass the framing test while sitting entirely
outside the lamp pool, which happened twice during the build. Check luminance
separately, on the specific box that matters.

## Layout

    index.html    the site
    vendor/       three.min.js (r149) and four woff2 faces
