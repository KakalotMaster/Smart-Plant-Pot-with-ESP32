## Calibration

The code itself comes pre-calibrated with my own settings.
However, if you somehow mess up the code you can re-calibrate it using the same schematic

**First** load calibration.ino to your ESP32 Devkit v1. 

**Then** hold the soil moisture sensor in the air for about 5-10 seconds and note the raw reading down.

**After that** stick the sensor in a cup of water and note the raw reading from that one too.

*After all those* you can re-write the #define DRY_VALUE and #define WET_VALUE to match your readings.

Still having problems? write out to me in yamanbirturk@gmail.com
