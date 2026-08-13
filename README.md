# Image Archive — Advanced Image Gallery

A single-file, self-contained image gallery built with vanilla **HTML, CSS, and JavaScript** — no frameworks, no build step, no dependencies. Open `index.html` in any modern browser and it runs.

## Description

Image Archive is a responsive gallery interface styled around a warm, hand-drawn "cream paper" aesthetic — a serif heading, gold hatch-textured background, and navy filter pills. Beyond the core gallery requirements (grid layout, lightbox navigation, hover states, responsive design, and category filters), it goes further with a set of interaction details that most basic galleries skip entirely:

- A **live generative ink background** — not a static image, but a real-time Canvas2D flow-field simulation that continuously draws soft gold/olive strokes, and visibly reacts as the cursor moves through it.
- A **magnetic custom cursor** that trails the pointer with easing and grows when hovering interactive elements.
- A **command palette** (press `/` or click "search") for instantly searching and jumping to any image by title or category, fully keyboard-navigable.
- **Per-photo color intelligence** — each card samples its own thumbnail's dominant color on load and tints its own hover glow to match, so no two cards look the same.
- A **native View Transitions morph** — clicking a photo animates it directly from its grid position into the lightbox using the browser's `startViewTransition` API, with a clean fade fallback in unsupported browsers.
- A **zoom & pan lightbox** — scroll or pinch to zoom up to 4×, drag to pan, double-click/double-tap to snap-zoom, with swipe-to-navigate when not zoomed.
- A **shakeable background** — click the "shake background" button, press `S`, or physically shake your phone (via the device motion API) to jolt the ink layer.
- A **live HUD** in the corner showing the clock, cursor coordinates, scroll position, and real-time FPS.

## Features Checklist

| Requirement | Implementation |
|---|---|
| HTML/CSS layout | Responsive CSS Grid (`auto-fill`, `minmax`) |
| JS navigation (next/prev, lightbox) | Full lightbox with prev/next buttons, keyboard arrows, and swipe |
| Hover effects & transitions | 3D tilt-on-hover, image zoom, shine overlay, caption reveal |
| Responsive design | Fluid grid + mobile breakpoints for HUD/controls |
| Bonus: categories/filters | Filter chips (All / Tech / Urban / Nature / Abstract) with live counts |

## File Structure

```
image-gallery/
├── index.html   — everything: markup, styles, and script in one file
└── README.md    — this file
```

## How to Run

No installation needed.

1. Download `index.html`.
2. Double-click it (or drag it into a browser window).
3. That's it — the gallery loads immediately.

To serve it locally instead (recommended if you plan to swap in your own images, since some browsers restrict certain APIs on `file://`):

```bash
# from the folder containing index.html
python3 -m http.server 8000
# then open http://localhost:8000
```

## Customizing

All image data lives in a single `DATA` array near the top of the `<script>` block:

```js
const DATA = [
  { seed:'trace-01', cat:'tech', title:'Bare PCB, top layer' },
  // ...
];
```

- **Swap in your own images:** replace the `imgUrl()` helper (currently pulling placeholders from `picsum.photos`) to point at your own image URLs, or change each item's `seed` field.
- **Add/remove categories:** just use new `cat` values in `DATA` — the filter chips and counts are generated automatically.
- **Add more images:** append more objects to the `DATA` array; the grid, lightbox, and command palette all pick them up with no other changes needed.

## Browser Support

Built entirely on standard web APIs. Core gallery functionality (grid, lightbox, filters, tilt) works in all modern browsers. Two features degrade gracefully where unsupported:

- **View Transitions morph** — Chromium-based browsers currently support `document.startViewTransition`; other browsers get a smooth opacity fade instead.
- **Device-shake** — requires a phone/tablet with motion sensors; on desktop, the on-screen "shake background" button and the `S` key work everywhere.

All animations respect `prefers-reduced-motion`.

## Tech Notes

- Zero dependencies, zero build tooling — just three native web technologies in one file.
- The background is a hand-rolled Canvas2D "flow field": a set of particles whose direction is driven by a cheap sum-of-sines pseudo-noise function, redrawn every frame with a fading trail for the ink effect.
- Per-card dominant-color extraction uses an offscreen `<canvas>` to sample each thumbnail's pixels (`crossorigin="anonymous"` is set so this works with the external placeholder images); if the browser blocks the read (CORS), it fails silently and the card keeps its default gold accent.
