# Mastering Workflow: All-Native Ableton Live Chain

This document outlines a standardized, data-driven mastering workflow utilizing 100% native Ableton Live devices for maximum precision, zero external latency, and phase integrity.

## 1. Preparation & Gain Staging
- **Project Settings:** Work at your high-res native rate (e.g., 96kHz/24-bit).
- **Gain Staging:** Ensure the track is not clipping prior to hitting the Master track.
- **Reference:** Keep a commercial reference track loaded on an audio track for A/B comparison.

## 2. DAW Setup (Native Ableton Audio Effect Rack)
- **Master Insert:** Group an **Audio Effect Rack** on Live’s Master track.
- **Signal Flow:** Maintain a strict vertical, serial signal chain inside the rack for clean global bypassing.

## 3. The Mastering Chain (Serial Processing)
Apply these native devices in order:

### A. EQ Eight (Subtractive Stage)
- **Goal:** Correct spectral imbalances and remove mud.
- **Action:** High-pass filter sub-sonics below 25-30 Hz (24/48 dB slope) and apply precise cuts to resonant frequencies.

### B. EQ Eight (Additive Stage - Optional)
- **Goal:** Enhance character, "air," or musicality.
- **Action:** Apply broad, gentle boosts using a **wide Q factor (< 2 dB)** to add high-end "air" or presence. Use only if the mix requires it.

### C. Saturation / Tape Emulation (Optional)
- **Goal:** Introduce analog warmth, harmonic saturation, and natural transient rounding before compression.
- **Settings:** Use Ableton's **Saturation** device set to the **Tape** curve with a conservative **Drive** setting.

### D. Glue Compressor (Glue Stage)
- **Goal:** Unify the mix and provide rhythmic "glue" reacting to the pre-warmed transients.
- **Settings:** Ratio at 2:1 or 4:1; slow attack; target **1–2 dB of gain reduction**.
- **Crucial Step:** Match output volume using makeup gain against the bypassed state.

### E. Limiter (Peak Safety Stage)
- **Goal:** Achieve target density and protect against peaks.
- **Settings:** Ceiling hard-locked to **-1.0 dB**; Auto release.
- **Action:** Drive input gain slowly, targeting **1–3 dB** of reduction on the loudest chorus.

## 4. Monitoring & Measurement
- **Integrated LUFS:** Target **-13 to -14 LUFS** (dynamic) or up to **-12 LUFS** (competitive).
- **True Peak:** Must never exceed **-1.0 dB**.

## 5. Final Export & Delivery (Ableton Live Render Dialog)
- **Rendered Track:** Master
- **Format:** WAV
- **Sample Rate:** 48 kHz (downsampling from 96 kHz).
- **Bit Depth:** 24-bit.
- **Dither:** None.
- **Resampling Quality:** Best / High Quality.
