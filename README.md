# 🌱 Smart Plant Pot

An ESP32-based smart plant pot that monitors soil moisture, displays the plant's "mood" as an animated face, tracks watering history, and predicts when it'll need water next.

![Status](https://img.shields.io/badge/status-in--progress-yellow)

## Features

- 📊 **Live moisture readout** with animated emoji-style feedback
- 😊 **Emotion animations** — happy bounce/sparkle when watered, droopy/wilting when thirsty, gentle idle "breathing" when healthy
- 📈 **Mini history graph** — sparkline of recent moisture readings
- 📉 **Stats screen** — min / max / average moisture
- 🔮 **Watering forecast** — estimates days until the soil will be dry, based on recent drying rate
- 🔘 **Single-button UI** — short press cycles screens, long press logs "watered" and resets the watering streak
- 🚨 **Dry alert** — pulsing screen border when moisture drops below threshold

## Hardware

| Component | Notes |
|---|---|
| ESP32 DevKit V1 (30-pin) | Main controller |
| Capacitive/resistive soil moisture sensor | 4-pin (VCC, GND, AO, DO) — only AO is used |
| 128x64 I2C OLED (SSD1306) | Status display |
| Push button | Menu navigation + "watered" trigger |

## Wiring

| Component Pin | ESP32 Pin |
|---|---|
| Moisture sensor VCC | 3.3V |
| Moisture sensor GND | GND |
| Moisture sensor AO | GPIO32 (D32) |
| OLED VCC | 3.3V |
| OLED GND | GND |
| OLED SDA | GPIO21 (D21) |
| OLED SCL | GPIO22 (D22) |
| Button (other leg to GND) | GPIO23 (D23) |

> **Note on the moisture sensor pin:** GPIO32 (ADC1) is used instead of ADC2 pins like GPIO2/GPIO15. ADC2 pins share hardware with the ESP32's WiFi radio and become unreliable whenever WiFi is active, and some ADC2 pins are also "strapping pins" that affect boot behavior. If you plan to add WiFi features later, stick to ADC1 pins: **32, 33, 34, 35, 36 (VP), 39 (VN)**.

## Libraries required

Install via Arduino IDE → Library Manager:
- `Adafruit GFX Library`
- `Adafruit SSD1306`

`Wire.h` ships with the ESP32 board package.

**Board setup:**
- Boards Manager URL: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
- Board: **ESP32 Dev Module**
- Upload speed: 115200 (safer than 921600 on some clone boards/cables)

## Calibration

Soil moisture sensors vary board to board, so calibrate before first use:

1. Flash the calibration sketch (see `/calibration` — prints raw ADC value to Serial + OLED).
2. Note the raw value with the sensor in **open dry air** → this is `DRY_VALUE`.
3. Note the raw value with the sensor **fully in water** (prongs only, not the electronics) → this is `WET_VALUE`.
4. Plug both into the main sketch:
   ```cpp
   #define DRY_VALUE ___
   #define WET_VALUE ___
   ```

Typical range: dry ~3000–4095, wet ~1200–1800 — but always use your own measured values, since sensor boards vary.

## Project structure

```
/smart-plant-pot
  ├── smart_plant_pot.ino     # main sketch
  ├── calibration/
  │   └── calibration.ino     # raw ADC reading sketch for calibration
  ├── tests/
  │   ├── i2c_scanner.ino     # finds OLED I2C address / confirms it's alive
  │   ├── oled_test.ino       # basic display test
  │   └── button_test.ino     # isolates button wiring issues
  └── README.md
```

## Roadmap / ideas

- [ ] WiFi dashboard for remote monitoring
- [ ] Piezo buzzer for audible dry alerts
- [ ] Deep sleep between readings for battery operation
- [ ] Data logging to SD card or a cloud sheet

## Known issues

- Moisture readings can be affected by sensor corrosion over time (resistive-type sensors). Powering the sensor only briefly before each reading (via a spare GPIO instead of a permanent 3.3V connection) can extend its life.

## License

MIT — do whatever you want with it.
