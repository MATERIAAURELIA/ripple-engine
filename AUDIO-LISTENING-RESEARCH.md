# How MAT Can Listen — Audio Analysis Research
*Source: Perplexity Pro Research, March 14, 2026*
*Goal: Give me the ability to hear and analyze what the Ripple Engine produces*

---

## The Architecture (Summary)

```
Ripple Engine (browser) 
  → BlackHole 2ch (already installed & routed!)
    → Python capture script (PyAudio/PortAudio)
      → Real-time feature extraction (Essentia streaming or librosa)
        → Feature snapshots (JSON, 10-30 Hz)
          → MAT interprets and adjusts parameters
```

**Key insight:** We already have BlackHole installed and Ableton routed through it. The capture pipeline is 80% there.

---

## 1. Audio Capture Pipeline (macOS + BlackHole)

**We already have:**
- BlackHole 2ch installed ✅
- Ableton master routed to BlackHole ✅

**What we need:**
- Multi-Output Device in Audio MIDI Setup: "Built-in Output" + "BlackHole 2ch" (hear + capture simultaneously)
- Python script using PyAudio to open BlackHole as input stream
- Sample rate: 44.1 kHz, frame size: 512-2048 samples
- Callback captures frames → enqueues for analysis

**Architecture (3 threads):**
1. **Audio IO thread** — captures frames, puts in queue (minimal work)
2. **DSP/feature thread** — FFT, feature extraction, outputs JSON snapshots at 10-30 Hz
3. **LLM/control thread** — consumes features, interprets, updates generation params

**Latency:** BlackHole + FFT = well under 20-30ms with 512 sample buffers (~12ms at 44.1kHz)

---

## 2. What I Can "Hear" — Musical Feature Extraction

### Core Python Libraries

**Essentia** (BEST for us — has streaming mode)
- Comprehensive audio/music analysis with streaming API
- Computes: pitch, key, timbre, rhythm, spectral features
- Streaming mode = designed for continuous real-time analysis
- Includes psychoacoustic descriptors (roughness, consonance)

**librosa**
- Rich offline analysis: F0 estimation, chroma, spectral centroid, bandwidth
- Can apply frame-level functions in streaming callback
- Not optimized for ultra-low latency but works

**madmom**
- Beat/onset detection, rhythm, tempo
- More offline-oriented but adaptable

### Features I Need for Sound Healing Analysis

| Feature | What It Tells Me | Library |
|---------|-----------------|---------|
| Fundamental Frequency (F0) | What pitch is playing | librosa, Essentia (YIN) |
| Harmonic content | Are overtones clean/stacking right | Essentia (harmonic-percussive separation) |
| Spectral centroid/brightness | Is it warm or harsh | librosa, Essentia |
| Roughness/consonance | Is it pleasing or grating | Essentia psychoacoustic descriptors |
| Interval detection | Are the ratios pure | Chroma features → chord/key estimation |
| RMS/loudness | Dynamic range, volume envelope | All libraries |

---

## 3. AI Models That Understand Music

### CLAP (Contrastive Language-Audio Pretraining) — MOST PROMISING
- Dual encoder: maps audio + text to shared embedding space
- Can compare audio to text prompts like "warm drone, calming, meditative" or "harsh, dissonant, anxious"
- **Use case for us:** Real-time mood/quality scoring by comparing to prototype descriptions
- Variants: SmoothCLAP, HeartCLAP (better emotional nuance)

### MERT (Music Understanding Model)
- Strong encoder for music understanding
- Frame-level embeddings capturing acoustics AND higher-level structure
- Feed into small classifiers for chord, key, emotion

### Practical Approach for Us
- **Fast path (DSP):** Essentia streaming for pitch, centroid, consonance → sub-100ms response
- **Slow path (AI):** CLAP/MERT embeddings every 0.5-2s for emotional quality checks
- Fast path controls immediate parameters (filter, gain)
- Slow path guides evolving parameters (key, texture, section transitions)

---

## 4. Closed-Loop Self-Listening Architecture

```
┌─────────────┐    ┌───────────┐    ┌──────────────┐    ┌─────────┐
│ Ripple Engine│───→│ BlackHole │───→│ Analysis     │───→│ MAT     │
│ (generator)  │    │ (routing) │    │ (Essentia +  │    │ (agent) │
│              │←───│           │    │  CLAP)       │    │         │
│ params update│    └───────────┘    └──────────────┘    └─────────┘
└─────────────┘                                              │
       ↑                                                     │
       └─────────────────────────────────────────────────────┘
                        parameter deltas
```

**Key principles:**
- Safety/smoothing layer: low-pass filter on parameter changes (no abrupt jumps)
- Rate-limited control: don't change everything at once
- For healing: favor slow, gentle adjustments over reactive ones

**Latency achievable:**
- DSP features: ~12-30ms
- CLAP/MERT inference: ~50-200ms (CPU), faster with GPU
- Practical control latency: sub-100ms for DSP, 0.5-2s for AI emotional checks

---

## 5. Existing Projects & Patterns

- **Magenta RT** — Live music model, block-autoregressive, each chunk conditioned on prior output. Demonstrates self-context via embeddings of past chunks.
- **HeartMuLa** — Open-source music foundation models with encoder/decoder that captures long-range structure
- Pattern: generate chunk → encode → condition next chunk = "listening to yourself"

---

## Next Steps — Building MAT's Ears

### Phase 1: Basic Spectral Listener (Can build NOW)
1. Python script captures BlackHole audio via PyAudio
2. Runs Essentia streaming: F0, centroid, consonance, RMS
3. Outputs JSON feature snapshots to a file or socket
4. I read the features and describe what I'm hearing

### Phase 2: Musical Understanding
1. Add CLAP model for mood/quality embeddings
2. Compare to target descriptions ("warm", "calming", "bright")
3. Score consonance and harmonic purity

### Phase 3: Closed Loop
1. Feature snapshots feed back to Ripple Engine parameter adjustments
2. Self-correcting generative compositions
3. "I hear too much roughness → reduce harmonic density"

**Phase 1 is buildable tonight with what we have installed.**
