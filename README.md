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
| Lamellar GO membrane — tortuous path | A GO laminate drawn as a brick wall of flakes of length L in offset rows, separated by interlayer galleries of height d, on a macroporous support. The molecules' route is generated from the drawn gaps rather than sketched alongside them: a permeating molecule runs along a gallery until the flake above it ends, drops through that slit and starts again one layer down, so its path is many times the film thickness — that long path is the low permeance, while the gallery height (keyed to gas A's size) is the sieve that keeps the larger gas out. The dashed guide is the real route of the first permeating molecule, not a decoration. Flow speed ¼× to 3× (default ⅓×) plus a separate in-membrane slowdown, and a size slider for every part — layers, flake length L, interlayer spacing d, flake thickness, membrane width, support, molecules and labels (defaults show H₂/CO₂; 860 × 540, ≈ 0.37 MB at 4 s / 20 fps) |
| Gas separation membrane (3D slab) | The generic membrane figure drawn in perspective: the feed enters from the left and fans out over the slab's top face, the permeating gas drops straight through into the permeate space below, and the rejected gas is swept across and off to the right as retentate, with a hollow ΔP arrow on the left for the driving force. Molecules follow the slab's own axes and shrink as they recede, and anything under the top face is hidden by the slab. Any gas pair, a permeated fraction for each, and a size slider for every part — slab width, depth and thickness, molecules, arrows, ΔP arrow and labels — so it can be re-proportioned for a slide (defaults show H₂ permeating and CO₂ retained; 880 × 460) |
| Polymeric membrane (solution–diffusion) | Dense polymer film with entangled chains and no size-sieving pores: both gases dissolve at the feed face, hop between free-volume elements and desorb into the permeate. Gas A diffuses faster, gas B is more soluble and lingers at half the rate, so both get through — high flux, low selectivity (the Robeson trade-off). Every route starts and ends off-canvas, so no molecule wraps inside the frame. Flow speed ¼× to 4× (default ½×; 3× is left out because it would put gas B at 1.5×, which does not tile the loop) and a molecule-size slider (defaults show H₂/CO₂; 800 × 500) |
| Crumpled GO membrane — cross-section (cGO) | Strain-crumpled GO laminate with a multidomain structure (Zhang et al., *Nat. Nanotechnol.* 2025): expanded interlayer pockets take gas up quickly, compact domains sieve it — gas A rides the interlayer channel and threads the sieve to the permeate, gas B is mostly swept to the retentate (defaults show H₂/CO₂) |
| Crumpled GO membrane — 3D view (cGO) | The same separation seen in 3D: a static crumpled graphene oxide flake with H₂ slipping through the wrinkled laminate into the permeate, while bulkier CO₂ is turned away at the surface and sweeps off to the sides. Only the gas moves. Raise “CO₂ break-through” above 0 for a finite rather than perfect selectivity |
| Cryogenic distillation (tray column) | A cryogenic sieve-tray column: the feed is cooled in a heat exchanger and expanded through a J–T valve into the middle of the column. The volatile gas rises through the trays as vapour to the condenser and leaves overhead; the heavy gas runs across each tray and down each downcomer as liquid into the reboiler sump. Includes reflux droplets, boiling bubbles, tray liquid tinted by composition, rectifying/stripping brackets and a temperature gradient labelled with the boiling points (defaults show H₂/CO₂ at ~30 bar, −50 °C top / −5 °C sump; choose N₂/O₂ for air separation; 720 × 800, ≈ 0.56 MB at 3 s / 20 fps) |
| Steam methane reforming (SMR) | Top-fired reformer with burner flames and Ni catalyst tubes (CH₄ + H₂O → CO + 3 H₂), a waste-heat boiler, then a water–gas shift bed (CO + H₂O → CO₂ + H₂). Each gas packet enters as CH₄ + 2 H₂O and leaves as CO₂ + 4 H₂; every molecule changes species in place, with a flash, as it crosses a reaction front (800 × 560, ≈ 0.6 MB) |
| Coal gasification (entrained flow) | Slagging entrained-flow gasifier: coal–water slurry and O₂ meet at the top burner, coal particles swirl down through the flame and shrink as they gasify to CO + H₂ (+ some CO₂), the ash melts to slag at the throat and freezes in the water quench; syngas leaves through the dip tube, quench and scrubber, then takes on steam and passes a water–gas shift bed where each CO becomes CO₂ with a new H₂ behind it, so the product is H₂ + CO₂. Refractory lining, reaction equations with ΔH° (760 × 680, ≈ 0.6 MB) |
| Pressure swing adsorption (PSA) | Twin-bed PSA cycle: one column adsorbs gas B at high pressure while gas A passes through as product, the other is blown down at low pressure to release gas B as off-gas — beds swap every half cycle, with pressure gauges and valve highlights. Flow speed runs from ¼× to 3× (default ⅓×); the adsorbed gas slows by taking longer over its single trip per half cycle, so it still reaches its bead before the swap (defaults show H₂ purification over a zeolite bed) |
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
loops seamlessly. Exports use a built-in GIF89a encoder (LZW compression, one global
palette for the whole animation), so the app works fully offline.

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
- `params` — an array of
  `{key, label, type: color|range|text|checkbox|select, def, min, max, step}`
  (a `select` takes `options: [[value, label], ...]` and hands `draw` the value as
  a **string**),
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

The SMR, gasifier, PSA, cryogenic distillation, 3D membrane slab and lamellar GO
templates have a size slider (in %) for every part — furnace/gasifier width and
height, tubes, pellets, flames, boiler, scrubber, shift reactor width and height,
columns, trays, adsorbent beads, flakes, interlayer spacing, molecules, coal
particles, pipes and labels.
They lay themselves out in design units from those sizes (see `fitLayout`), so
a part that grows pushes its neighbours aside, and the whole drawing shrinks
to fit the canvas if it no longer does. It never scales up past the default,
so shrinking one part leaves margin rather than inflating everything else.

For flow slower than one pass per loop, use `flowCount` / `flowPhase` (see the
SMR, gasifier, PSA, 3D membrane slab, polymer and lamellar GO templates): at
speed 1/m every mover gets m copies spaced 1/m apart in phase, which hand over to one another at the loop point, so ½×, ⅓× and
¼× still loop seamlessly.
