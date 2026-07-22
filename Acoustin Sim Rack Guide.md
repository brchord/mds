# Ableton Live Acoustic Guitar Simulation Rack: Architecture & Implementation Guide

## 1. Architectural Overview & Parallel Routing
To accurately emulate an acoustic guitar from a solid-body electric instrument (using magnetic pickups), a linear serial chain is insufficient. The simulation requires a **parallel chain architecture** inside an Ableton Audio Effect Rack to blend the direct transient snap with the resonant body convolution.

*   **Chain 1: IR Body / Wet** — Houses the pre-filtering, dynamic transient enhancement, IR convolution, high-end air, compression, and spatial environment.
*   **Chain 2: Direct / Dry** — Retains foundational instrument definition and crisp pick attack to prevent the tone from sounding hollow or phase-smeared.

---

## 2. Complete Signal Chain Breakdown

### Chain 1: IR Body / Wet (Signal Path)
1.  **EQ Eight (Pre-Filter & Notch):**
    *   *Band 1 (High-Pass):* 85 Hz, 48 dB/oct slope (removes sub-bass magnetic rumble).
    *   *Band 3 (Bell):* 280 Hz, -4.5 dB gain, Q 2.5 (scoops out boxy mid-range mud).
2.  **Envelope Follower & Utility (Transient Injection):**
    *   *Envelope Follower:* Rise 10%, Fall 40%, Gain 0 dB.
    *   *Utility:* Gain mapped to Envelope Follower output (Range: 0.0 dB to +3.0 dB) to dynamically inject pick-attack snap on downstrokes.
3.  **Lancaster Audio Pulse 2 (IR Convolution):**
    *   *IR File:* High-quality acoustic guitar body impulse response (e.g., Dreadnought or Grand Auditorium body profile).
    *   *Settings:* Linear phase active, 100% wet output.
4.  **EQ Eight (High-End Air):**
    *   *Band 7 (High-Shelf):* 7.2 kHz, +3.5 dB gain, Q 0.71 (introduces string sparkle and air).
5.  **Compressor (Dynamic Leveling):**
    *   *Model:* Peak/RMS mode. Threshold set to trigger lightly on hard strums (-18 dB to -22 dB), Ratio 2:1, Attack 10 ms, Release 100 ms.
6.  **Reverb (Spatial Environment):**
    *   *Algorithm:* Wooden Studio / Room, Pre-delay 15 ms, Decay 1.1 s, Low cut 200 Hz, High cut 6 kHz, Dry/Wet 18%.

### Chain 2: Direct / Dry (Signal Path)
1.  **EQ Eight (Sub-Filter):** High-pass filter at 100 Hz (12 dB/oct) to keep low frequencies clean.
2.  **Utility:** Base volume anchor for parallel blending.

---

## 3. 8-Macro Configuration & Mapping

| Macro | Label | Target Device & Parameter | Operational Range |
| :--- | :--- | :--- | :--- |
| **Macro 1** | Top / Air | EQ Eight (Band 7) Gain | `0.0 dB` to `+6.0 dB` |
| **Macro 2** | Wet/Dry Blend | Rack Chain Volume (Wet vs Dry crossfade) | Inverse mapping (`-inf dB` to `0 dB`) |
| **Macro 3** | Attack / Snap | Envelope Follower Gain / Utility Range | `-12 dB` to `+12 dB` |
| **Macro 4** | Mud Control | EQ Eight (Band 3) Bell Gain | `0.0 dB` to `-9.0 dB` |
| **Macro 5** | Dynamics | Compressor Threshold | `-12 dB` to `-30 dB` |
| **Macro 6** | Space | Reverb Dry/Wet | `5%` to `35%` |
| **Macro 7** | Decay | Reverb Decay Time | `0.6 s` to `2.5 s` |
| **Macro 8** | Low Cut | EQ Eight (Band 1) High-Pass Freq | `60 Hz` to `120 Hz` |
