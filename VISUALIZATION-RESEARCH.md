# Visualization Research — Ripple Engine / Harmonic Bloom
*Source: Perplexity Deep Research, March 14, 2026*
*Query: Mathematical sound visualization for generative healing instrument*

---

## Core Principle
> "Treat your whole instrument as a single 'field' where ratios, phases, and growth laws all share the same underlying math; the beauty emerges when you keep those mappings honest and simple."

---

## 1. Lissajous Figures — Living Intervals

**What they are:** Two perpendicular sinusoidal motions: `x(t) = A sin(at + δ)`, `y(t) = B sin(bt)`

**Key insights:**
- Use the **frequency ratio a:b** directly from just-intonation intervals
- Closed curves occur when the ratio is rational
- Number of lobes = the integers in the ratio (e.g., 3:2 gives 3 lobes one way, 2 the other)
- **Purity mapping:** Small integer ratios (2:1, 3:2, 4:3) → more symmetric, clean strokes. Dense/mistuned ratios → "smear" or precess
- **Phase = psychoacoustic quality:** in-phase (δ≈0) for consonant coherent visuals; slow phase drift for evolving chords
- For chords: layer multiple Lissajous curves in polar space or extrude into 3D tubes (radius = amplitude) → "breathing geometry"

**Implementation (WebGL/Canvas):**
1. Derive a:b from just-intonation ratio per voice
2. Sample Web Audio API for instantaneous phase/amplitude envelope per voice
3. Animate A, B, δ slightly (not the base ratio)
4. Render as GPU line strips or SDF ribbons with blur/glow

---

## 2. Chladni Patterns & Cymatics — True Nodes

**What they are:** Sand/particles settle along nodal lines of a vibrating plate at resonance

**Key insights:**
- Each resonant frequency = distinct standing-wave mode
- Higher frequency → more nodes → more intricate patterns
- **Scientific honesty:** Don't fake shapes — either precompute real patterns or numerically approximate eigenmodes
- **Practical approach:** Library of real Chladni/cymatic glyphs keyed by frequency and "mode index," cross-fade in WebGL as sound approaches those regions

**Visual design:**
- Displacement maps on a water surface plane in shader
- Brightness zeroed at nodes, maximum at antinodes (preserves standing wave structure)
- Tie **intricacy to harmonic density:** simple modes for single tones, complex superpositions as partials stack

---

## 3. Fibonacci, Phyllotaxis & Golden Spirals

**The math:** Place each element at radius `r = c√n`, angle `θ = nα`, with α near **golden angle ~137.5°**
- Yields spiral counts that are consecutive Fibonacci numbers
- When α = 360°(2−φ), parastichy counts follow Fibonacci pairs (e.g., 34/55)

**Sound mapping:**
- Map **time to n:** each new beat or Fibonacci-timed subdivision spawns the next point on the spiral
- **Harmonic series → different rings:** partial k occupies a separate phyllotactic disc, ring radius ∝ log(frequency) — overtones "bloom" outward
- For real footage: track center, approximate local spiral parameters, overlay mathematically perfect phyllotaxis → "revealing the code" effect

---

## 4. Sacred Geometry & Harmonic Structure

**Flower of Life → Metatron's Cube → All 5 Platonic Solids**

**Scientifically honest strategies:**
- Treat as **spatial groupings of ratios** (symbolic lens), not literal physics
- Map scale degrees/chord tones to **vertices of Platonic solids** (fifths on opposite vertices, thirds on edges)
- Distances/angles between vertices = visual metaphors for interval size and quality
- **Flower of Life as oscillator lattice:** each circle = an oscillator, connections drawn stronger for simple ratios (3:2, 4:3, 5:4), fainter for complex ones
- Animate solids rotating and "snapping" as chords resolve to consonance

---

## 5. Real-Time Generative Web Techniques

**Core stack:**
- **Web Audio API** — FFT, bands, envelopes via AnalyserNode
- **WebGL / Three.js** — 3D and shader work
- **GLSL shaders** — phyllotaxis fields, Lissajous ribbons, cymatic surfaces

**Audio-reactive patterns:**
- Feed smoothed band energies + harmonic magnitudes into shader uniforms
- Modulate radius, thickness, color of geometric forms

**Multi-space layering (our architecture?):**
1. **Lissajous** in central, slow-moving 2D/3D core
2. **Phyllotaxis particle halos** for growth and breath
3. **Cymatic plane** in background, modulated by low frequencies

**Healing-specific:**
- Favor **slow, continuous morphs** tied to envelopes and Fibonacci timing
- NOT jumpy beat-synced flashes
- Use easing functions so the nervous system can entrain gently
- **Temporal coherence** is key for healing context

**Video production:** Run same engine headless or capture in-browser at high res with scripted parameter curves

---

## 6. References & Inspiration

- **Peter Frazer's interval figures** — Lissajous-based tuning visualizations showing ratio → curve mapping
- **Cymatics art & meditation videos** — water/sand on speakers forming live patterns
- **WebGL music visualizer tutorials** — Web Audio + Three.js/Shader Park pipelines
- **Wolfram phyllotaxis demos** — interactive angle/growth law experimentation
- **Daniel Shiffman (Coding Train)** — Lissajous Curve Table coding challenge

---

## Next Steps for Us

1. **Start with Lissajous** — most direct math-to-visual mapping, works with our just-intonation ratios
2. **Add phyllotaxis particle layer** — Fibonacci timing already in Ripple Engine, natural fit
3. **Cymatic texture layer** — precompute library of real patterns, cross-fade by frequency
4. **Sacred geometry as chord visualization** — Flower of Life lattice showing active harmonics
5. **Nature overlay prototype** — tree/fern footage with Fibonacci spiral overlay, synced to audio

**Mood direction to explore:** Botanical? Celestial? Crystalline? (Marcus to decide)
