# InternalEnvironmentMonitor

**InternalEnvironmentMonitor** is an intelligent indoor air quality and room-state monitor powered by the **Arduino UNO Q** and the **Adafruit BME688** 4-in-1 environmental gas sensor. 

Based on the **Atmosis** edge-AI concept, this project combines real-time microcontroller sensor polling with on-device machine learning to detect indoor events (such as cooking fumes, stale air, or high humidity) and translate raw metrics into plain-language health and comfort advisories.

---

## Key Features
* **4-in-1 Environmental Sensing:** Tracks temperature, barometric pressure, relative humidity, and Volatile Organic Compounds (VOCs).
* **Dual-Core Architecture:** Offloads low-level I2C sensor polling to the STM32U585 MCU while running AI inference and local hosting on the Qualcomm QRB2210 Linux core.
* **Edge AI Gas Classification:** Uses custom heater profiles and machine learning models (via Edge Impulse or Bosch BME AI-Studio) to recognise specific indoor air patterns offline.
* **Plain-Language Advisories:** Summarises environmental trends into actionable recommendations without relying on cloud APIs.

---

## Hardware Requirements
* **Microcontroller / SBC:** [Arduino UNO Q](https://arduino.cc)
* **Sensor:** [Adafruit BME688](https://adafruit.com) (Temperature, Humidity, Pressure, and Gas Sensor)
* **Connection:** 1x STEMMA QT / Qwiic JST SH 4-pin cable (or standard male-to-female jumper wires)
* **Power:** USB-C power supply for the Arduino UNO Q

---

## Wiring & Hardware Setup

### Option 1: Qwiic / STEMMA QT (Recommended)
1. Connect one end of the 4-pin Qwiic cable to the **Qwiic connector** on the Arduino UNO Q.
2. Connect the other end to either **STEMMA QT port** on the Adafruit BME688 breakout board.

### Option 2: Standard Header Pins (I2C)
If you are using jumper wires, connect the pins as follows:

| Adafruit BME688 | Arduino UNO Q |
| :--- | :--- |
| **VIN** | 3.3V |
| **GND** | GND |
| **SCK / SCL** | SCL (A5) |
| **SDI / SDA** | SDA (A4) |

---

## Software Stack & Development Environment
* **IDE & Build System:** JetBrains CLion with PlatformIO
* **Firmware (MCU Core):** C++ / Arduino Framework & Bosch BSEC2 Library
* **AI & Application Layer (Linux Core):** Python, Edge Impulse Linux SDK / Bosch BME AI-Studio

---

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com
cd InternalEnvironmentMonitor
```

### 2. Open in CLion & Configure PlatformIO
1. Open the `InternalEnvironmentMonitor` folder in **CLion**.
2. Ensure the **PlatformIO plugin** is installed and initialised.
3. Verify your `platformio.ini` includes the required sensor libraries:
```ini
lib_deps =
    adafruit/Adafruit BME680 Library
    boschsensortec/BSEC2 Software Library
```

### 3. Build & Flash MCU Firmware
1. Use the PlatformIO build configuration in CLion to compile the STM32U585 firmware in `src/`.
2. Connect the Arduino UNO Q via USB-C and upload the firmware to the board.

### 4. Collect Baseline Room Data & Train AI Model
1. Capture serial telemetry logs across different room states (e.g., *Normal Air*, *Window Open*, *Cooking*, *Stuffy Room*).
2. Export the CSV logs to [Edge Impulse](https://edgeimpulse.com) or [Bosch BME AI-Studio](https://bosch-sensortec.com) to train your gas classification model.

### 5. Deploy & Run on the Linux Core
1. Transfer your compiled model to the `/models` directory on the UNO Q's Linux filesystem.
2. Start the inference and monitoring service:
```bash
python3 app/main.py
```

---

## Project Structure
```text
InternalEnvironmentMonitor/
├── include/            # C++ header files for sensor & heater profiles
├── src/                # STM32U585 firmware source files (PlatformIO)
├── lib/                # Private project libraries
├── app/                # Python scripts for Linux core AI inference & UI
├── models/             # Trained Edge Impulse / BSEC classification models
├── data/               # Sample CSV logs for training gas heater profiles
├── platformio.ini      # PlatformIO project & dependency configuration
└── README.md
```

---

## Roadmap
- [x] Hardware integration with Adafruit BME688 via Qwiic
- [x] PlatformIO & CLion project setup
- [ ] Baseline dataset collection for indoor room states
- [ ] Local web dashboard for real-time telemetry and AI alerts
- [ ] Offline alert notifications (OLED display / local webhook)

---

## Acknowledgements
* Inspired by the **Atmosis** edge-AI environmental monitoring architecture.

## Licence
Distributed under the MIT Licence. See `LICENSE` for more information.
