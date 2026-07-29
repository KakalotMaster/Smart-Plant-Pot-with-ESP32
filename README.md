# Smart-Plant-Pot-with-ESP32

An ESP32-based smart plant pot that monitors soil moisture, displays the plant's "mood" as an animated face, tracks watering history, and predicts when it'll need water next.

![Status](https://img.shields.io/badge/status-in--progress-yellow)

## Features

-  **Live moisture readout** with animated emoji-style feedback
-  **Emotion animations** — happy bounce/sparkle when watered, droopy/wilting when thirsty, gentle idle "breathing" when healthy
-  **Mini history graph** — sparkline of recent moisture readings
-  **Stats screen** — min / max / average moisture
-  **Watering forecast** — estimates days until the soil will be dry, based on recent drying rate
-  **Single-button UI** — short press cycles screens, long press logs "watered" and resets the watering streak
-  **Dry alert** — pulsing screen border when moisture drops below threshold

## Hardware
-a ESP32 based developer board-
-a 128x64 I2C display with the SSD1306 driver-
-a single push button-
-Soil Moisture Sensor Module with LM393 Comparator-

## Wiring



> **Note on the moisture sensor pin:** GPIO32 (ADC1) is used instead of ADC2 pins like GPIO2/GPIO15. ADC2 pins share hardware with the ESP32's WiFi radio and become unreliable whenever WiFi is active, and some ADC2 pins are also "strapping pins" that affect boot behavior. If you plan to add WiFi features later, stick to ADC1 pins: **32, 33, 34, 35, 36 (VP), 39 (VN)**.

## Libraries required

Install in Arduino IDE -> Library Manager:
- `Adafruit GFX Library`
- `Adafruit SSD1306`

`Wire.h` comes pre-installed. if you can't find it search for in library manager.

**Board setup:**
- Boards Manager URL: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
- Board: **ESP32 Dev Module**
- Upload speed: 115200 (you can also adjust the speed if your cable is fast enough)

## Calibration

while using the soil moisture sensor, the readings may be wrong if the code is not calibrated.
İf you think your reading are wrong: look at CALIBRATION.md

## Project structure

```
/smart-plant-pot.zip
  ├── smart_plant_pot.ino     # main sketch
  ├── calibration/
  │   └── calibration.ino     # raw ADC reading sketch for calibration
  |   └──  CALIBRATION.md     # explains how calibration.ino works
  ├── tests/
  │   ├── i2c_scanner.ino     # finds OLED I2C address / confirms it's alive
  │   ├── oled_test.ino       # basic display test
  │   └── button_test.ino     # isolates button wiring issues
  └── README.md
  └── LICENSE.md
```

## License

MIT — open-sourced (look at LICENSE.md for more)
