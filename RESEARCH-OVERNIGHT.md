# Ripple Engine — Overnight Research (March 15, 2026)

Research done while Marcus sleeps. For tomorrow's build session.

---

## 1. Brainwave Entrainment — Three Methods for Speakers

### Isochronic Tones (BEST for speakers)
- A single tone **pulsed on and off** at the target brainwave frequency
- Works through speakers — no headphones required
- Produces **stronger neural responses** than binaural beats (Lake Forest College study)
- Implementation: `GainNode` amplitude modulation at target Hz
- Sharp, distinctive pulses — the brain locks onto the rhythm

**Web Audio implementation:**
```javascript
// Isochronic: pulse a tone's gain at target Hz (e.g., 6Hz for theta)
const osc = audioCtx.createOscillator(); // carrier tone (e.g., 200Hz)
const lfo = audioCtx.createOscillator(); // modulator at brainwave freq
const lfoGain = audioCtx.createGain();
lfo.frequency.value = 6; // 6Hz = theta
lfo.type = 'square'; // sharp on/off pulses
lfoGain.gain.value = 0.5;
lfo.connect(lfoGain);
lfoGain.connect(osc.frequency); // or connect to a gain node for AM
```

Better approach — amplitude modulation via gain:
```javascript
const carrier = audioCtx.createOscillator();
carrier.frequency.value = 200; // carrier tone
const pulseGain = audioCtx.createGain();
// Schedule gain ramps for clean isochronic pulses
const pulseRate = 6; // Hz (theta)
const interval = 1 / pulseRate;
for (let i = 0; i < 60 * pulseRate; i++) { // 60 seconds
  const t = audioCtx.currentTime + i * interval;
  pulseGain.gain.setValueAtTime(0, t);
  pulseGain.gain.linearRampToValueAtTime(0.3, t + interval * 0.1); // 10% attack
  pulseGain.gain.setValueAtTime(0.3, t + interval * 0.5); // 50% sustain
  pulseGain.gain.linearRampToValueAtTime(0, t + interval * 0.7); // fade out
}
```

### Monaural Beats (works through speakers)
- Two frequencies mixed BEFORE reaching the ear
- Physical acoustic interference happens in the air
- Example: 200Hz + 206Hz = 6Hz monaural beat
- **Stronger cortical response** than binaural (Engelbregt et al., 2019, PMC7204407)
- Can reduce anxiety (study confirmed)

**Implementation:** Simply play two oscillators at slightly different frequencies through the same output. The interference happens naturally.
```javascript
const osc1 = audioCtx.createOscillator();
const osc2 = audioCtx.createOscillator();
osc1.frequency.value = 200;
osc2.frequency.value = 206; // 6Hz difference = theta monaural beat
// Both go to same gain node → mono mix → speakers
```

### Binaural Beats (headphones only)
- Different frequency in each ear
- Brain perceives the difference
- Requires headphones — useless in sound bath setting
- Good for personal practice, not group sessions

### Summary for Ripple Engine
| Method | Speakers? | Headphones? | Strength | Best For |
|--------|-----------|-------------|----------|----------|
| Isochronic | ✅ Yes | ✅ Yes | Strongest | Group sound baths |
| Monaural | ✅ Yes | ✅ Yes | Strong | Subtle background entrainment |
| Binaural | ❌ No | ✅ Required | Moderate | Personal meditation tracks (IT) |

**Recommendation:** Build all three, let Marcus choose per context. Sound bath = isochronic + monaural. Insight Timer tracks = binaural option too.

---

## 2. Brainwave Frequency Targets

| Band | Range | State | Sound Healing Use |
|------|-------|-------|-------------------|
| **Delta** | 0.5–4 Hz | Deep sleep, unconscious healing | Deep sleep tracks, trauma release |
| **Theta** | 4–8 Hz | Meditation, creativity, intuition, memory | Sound baths, deep meditation, inner work |
| **Alpha** | 8–12 Hz | Relaxed awareness, calm focus | Beginning of sound bath, light meditation |
| **Beta** | 12–30 Hz | Active thinking, alertness | Wake-up sequences, energizing |
| **Gamma** | 30–100 Hz | Peak awareness, insight, compassion | Advanced meditation, peak states |

