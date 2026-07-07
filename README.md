# SciGIF Studio

A single-file web app that creates **looping scientific schematic GIFs** — entirely in your
browser, no install, no upload, no dependencies.

## How to run

Just double-click `index.html` (it works from the file system), or serve it locally:

```
python -m http.server 8321 --directory C:/Users/MrBo/sci-gif-studio
# then open http://localhost:8321
```

## Templates

| Template | Shows |
|---|---|
| Flow reactor (packed bed) | Gas molecules flowing through a catalyst bed, changing color as they convert (defaults set up for CO₂ methanation: CO₂ + 4 H₂ → CH₄ + 2 H₂O over Ni/Al₂O₃) |
| Gas separation membrane (2D) | Feed flowing over a horizontal membrane: small molecules pass through the pores to the permeate below, larger ones are blocked and swept to the retentate (defaults set up for H₂/CO₂ separation through a graphene oxide membrane) |
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

## Adding your own template

Open `index.html` and add an entry to the `TEMPLATES` object with:
- `name`, `desc` — shown in the sidebar,
- `params` — an array of `{key, label, type: color|range|text|checkbox, def, min, max, step}`,
- `draw(ctx, t, P, W, H)` — a canvas draw function where `t ∈ [0,1)` is the
  loop phase and `P` holds the current parameter values.

Use only **integer multiples** of `t` inside `sin/cos` (or phase-local motion
with fade-in/out) so the GIF loops without a visible seam.
