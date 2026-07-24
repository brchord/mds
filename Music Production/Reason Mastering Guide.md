# Mastering Workflow: All-Native Reason Chain

This document outlines a standardized, data-driven mastering workflow utilizing 100% native Reason devices in the standalone Master Section for phase integrity and analog-modeled precision.

## 1. Preparation & Gain Staging
- **Project Settings:** Work at your high-res native rate (e.g., 96kHz/24-bit).
- **Gain Staging:** Ensure the track is not clipping prior to hitting the Master Section inserts.
- **Reference:** Keep a commercial reference track loaded on an audio track for A/B comparison.

## 2. DAW Setup (Reason Master Section)
- **Rack Routing:** Access the **Master Section** at the very top of the Reason rack interface.
- **Signal Flow:** Maintain a strict vertical, serial signal flow through the built-in Master inserts.

## 3. The Mastering Chain (Serial Processing)
Apply these native Reason devices in order within the Master inserts:

### A. MClass Equalizer (Subtractive Stage)
- **Goal:** Correct spectral imbalances and remove mud.
- **Action:** Engage the low-shelf/high-pass filters to clean up sub-sonics below 25-30 Hz and apply precise cuts to resonant frequencies.

### B. MClass Equalizer (Additive Stage - Optional)
- **Goal:** Enhance character, "air," or musicality.
- **Action:** Apply broad, gentle boosts using a **wide Q factor (< 2 dB)** to add high-end "air" or presence. Use only if the mix requires it.

### C. Scream 4 Sound Destruction Unit / Tape Emulation (Optional)
- **Goal:** Introduce analog warmth, harmonic saturation, and natural transient rounding before compression.
- **Action:** Use the **Scream 4** device configured with the **Tape** algorithm, keeping the Damage/Drive control conservative.

### D. MClass Stereo Compressor (Glue Stage)
- **Goal:** Unify the mix and provide rhythmic "glue" reacting to the pre-warmed transients.
- **Settings:** Ratio at 2:1 or 4:1; slow attack; target **1–2 dB of gain reduction**.
- **Crucial Step:** Match output volume using makeup gain against the bypassed state.

### E. MClass Maximizer (Limiter Stage)
- **Goal:** Achieve target density and protect against peaks.
- **Settings:** Ceiling hard-locked to **-1.0 dB**; engage Soft Clip optionally for added warmth.
- **Action:** Drive input slowly, targeting **1–3 dB** of reduction on the loudest chorus.

## 4. Monitoring & Measurement
- **Integrated LUFS:** Target **-13 to -14 LUFS** (dynamic) or up to **-12 LUFS** (competitive).
- **True Peak:** Must never exceed **-1.0 dB**.

## 5. Final Export & Delivery (Reason Export Dialog)
- **Rendered Track:** Master Output
- **Format:** WAV
- **Sample Rate:** 48 kHz (downsampling from 96 kHz).
- **Bit Depth:** 24-bit.
- **Dither:** None.
