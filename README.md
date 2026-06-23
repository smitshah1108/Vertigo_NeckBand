# 🎗️ Vertigo Band
### WRO 2026 · Future Innovators · *Robots Meet Culture*

> **"Feel the signal before the fall."**
> A wearable ESP32 neckband that detects dangerous head angles in real time, fires haptic alerts on-device, and logs every episode to a Raspberry Pi — protecting cultural performers and patients alike.

🌐 **[Live Project Site](https://udaydotoffical-svg.github.io/Vertigo_NeckBand/)**

---

## 🎭 The Cultural Angle

Vertigo Band was built for WRO 2026's **"Robots Meet Culture"** theme. It directly addresses two of the three official WRO areas:

| Area | Fit |
|------|-----|
| **Area 2 — Co-Creation: Humans, Robots & AI** | The band is a real-time robotic partner for cultural performers — it doesn't replace the artist, it protects them during practice |
| **Area 1 — Protecting & Preserving Cultural Heritage** | Classical dancers, musicians, and martial artists are living cultural heritage. When injury ends a performer's career, the tradition they carry goes with them |

**Who it protects:**
- Bharatanatyam & Kathak dancers (intense head isolations, aramandi posture)
- Classical musicians (violinists, veena players — asymmetric neck positions for hours)
- Kalaripayattu & Chhau martial artists
- Vertigo & vestibular disorder patients
- Elderly artists continuing to teach and perform

---

## 🧠 System Architecture

Three-layer pipeline — fully offline-capable:

```
┌─────────────────────────────┐
│  LAYER 1 — Wearable (ESP32) │  Senses + Acts
│  MPU-6050 → ML inference    │
│  → Haptic motor (instant)   │
└────────────┬────────────────┘
             │ UDP / HTTP (local Wi-Fi)
┌────────────▼────────────────┐
│  LAYER 2 — Wireless Link    │  No internet required
│  Pitch · Roll · Yaw + flag  │
└────────────┬────────────────┘
             │
┌────────────▼────────────────┐
│  LAYER 3 — Raspberry Pi     │  Logs + Dashboard
│  SQLite · Flask · Charts    │
└─────────────────────────────┘
```

**Key design decisions:**
- Alert logic runs **100% on-device** — zero cloud dependency, zero latency
- Wi-Fi only used for logging bursts every few minutes
- Hot-swap LiPo system for uninterrupted 24/7 runtime

---

## 🤖 ML Pipeline

Built and deployed via **Edge Impulse**:

1. Raw IMU data collected from MPU-6050 (pitch, roll, yaw)
2. Labelled dataset: safe angles vs. dangerous trigger positions
3. Model trained on Edge Impulse cloud
4. Quantized and exported as Arduino library
5. Runs on-device at inference — no cloud call needed

---

## 🔩 Hardware

| Component | Role |
|-----------|------|
| ESP32 | Central MCU — ML inference, Wi-Fi, motor control |
| MPU-6050 | 6-axis IMU — 3-axis gyro + accelerometer via I²C |
| Vibration Motor | Haptic alert on dangerous angle detection |
| LiPo Battery | Hot-swap capable — dual battery design |
| Raspberry Pi | Local server — SQLite logging + Flask dashboard |

---

## ⚡ Power System

Hot-swap LiPo design for continuous operation:

- Two batteries in rotation — swap one while the other powers the device
- No shutdown needed during battery change
- Full runtime maintained during performances or clinical monitoring
- **WRO talking point:** demonstrates real-world deployment thinking beyond prototype

---

## 🚀 Getting Started

### ESP32 Firmware
```bash
# Clone the repo
git clone https://github.com/udaydotoffical-svg/Vertigo_NeckBand.git

# Open in Arduino IDE
# Install dependencies:
# - MPU6050 by Electronic Cats
# - Edge Impulse Arduino library (exported from your EI project)
# - WiFi.h (built-in ESP32)

# Flash to ESP32
```

### Raspberry Pi Server
```bash
# Install dependencies
pip install flask sqlite3

# Run the logging server
python server.py
```

### Edge Impulse
1. Create a new project at [edgeimpulse.com](https://edgeimpulse.com)
2. Connect ESP32 as data forwarder
3. Collect IMU data and label (safe / danger)
4. Train classifier → export Arduino library
5. Drop into `/firmware/lib/` and reflash

---

## 📁 Repo Structure

```
Vertigo_NeckBand/
├── firmware/           # ESP32 Arduino code
│   ├── vertigo_band.ino
│   └── lib/            # Edge Impulse exported model
├── server/             # Raspberry Pi Flask server
│   ├── server.py
│   └── templates/      # Dashboard HTML
├── index.html          # Project website
└── README.md
```

---

## 🏆 WRO 2026 Judging Alignment

| Rubric | How Vertigo Band addresses it |
|--------|-------------------------------|
| **Idea, Quality & Creativity** | Novel use of wearable ML for cultural performer safety — not an obvious idea |
| **Social Impact & Need** | Protects practitioners of UNESCO-recognized art forms; also applicable to medical patients, workers |
| **Robotic Solution** | Multiple sensors + actuators, autonomous decision-making (no human triggers the alert) |
| **Code Efficiency** | On-device inference, minimal Wi-Fi usage, threshold + ML hybrid logic |
| **Key Innovation** | Offline-first ML on a wearable — competitor solutions require cloud connectivity |

---

## 👥 Team

WRO Future Innovators 2026 — India

---

## 📄 License

MIT License — open source, open hardware.
