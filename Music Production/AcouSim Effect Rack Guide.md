# Ableton Live Acoustic Guitar Simulation Rack: Final Architecture Guide

## 1. Architectural Overview & Serial Routing Rationale
Following iterative studio testing, both parallel IR mixing and envelope-follower transient modulation were eliminated due to phase cancellation (comb filtering) and dynamic compression pumping. The final implementation relies on a **clean, 100% wet serial chain** to ensure absolute phase integrity and predictable envelope behavior, streamlined into a highly efficient 6-macro layout.

---

## 2. Finalized Signal Chain Breakdown (Serial Path)

1.  **EQ Eight (Pre-Filter):**
    *   *Band 1 (High-Pass):* 85 Hz, **24 dB/oct slope** (gentler slope preserves fundamental low-end warmth and prevents an anemic tone).
    *   *Band 3 (Bell):* 280 Hz, -4.5 dB gain, Q 2.5 (scoops out boxy mid-range mud inherent to magnetic pickups).
2.  **Lancaster Audio Pulse 2 (IR Convolution):**
    *   *IR File:* High-quality acoustic guitar body impulse response (e.g., Dreadnought or Grand Auditorium body profile).
    *   *Settings:* **100% wet output**, with independent input pre-amplification and output gain staging.
3.  **EQ Eight (High-End Air):**
    *   *Band 7 (High-Shelf):* 7.2 kHz, +3.5 dB gain, Q 0.71 (introduces acoustic string sparkle and air).
4.  **Compressor (Dynamic Leveling):**
    *   *Model:* Peak/RMS mode. Threshold set to trigger lightly on hard strums (-18 dB to -22 dB), Ratio 2:1, Attack 10 ms, Release 100 ms (operates stably without modulation spikes).
5.  **Reverb (Spatial Environment):**
    *   *Algorithm:* Wooden Studio / Room, Pre-delay 15 ms, Decay 1.1 s, Low cut 200 Hz, High cut 6 kHz, Dry/Wet 18%.

---

## 3. Finalized 6-Macro Configuration & Mapping

| Macro | Label | Target Device & Parameter | Operational Range |
| :--- | :--- | :--- | :--- |
| **Macro 1** | Top / Air | EQ Eight (Band 7) Gain | `0.0 dB` to `+6.0 dB` |
| **Macro 2** | IR Input Gain | Pulse 2 Input Pre-Amp Volume | `-6.0 dB` to `+6.0 dB` |
| **Macro 3** | IR Output Gain | Pulse 2 Master Output Gain | `-12.0 dB` to `0.0 dB` |
| **Macro 4** | Mud Control | EQ Eight (Band 3) Bell Gain | `0.0 dB` to `-9.0 dB` |
| **Macro 5** | Dynamics | Compressor Threshold | `-12 dB` to `-30 dB` |
| **Macro 6** | Space | Reverb Dry/Wet | `5%` to `35%` |
