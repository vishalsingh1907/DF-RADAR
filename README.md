
# 2.4GHz RF Radar | ESP32 + nRF24L01

A DIY RF activity scanner built using an ESP32, an nRF24L01+ transceiver, and a 3.5-inch SPI TFT display. The project visualizes detected radio-frequency activity through a radar-style interface.

> **Note:** This is a receive-only RF activity scanner, not a conventional radar. It detects radio transmissions rather than physical objects.

## Features

- **2.4GHz RF scanning** using the nRF24L01+ transceiver.
- **Wi-Fi detection** with received signal strength indicator (RSSI) readings.
- **Radar-style visualization** with a rotating sweep and signal indicators.
- **ESP32-based architecture** for wireless scanning and display control.
- **Expandable design** for future direction-finding and signal-logging features.

## Hardware Requirements

| Component | Purpose |
|---|---|
| ESP32 NodeMCU-32S | Main controller |
| nRF24L01+ | RF channel activity detection |
| 3.5-inch SPI TFT display | Radar visualization |
| Regulated 3.3V power module | Stable radio power |
| Jumper wires and breadboard | Circuit connections |
| USB cable | Programming and power |

**Display compatibility:** The display must use a compatible SPI interface and controller, such as ILI9486 or ILI9488. Parallel/DPI Raspberry Pi displays are not directly compatible with this setup.

## System Architecture

```text
       ESP32 Controller
          /         \
         /           \
    nRF24L01+      Wi-Fi Scan
         \           /
          \         /
       Signal Processing
               |
        TFT Radar Display
```

## Wiring

### ESP32 to nRF24L01+

| nRF24L01+ Pin | ESP32 Pin |
|---|---|
| VCC | External regulated 3.3V |
| GND | GND |
| CE | GPIO 4 |
| CSN | GPIO 5 |
| SCK | GPIO 18 |
| MOSI | GPIO 23 |
| MISO | GPIO 19 |
| IRQ | Not connected |

### ESP32 to SPI TFT Display

| Display Pin | ESP32 Pin |
|---|---|
| VCC | Compatible supply voltage |
| GND | GND |
| CS | GPIO 15 |
| RESET | GPIO 2 |
| DC / RS | GPIO 0 |
| MOSI / SDI | GPIO 13 |
| SCK | GPIO 14 |
| MISO / SDO | GPIO 12 |
| LED / Backlight | As specified by the display board |

Verify the actual pin labels and voltage requirements of your hardware before connecting it.

### Power Requirements

- Supply the nRF24L01+ with a stable, regulated **3.3V** source.
- Connect the grounds of the ESP32, radio module, and external power supply.
- Add a 10–100 µF capacitor across the nRF24L01+ power pins to reduce voltage fluctuations.
- Never connect the nRF24L01+ VCC to 5V.
- Verify the display's power and logic-voltage requirements.

## Software Requirements

- [Arduino IDE](https://www.arduino.cc/en/software)
- [ESP32 Arduino Core](https://github.com/espressif/arduino-esp32)
- [RF24 Library](https://github.com/nRF24/RF24)
- [TFT_eSPI Library](https://github.com/Bodmer/TFT_eSPI)

## Installation and Setup

1. Install the Arduino IDE.
2. Open **File → Preferences → Additional Boards Manager URLs**.
3. Add the following URL:

   ```text
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
   ```

4. Open Boards Manager and install **esp32 by Espressif Systems**.
5. Select **NodeMCU-32S** or **ESP32 Dev Module**, depending on your board.
6. Install the RF24 and TFT_eSPI libraries through Library Manager.
7. Configure `TFT_eSPI/User_Setup.h` for your display driver and wiring.

Example TFT_eSPI configuration:

```cpp
#define ILI9486_DRIVER

#define TFT_MISO 12
#define TFT_MOSI 13
#define TFT_SCLK 14
#define TFT_CS   15
#define TFT_DC   0
#define TFT_RST  2
```

Select the correct display driver for your hardware. If your display uses ILI9488, configure the corresponding driver instead.

8. Connect the ESP32 through USB and select the correct board and port.
9. Upload the project sketch.

## How It Works

1. The ESP32 initializes the TFT display and nRF24L01+ module.
2. The radio scans individual channels and checks for activity using its Received Power Detector (RPD).
3. The ESP32 periodically scans nearby Wi-Fi networks and reads their RSSI values.
4. Detected activity is represented by indicators on the radar-style display.
5. The sweep continues, and older indicators are removed over time.

## Understanding the Display

- **Sweep line:** Represents the current scan cycle.
- **Blips:** Represent detected or sampled wireless activity.
- **Signal strength:** Wi-Fi RSSI is used to estimate relative signal strength.
- **Blip position:** Does not establish the actual direction or distance of a transmitter.

The current implementation assigns approximate or random angular positions to some detections. The display should therefore be treated as an RF activity visualization, not a calibrated map of surrounding devices.

## Troubleshooting

| Problem | Possible Solution |
|---|---|
| Blank or white display | Verify the driver, SPI pins, reset pin, and display power. |
| Distorted display output | Check wiring and reduce SPI frequency. |
| nRF24L01+ initialization failure | Check the 3.3V supply, common ground, SPI connections, and capacitor. |
| No Wi-Fi networks detected | Verify Wi-Fi initialization and scanning operations. |
| Unstable radio readings | Improve power stability and shorten jumper wires. |
| Touchscreen not responding | Verify touch-controller wiring and software configuration. |

## Future Improvements

- Servo-mounted directional antenna for approximate RF direction finding.
- Improved signal visualization and filtering.
- Touchscreen interface for scan information.
- Signal logging and historical activity graphs.
- Smoother radar animation using display buffering.
- Custom enclosure and integrated power system.

## Limitations

- This project detects radio-frequency activity, not passive physical objects.
- The nRF24L01+ RPD is a threshold-based activity indicator, not a calibrated spectrum analyzer.
- Wi-Fi RSSI does not directly measure distance.
- The nRF24L01+ cannot reliably identify every Wi-Fi or Bluetooth transmission or decode those protocols.
- The current display does not provide true directional measurements.

## Responsible Use

This project is intended for passive RF observation, experimentation, and educational purposes. It does not require transmission or jamming functionality. Follow applicable radio regulations and respect the privacy and security of wireless networks.

## License

Choose an appropriate open-source license before publishing the repository. The [MIT License](https://opensource.org/license/mit) is one option for projects intended to permit reuse, modification, and redistribution.

---

**Platform:** ESP32 · Arduino IDE  
**Hardware:** nRF24L01+ · SPI TFT Display  
**Purpose:** Educational RF activity visualization