**Sweet spots for sound healing:**
- **6 Hz** — Deep theta, proven to entrain within 10 minutes (study)
- **7.83 Hz** — Schumann resonance (Earth's electromagnetic frequency) — already in Harmonic Journey!
- **10 Hz** — Alpha, relaxed awareness
- **4 Hz** — Theta/delta border, deep trance
- **40 Hz** — Gamma, associated with compassion meditation (Tibetan monks study)

---

## 3. Fibonacci in Music — Academic Backing

### Bartók's Music for Strings, Percussion, and Celesta (1936)
- **89 measures** total (Fibonacci number)
- Climax at end of **bar 55** (Fibonacci)
- Exposition lasts **21 bars** (Fibonacci)
- String mutes removed at measure **34** (Fibonacci)
- Subdivision: 34 and 55 approximate the golden ratio (0.618)
- Bartók also used **palindromic rhythms based on Fibonacci** (Lendvai's analysis)

### Golden Ratio in Musical Structure
- Dominant note = **5th** of major scale = **8th** note of all 13 chromatic notes (5, 8, 13 = Fibonacci)
- Octave = 13 chromatic steps, major scale = 8 notes, pentatonic = 5 notes
- Climax placement: most emotionally powerful point in a piece often falls at 61.8% (golden ratio)

### For the Ripple Engine
- Fibonacci timing creates rhythms that feel "natural" because they follow growth patterns found in nature
- The sequence 1,1,2,3,5,8,13 as beat subdivisions creates polyrhythms that resolve but never feel mechanical
- Golden ratio (1.618...) as a time multiplier creates gaps that are irrational — they never sync perfectly, creating perpetual forward motion
- **This is why Marcus's Ripple mode feels like handpan** — handpan tuning creates natural Fibonacci-adjacent overtone relationships

---

## 4. Sound Healing — What the Science Actually Says

### Systematic Reviews (2024-2025)
- **PMC12063014** (2025): Systematic review of 19 clinical studies on singing bowls
  - 9 RCTs, 7 case series, 2 randomized crossover
  - Evidence for: anxiety, depression, pain, sleep, Parkinson's, autism spectrum
  - Mainly mental health outcomes
  
- **PMC5871151** (2017, Goldsby et al.): Sound meditation with bowls/gongs
  - Significant reduction in tension, anger, fatigue, depressed mood (p < 0.001)
  - Spiritual well-being significantly higher post-session
  - "High-intensity, low-frequency combination" key factor

- **MDPI Medicina** (2022): Neurophysiological effects
  - Singing bowl massage showed measurable nervous system changes

### What's Actually Happening (Mechanistically)
1. **Vagal nerve stimulation** — low frequencies physically vibrate the body, stimulating parasympathetic nervous system
2. **Brainwave entrainment** — rhythmic pulses entrain neural oscillations
3. **Acoustic beating** — bowls produce natural monaural beats from multiple overtones
4. **Attention capture** — rich harmonic content holds attention, reducing rumination
5. **Vibroacoustic therapy** — physical vibration through the body (not just heard)

### The Honest Take
- Singing bowls work. The evidence is growing.
- The MECHANISM is debated — is it the frequencies specifically, or the meditative context?
- Solfeggio frequencies (528Hz etc.) have very thin evidence for specific frequency effects
- But entrainment, relaxation response, and vagal stimulation have solid evidence
- **The Ripple Engine can test this** — control specific variables, measure responses

---

## 5. Implementation Plan for Tomorrow

### Priority 1: Isochronic Tone Layer
- Add entrainment section to strip: frequency target (Delta/Theta/Alpha/Beta/Gamma presets)
- Carrier tone slider (100-500Hz)
- Pulse depth (how hard the on/off cuts)
- Runs continuously alongside played notes

### Priority 2: Monaural Beat Layer  
- Two sub-oscillators with adjustable frequency offset
- Offset = brainwave target (e.g., 6Hz)
- Carrier frequency tied to current root note or independent

### Priority 3: Fibonacci Stagger for Harmonics
- Apply Fibonacci/Golden/Prime sequences to the stagger timing between harmonic entries
- Each harmonic enters at F(n) * baseStagger instead of linear n * baseStagger

### Priority 4: Tempo-Locked Gap
- BPM control in strip
- Gap values snap to nearest musical subdivision
- All sequence modes (Fib/Gold/Lin/Log) operate relative to beat grid

---

## 6. Pulse Shape Research

For isochronic tones, the pulse shape matters:
- **Square** (hard on/off) — strongest entrainment but can feel harsh
- **Sine-shaped envelope** — gentler, more pleasant, still effective
- **Raised cosine** — good compromise (soft attack/release, full sustain)
- **Recommendation:** Use raised cosine with adjustable "hardness" parameter
  - Hardness 0 = pure sine modulation (gentle)
  - Hardness 1 = square pulse (maximum entrainment)
  - Let Marcus choose based on context

---

*Research compiled by Materia, 12:15 AM MDT, March 15, 2026*
*Sources: Wikipedia, PMC, ResearchGate, AMS, ScienceDirect, MDN Web Docs*
