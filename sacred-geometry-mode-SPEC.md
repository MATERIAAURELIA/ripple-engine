# Sacred Geometry Mandala — Mode 2 SPEC
*Captured Jun 8 (~11pm) from Marcus + Gemini brief. "Just gathering ideas now" — NEXT-session build,
not tonight (tonight = wind-down). A NEW mode alongside the Heart Portal (mandala-shader.html).*

## Concept
A dedicated, CRISP sacred-geometry mandala — interlocking lattices (Metatron's-Cube feel), precise
polygons, concentric geometric rings — vs. the Heart Portal's soft fluid fog. Different aesthetic,
same engine family.

## The math (GLSL)
1. **Polar coordinates** — `r = length(p)`, `theta = atan(p.y, p.x)`. Convert from X/Y.
2. **Rotational symmetry** — multiply theta by N (6/8/12), then `mod`/`abs` to mirror into pie
   slices. Trivial N-fold symmetry. `sin(theta * N)` → petals radiating from center.
3. **Iterative space folding (IFS)** — a `for` loop (3–5 iters) that on each pass: scales, shifts,
   and applies `abs()` to the coords. Simple shapes replicate + interlock → complex sacred-geometry
   lattices. (Origami folding of space — the Metatron's-Cube lattice trick.)
4. **Concentric geometric rings** — `sin(r * frequency - u_time)` → rings pulsing outward from
   center.
5. **Crisp lines via SDF + glow** — Signed Distance Fields: compute distance to a shape, draw a
   sharp stroke with `smoothstep`, add a slight neon/ethereal glow overlay. Crisp but luminous.

## Audio mapping (the "dances to the music" part)
- **Bass → number of symmetry slices** (e.g. 6-fold → 12-fold as music intensifies)
- **Treble → rotation speed** of the inner geometric rings
- (Reuse the existing audio engine: RMS/bass/treble + the gain/smoothing knobs.)

## How it fits
- **Mode 1 = Heart Portal** (soft, fluid, fBm domain-warp) — done, loved.
- **Mode 2 = Sacred Geometry** (crisp, IFS lattice, SDF lines) — this spec.
- A mode toggle to switch between them; shared controls (symmetry, hue, zoom, rotate, audio panel,
  hide-UI, transport). Same instrument, two faces.
- Connects to: the Structure "lines" end Marcus already loves; the chakra composition (crisp
  geometry per chakra); the kishimisu/IFS study thread.

## Cinematic upgrade brief (Gemini, Jun 8) — "high-end YouTube quality"
Move from flat 2D to luminous/3D/cinematic. Items:
- ✅ **Glow fields (SDF glow)** — DONE Jun 8: `opacity = k/(dist+eps)` reciprocal emission →
  neon light-filament look instead of flat lines.
- ✅ **Cinematic color grading** — DONE Jun 8: non-black dark-indigo background, gold center →
  magenta/violet outward, soft highlight rolloff (`col/(1+col*0.45)`).
- ⏳ **3D camera / raymarching** — fly THROUGH the geometry (deep tunnel/portal). Biggest climb:
  needs a raymarch loop or perspective setup. NEXT SESSION.
- ⏳ **Chromatic aberration + lens distortion** — split R/G/B by distance-from-center for cinematic
  lens bleed at edges. (Cheap-ish but needs care / slight extra cost.)
- ⏳ **Temporal motion blur** — accumulate % of previous frame (ping-pong framebuffers) → smooth
  light trails on rotation/pulse. Needs FBO setup. NEXT SESSION.
- Tip: IFS loop `i<5` → try 6–7 for more intricate sub-patterns (watch perf, don't exceed ~10).
- Tip: the glow factor `0.014`/`0.03` — raise to burn brighter.

## Status
Base mode + glow-fields + cinematic grading shipped (Jun 8, as calm wind-down polish). BIG climbs
deferred to next session: 3D raymarching camera, chromatic aberration, temporal motion blur.

## ⚠️ KEY LEARNING (Jun 8): the 2D approach DOESN'T reach Marcus's imagined look
Marcus: "not landing close to what I imagined." Honest read: the cinematic "carved out of light /
flying through space" feel he wants REQUIRES 3D DEPTH (raymarching) — a flat 2D geometry plane can't
fake dimensionality even with glow. The Heart Portal landed BECAUSE it had depth/flow; this flat
cousin proved it by contrast. → **Next session START with raymarching / pseudo-3D**, not more 2D
polish. The vision needs the third dimension, full stop.
