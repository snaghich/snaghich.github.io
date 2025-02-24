# Human Interface System (Buttons Input & Output)

## Overview
This project implements a human interface system using microcontrollers (ESP32 or PIC) to process multiple button inputs and generate an output waveform (PWM/Analog Out). The system communicates using I2C, SPI, or UART and provides user feedback through an LED/OLED display.

## System Flowchart
![Flowchart]([image](https://github.com/user-attachments/assets/64d827dd-09bd-41c4-914d-755dbe25dfc4))

## Documentation
- docs/Block Diagram.md
- docs/Component Selection.md
- docs/Schematics.md

## Components Used

| Component | Model | Purpose | Datasheet | Cost | Pros | Cons |
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| **Microcontroller** | ESP32-WROOM-32 | Wi-Fi & Bluetooth, multiple GPIOs, built-in DAC & PWM | [Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf) | $5-8 | Powerful, supports multiple interfaces, built-in DAC | Higher power consumption |
| **Microcontroller** | PIC16F877A | If PIC is required, supports I2C, SPI, UART | [Datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/30292c.pdf) | $4-6 | Low power, widely used in industry | Limited processing power |
| **Button Module** | TS02-66-70-BK-100-LCR-D | Multiple inputs | - | $1-3 | Simple, reliable | Requires debounce circuit |
| **Resistors** | 10kΩ | Pull-up for button inputs | - | <$1 | Prevents floating states | Required for stable operation |
| **Capacitors** | 0.1µF & 10µF | Decoupling & noise reduction | - | <$1 | Improves stability | Needs proper placement |
| **DAC (if needed)** | MCP4725 | 12-bit I2C DAC for waveform output | [Datasheet](https://cdn.sparkfun.com/datasheets/BreakoutBoards/MCP4725.pdf) | $3-5 | Precise, I2C controlled | Extra component needed |
| **Voltage Regulator** | AMS1117-3.3V | For ESP32 power regulation | [Datasheet](https://www.sparkfun.com/datasheets/Components/LD1117V33.pdf) | <$1 | Stable 3.3V output | Needs heat dissipation |
| **Oscillator (for PIC if required)** | 16MHz Crystal with 22pF Capacitors | Provides clock for PIC | - | <$1 | Accurate timing | Requires external components |
| **Display (optional)** | SSD1306 OLED | I2C-based for visualization | [Datasheet](https://cdn-shop.adafruit.com/datasheets/SSD1306.pdf) | $5-10 | Low power, clear display | Small size |
| **Power Supply** | 5V DC Power Supply or Battery | Provides power to circuit | - | Varies | Portable, reliable | Needs regulation |

## Functionality
1. **Button Module Input:** Detects multiple button presses using a microcontroller.
2. **Microcontroller Processing:** The ESP32/PIC reads button inputs and processes them accordingly.
3. **Communication:** Data is transmitted using I2C, SPI, or UART protocols for external interaction.
4. **Output Waveform Generation:** The system outputs either a PWM signal or an analog waveform.
5. **User Engadgement:** The display module (SSD1306 OLED/LEDs) provides real-time feedback on button presses and output states.

## Pin Connections

### ESP32-WROOM-32 Pin Mapping
| Function | ESP32 Pin |
|----------|----------|
| Button Inputs | GPIO 12, 13, 14, 15 |
| I2C SDA | GPIO 21 |
| I2C SCL | GPIO 22 |
| SPI MOSI | GPIO 23 |
| SPI MISO | GPIO 19 |
| SPI SCK | GPIO 18 |
| UART TX | GPIO 17 |
| UART RX | GPIO 16 |
| DAC Output | GPIO 25 or GPIO 26 |
| PWM Output | Any GPIO with PWM support |
| OLED Display | I2C (SDA - GPIO 21, SCL - GPIO 22) |

### Power Supply Connections
- **ESP32 Power:** 3.3V from AMS1117 regulator
- **Power:** 5V regulated
- **OLED Display:** 3.3V supply from ESP32
- **Buttons:** Connected with pull-up resistors (10kΩ)

