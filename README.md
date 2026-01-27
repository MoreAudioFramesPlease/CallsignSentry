# 📡 CallsignSentry
**The Guard Dog for Aviation Radio.**

CallsignSentry is an Android-based "Brain-in-a-Box" that monitors the entire (hardware dependent) local airband radio spectrum. It uses an attached Wideband Software Defined Radio (SDR) and phone-local AI to transcribe radio traffic in real-time, alerting the crew when their specific callsign is detected, and providing instant playback of the last transmission.

---

## 🎯 The Mission
General Aviation pilots often operate in high-workload environments where "missing a call" from ATC is a common point of friction and potential major risk. CallsignSentry acts as a digital safety-net by providing:
1. **Omni-Frequency Monitoring:** Watches your active frequency AND others (e.g., nearby CTAFs, Guard) simultaneously.
2. **Callsign Detection:** Passive AI-driven keyword spotting for your tail number or flight callsign.
3. **Instant Recall:** A one-touch "What did they just say?" audio replay of the last transmission.

🪽 For non-pilots to better understand how contributing could one day help save someone's life, please see [Project Vision & Safety Mission](https://github.com/MoreAudioFramesPlease/CallsignSentry/blob/main/VISION.md).

---

## 🛠️ The Tech Stack
We are prioritizing "proven blocks" to reach a functional prototype quickly:

* **RF Ingest:** [RTLSDR-Airband](https://github.com/szpajder/RTLSDR-Airband) (The "Slicer").
* **Hardware:** HackRF One (or RTL-SDR) + Android Host (via USB-C).
* **Processing:** RAM-first ring buffering to minimize I/O latency and SSD wear.
* **Transcription:** Lightweight, local-only Whisper/Vosk model tuned for aviation phraseology.
* **Interface:** Android Foreground Service with a high-contrast "Annunciator" UI.

---

## 🏗️ System Architecture
1. **SDR Firehose:** SDR captures ~20MHz of bandwidth.
2. **Slicing:** `rtl-airband` detects power-per-bin and "slices" transmissions into discrete PCM/WAV files.
3. **Transcription:** Slices are fed into a local AI inference engine. (possibly extending on [WhisperATC](https://github.com/jlvdoorn/WhisperATC))
4. **Logic:** If `TRANSMISSION_TEXT` contains `LOCAL_CALLSIGN`, trigger an immediate visual/haptic alert.
5. **Buffer:** The last 5 minutes of all audio slices are held in a RAM-based circular buffer for instant replay.

---

## 🚧 Development Roadmap (Phase 1)
- [ ] **Ingest:** Port/Cross-compile `rtl-airband` for Android (Termux/Native).
- [ ] **Transcription:** Benchmark local Whisper.tflite/Vosk performance on mobile SoCs.
- [ ] **Callsign Logic:** Implement fuzzy-string matching to account for transcription variants (e.g., "One-Two-Three" vs ICAO standard "One-Two-Tree" vs the vulgar "One-Hundred-Twenty-Three").
- [ ] **UI:** Simple "Annunciator" screen with a single "REPLAY" button.

---

## 🤝 Contributing
We are looking for:
* **Android/NDK Developers** familiar with USB-OTG and SDR drivers.
* **DSP Engineers** to optimize `rtl-airband` configurations for aviation-specific noise floors.
* **AI/ML Enthusiasts** to help tune local models for high-noise cockpit audio.

---

## ⚖️ A Note on Safety
CallsignSentry is a **Portable Electronic Device (PED)** intended for situational awareness ONLY. It is not a certified avionics component and should never be used as a primary source for navigation or communication. Always "Aviate, Navigate, Communicate" in that order.

---
*Inspired by the risk-stacking principles in "The Killing Zone", by Paul A. Craig.*
