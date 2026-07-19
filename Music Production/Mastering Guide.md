# Mastering Workflow: The Pro-Grade Signal Chain

This document outlines a standardized, data-driven mastering workflow designed for professional-grade results. This workflow emphasizes serial processing to maintain phase integrity and predictive gain staging.

## 1. Preparation & Gain Staging
Before processing, ensure your pre-master signal is consistent and clean.
- **Project Settings:** Work at your high-res native rate (e.g., 96kHz/24-bit).
- **Gain Staging:** Ensure the track is not clipping prior to the mastering chain.
- **Reference:** Always keep a commercial reference track in your session for A/B comparison.

## 2. The Mastering Chain (Serial Processing)
Apply these effects in order. Do not skip steps or reorder without purpose.

### A. Subtractive EQ
- **Goal:** Correct spectral imbalances and remove mud.
- **Action:** High-pass filter and subtle cuts to resonant frequencies.
- **Metric:** Ensure the frequency spectrum is balanced relative to your reference track.

### B. Additive EQ (Optional)
- **Goal:** Enhance character, "air," or musicality.
- **Action:** Broad, gentle boosts (wide Q factor, e.g., < 2 dB) to add "air" or presence.
- **Caution:** Use sparingly.

### C. MClass Compressor (Glue)
- **Goal:** Unify the mix and provide rhythmic "glue."
- **Settings:** Keep the ratio low (e.g., 2:1 or 4:1) and target only **1–2 dB of gain reduction**.
- **Crucial Step:** Apply **Makeup Gain** until the compressed signal matches the bypass volume level.

### D. High End Tape Saturation (if necessary)
- **Goal:** Provide soft saturation and mild compression on high frequencies to increase perceived loudness and to compensate for a fall off in the high frequency range.
- **Action:** Use a tape recorder saturation plugin and implement a very subtle gain.

### E. Maximizer / Limiter
- **Goal:** Achieve target density and protect against peaks.
- **Settings:** Ceiling: Hard-lock to **-1.0 dB**; Release: Medium or Auto; Soft Clip: Off.
- **Action:** Increase the **Input Gain** slowly.
- **Metric:** Monitor **Gain Reduction**. Target **1–3 dB** during the loudest chorus.

## 3. DAW-Specific Implementation
Regardless of the DAW, maintain the order defined above.

### A. Ableton Live (Primary / Hosting Reason)
- **Rack Setup:** Use an **Audio Effect Rack** to host your chain. This allows for clean bypass and A/B comparison of the entire chain.
- **Hosting Reason:** If using Reason as a Rack Plugin within Live, keep the signal chain inside the Reason Rack Plugin device to minimize latency and maintain signal path consistency.

### B. Reason (Standalone)
- **Master Section:** Use the **Master Bus** inserts.
- **Workflow:** Ensure the MClass Mastering Suite devices are placed in the Mastering Section at the top of the rack. Keep the signal flow vertical and serial to mirror the defined chain.

## 4. Monitoring & Measurement
- **Integrated LUFS:** Target **-13 to -14 LUFS** (dynamic) or **-12 LUFS** (competitive).
- **True Peak:** Must never exceed **-1.0 dB**.

## 5. Final Export & Delivery
- **Format:** WAV
- **Sample Rate:** 48 kHz.
- **Bit Depth:** 24-bit.
- **Dither:** None.