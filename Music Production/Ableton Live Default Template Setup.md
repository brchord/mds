# Ableton Live Standard Production Template

## 1. Core Audio Tracking Channels

### Track 1: Electric Guitar
* **Input:** Direct instrument line input
* **Insert Chain:** Custom native multi-effect guitar processing rack

### Track 2: Acoustic Simulator (Electric Guitar Input)
* **Input:** Direct instrument line input for rapid sketching
* **Insert Chain:** Custom acoustic simulator rack (EQ filtering, high-frequency transient enhancement, and body resonance emulation)

### Track 3: Acoustic Guitar (Recorded)
* **Input:** Small-capsule condenser mic input
* **Insert Chain:** 
  * **EQ Eight** (High-pass filter active at 80Hz–100Hz)
  * **Saturator** (Subtle soft-clip / analog curve for peak smoothing)
  * **Compressor** (Fast attack, moderate ratio to catch transient spikes)

### Track 4: Lead Vocals
* **Input:** Hardware vocal mic input
* **Insert Chain:** 
  * **Saturator** (Subtle front-end analog drive)
  * **Compressor** (Fast attack/release for peak management)
  * **EQ Eight** (Surgical low-mid cleanup and air boost)
  * **Limiter** (Safety ceiling set to -1.0 dB)

---

## 2. Spatial Auxiliary Returns (Live Standard Native)

* **Return A (Vocal Long Plate / Hall):** Live Standard **Reverb** (Large size, 2.5s–4.0s decay, dark high-cut tail, 100% wet)
* **Return B (Plucked / Acoustic Space):** Live Standard **Reverb** (Medium room size, 1.2s–1.8s decay, mid-depth ambience)
* **Return C (Rhythmic Delay):** Live Standard **Simple Delay** or **Filter Delay** (Configured to **1/8 triplets (1/8T)** with filtered feedback)

---

## 3. Master Output Chain (Tracking & Mixing Phase)

* **Limiter:** Brickwall safety ceiling set strictly to **-1.0 dB** with 0.0 dB gain
* **Voxengo SPAN:** Configured with a 4.5 dB/octave tilt slope and high-resolution FFT for spectral analysis
* **Youlean Loudness Meter:** Set for integrated LUFS and True Peak verification

---

## 4. Default Gain Staging & Fader Calibration

* **Initial Fader Starting Points:** Set core track faders (guitars, vocals, acoustic simulator) between **-6.0 dB and -10.0 dB** by default.
* **Purpose:** Preserves headroom during the tracking and sketching phase, prevents premature summing bus crowding, and avoids hitting the master limiter prematurely while keeping fader resolution optimal.
