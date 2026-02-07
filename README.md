# 📡 CallsignSentry
**An in-cockpit watchdog for aviation radio.**

CallsignSentry does not have ANY code today (2026-01-29). There are a handful of interested people who will hopefully be participating in research to make it real over time. Please check out [Discussions](https://github.com/MoreAudioFramesPlease/CallsignSentry/discussions) to see where we're at and what we're struggling with.

CallsignSentry will be a "Brain-in-a-Box" that monitors and records transmissions on the local airband radio spectrum. It will use a wideband Software Defined Radio (SDR) and device-local keyword spotting and transcription process to capture radio traffic in realtime,  alert the crew when their specific callsign (or keyword of interest) is detected, and provide instant playback of relevant transmissions.

---

## 🎯 The Mission
General Aviation pilots often operate in high-workload environments where "missing a call" from another station (like ATC) is a common point of friction and potential risk. CallsignSentry acts as a digital safety-net by providing:
1. **Whole-band Monitoring:** Watches your active frequency AND others (e.g., nearby CTAFs, Tower, Approach, Departure, Ground, Guard) simultaneously.
2. **Callsign Detection:** Passive keyword spotting for tail number or flight callsign.
3. **Instant Recall:** A one-touch "What did they just say?" audio replay of transmissions.

🪽 For non-pilots, please see [Project Vision & Safety Mission](https://github.com/MoreAudioFramesPlease/CallsignSentry/blob/main/VISION.md).

---

## 🛠️ The Tech Stack
We are prioritizing "proven blocks" to reach a functional prototype quickly:

* **RF Ingest:** [RTL-Airband](https://github.com/szpajder/RTLSDR-Airband) (The "Slicer").
* **Hardware:** HackRF One (or RTL-SDR) + Android Host (via USB-C).
* **Processing:** RAM-first ring buffering to minimize I/O latency and SSD wear.
* **Processing and Transcription:** Lightweight, local-only Keyword (aka Wakeword) Spotting, combined with Whisper/Vosk tuned for aviation phraseology.
* **Interface:** TBD Foreground Service with a high-contrast "Annunciator" UI and an easy, non-distracting way to play recorded transmissions

What's alredy been proven?

* ✅ Automatic transmissions extraction from wideband (by ground stations) using open-source projects (like [RTL-Airband](https://github.com/szpajder/RTLSDR-Airband))
* ✅ In-cockpit AI-based transcription of radio, by commercial projects. See Stratus Insight (https://stratusinsight.app/) and FlyShirley (https://airplane.team/)


What's missing?
* ⛔ Reliable, open-source and offline keyword spotting and transcription optimized for narrowband AM and localized airband traffic
* ⛔ An easy way to alert crew on keyword hits and link to recorded data
* ⛔ A hardware Bill of Materials tested in-flight

---

## 🏗️ Architecture
1. **Radio intake:** SDR captures ~20MHz of bandwidth covering the civil airband from 108–137 MHz
2. **Slicing:** `rtl-airband` detects power-per-bin and "slices" transmissions into discrete PCM/WAV files.
3. **Transcription:** Slices are fed into a local KWS/AI inference engine. (possibly extending on [WhisperATC](https://github.com/jlvdoorn/WhisperATC))
4. **Logic:** If `TRANSMISSION_TEXT` contains `LOCAL_CALLSIGN`, trigger an immediate visual/haptic alert.
5. **Buffer:** The last N minutes of all audio slices are kept in RAM-based circular buffers for instant replay.

---

## 🚧 Development Roadmap (Phase 1)
- [ ] **Transcription:** Benchmark local KWS/Whisper.tflite/Vosk performance on low-power hardware.
- [ ] **Callsign Logic:** Implement fuzzy-string matching to account for transcription variants (e.g., "One-Two-Three" vs ICAO standard "One-Two-Tree" vs the vulgar "One-Hundred-Twenty-Three").
- [ ] **UI:** Simple "Annunciator" screen with a single "REPLAY" button.

---

## 🤝 Contributing
We are looking for:
* **SDR enthusiasts** who are already experienced in using this tech to monitor local ATC.
* **DSP Engineers** to optimize `rtl-airband` configurations for aviation-specific noise floors.
* **AI/ML Enthusiasts** to help tune local models for high-noise cockpit audio.
* **Pilots and operators** who see a use for this integration in actual GA missions. In particular, we are interested in hearing from those who might want to use this for search-and-rescue. 

---

## ⚖️ A Note on Safety
CallsignSentry is a **Portable Electronic Device (PED)** intended for situational awareness ONLY. It is not a certified avionics component and should never be used as a primary source for navigation or communication. Always "Aviate, Navigate, Communicate" in that order.

---
*Inspired by the risk-stacking principles in "The Killing Zone", by Paul A. Craig.*
