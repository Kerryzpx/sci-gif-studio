# SciGIF Studio

A single-file web app that creates **looping scientific schematic GIFs** — entirely in your
browser, no install, no upload, no dependencies.

## How to run

Just double-click `index.html` (it works from the file system), or serve it locally:

```
python -m http.server 8321 --directory .   # run from the repo folder
# then open http://localhost:8321
```

## Templates

| Template | Shows |
|---|---|
| Flow reactor (packed bed) | Gas molecules flowing through a catalyst bed, changing color as they convert (defaults set up for CO₂ methanation: CO₂ + 4 H₂ → CH₄ + 2 H₂O over Ni/Al₂O₃) |
| Gas separation membrane (2D) | Feed flowing over a horizontal membrane: choose any pair from H₂, He, H₂O, N₂, O₂, CO, CH₄, CO₂ and set the permeated fraction of each — permeating molecules pass through the pores, the rest are swept to the retentate (defaults set up for H₂/CO₂ separation through a graphene oxide membrane) |
| Crumpled GO membrane — cross-section (cGO) | Strain-crumpled GO laminate with a multidomain structure (Zhang et al., *Nat. Nanotechnol.* 2025): expanded interlayer pockets take gas up quickly, compact domains sieve it — gas A rides the interlayer channel and threads the sieve to the permeate, gas B is mostly swept to the retentate (defaults show H₂/CO₂) |
| Crumpled GO membrane — 3D view (cGO) | The same separation seen in 3D: a static crumpled graphene oxide flake with H₂ slipping through the wrinkled laminate into the permeate, while bulkier CO₂ is turned away at the surface and sweeps off to the sides. Only the gas moves. Raise “CO₂ break-through” above 0 for a finite rather than perfect selectivity |
| Pressure swing adsorption (PSA) | Twin-bed PSA cycle: one column adsorbs gas B at high pressure while gas A passes through as product, the other is blown down at low pressure to release gas B as off-gas — beds swap every half cycle, with pressure gauges and valve highlights (defaults show H₂ purification over a zeolite bed) |
| Catalyst surface reaction | Adsorption → surface reaction (color change) → desorption on an atomic surface |
| Particle diffusion | Brownian-style particle motion in a container |
| Reaction energy diagram | A marker crossing the activation barrier, with Ea and ΔE annotations |
| Traveling wave | Propagating sine wave with oscillating medium particles |
| Catalytic / process cycle | Four-step cycle diagram with a marker traveling around it |

## Usage

1. Pick a template and tweak its colors, labels, and parameters in the sidebar.
2. Set canvas size, optional title, duration, and frame rate.
3. Click **Export GIF** — the file downloads immediately and loops forever.
4. **Save current frame as PNG** grabs a still for figures.

All animations are built from integer-frequency motion, so every exported GIF
loops seamlessly. Exports use a built-in GIF89a encoder (LZW compression,
per-frame palettes), so the app works fully offline.

Tips:
- PowerPoint, Google Slides, Twitter/X, and most journals' supplementary
  material accept GIFs directly.
- Keep durations ≤ 5 s and the canvas ≤ 800 px wide for small file sizes.
- Lower the frame rate to ~12 fps to cut file size roughly in half.
- What really drives file size is how much of the frame *moves*. Pixels that are
  identical to the previous frame are written as transparent and cost almost
  nothing, so a static background is close to free and fewer molecules means a
  smaller file. Measured on the cGO template at 700 × 620, 3 s, 20 fps: 0.71 MB
  with its default 18 H₂ + 9 CO₂, 0.44 MB at 12 fps. At 4 s / 30 fps but only
  6 H₂ + 3 CO₂ it is 0.58 MB — twice the frames, still smaller, because far less
  of each frame changes.

## Adding your own template

Open `index.html` and add an entry to the `TEMPLATES` object with:
- `name`, `desc` — shown in the sidebar,
- `params` — an array of `{key, label, type: color|range|text|checkbox, def, min, max, step}`,
- `draw(ctx, t, P, W, H)` — a canvas draw function where `t ∈ [0,1)` is the
  loop phase and `P` holds the current parameter values,
- `canvas: {w, h}` — optional; the canvas size to switch to when the template is
  selected, for templates that need a shape other than the 800 × 450 default.

The exporter builds one palette for the whole animation and stores each frame as
a difference from the one before. Two consequences worth designing around: parts
of the scene that never move are nearly free, and a `draw` that jitters every
pixel slightly on every frame will blow up the file for no visible gain.

Use only **integer multiples** of `t` inside `sin/cos` (or phase-local motion
with fade-in/out) so the GIF loops without a visible seam.
