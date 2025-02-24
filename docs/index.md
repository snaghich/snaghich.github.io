# Human Interface System

## Overview
This project implements a human interface system using microcontrollers (ESP32) to process multiple button inputs and generate an output waveform (PWM). The system communicates using I2C, SPI, or UART and provides user feedback through an LED/OLED display.

---
Block Diagram
---
![image](https://github.com/user-attachments/assets/64d827dd-09bd-41c4-914d-755dbe25dfc4)

## Functionality
1. **Button Module Input:** Detects multiple button presses using a microcontroller.
2. **Microcontroller Processing:** The ESP32/PIC reads button inputs and processes them accordingly.
3. **Communication:** Data is transmitted using I2C protocols for external interaction.
4. **Output Waveform Generation:** The system outputs a PWM signal.
5. **User Experience:** The display module (SSD1306 OLED/LEDs) provides real-time feedback on button presses and output states.
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


## Schematics
![My Image](https://github.com/snaghich/snaghich.github.io/blob/main/WhatsApp%20Image%202025-02-24%20at%2003.03.17_747cceb2.jpg)
Image: HUMAN INTERFACE SCHEMATICS 

# About

## Microcontroller
- **ESP32-WROOM-32** (Wi-Fi & Bluetooth, multiple GPIOs, built-in DAC & PWM)
- **PIC16F877A** (If PIC is required, supports I2C, SPI, UART)

## Buttons
- **4x Momentary Push Buttons** (e.g., Omron B3F-4055)

## Resistors & Capacitors
- **10kΩ Resistors** (Pull-up for button inputs)
- **100Ω Resistors** (Series protection for output)
- **0.1µF & 10µF Capacitors** (Decoupling & noise reduction)

## DAC (if needed)
- **MCP4725** (12-bit I2C DAC for waveform output)

## Voltage Regulator
- **AMS1117-3.3V** (For ESP32 power regulation)

## Oscillator (for PIC if required)
- **16MHz Crystal with 22pF Capacitors**

## Display (optional)
- **SSD1306 OLED** (I2C-based) for waveform visualization)

## Power Supply
- **5V DC Power Supply or Battery Module**

## Connection:
  - name: ESP32-WROOM-32
    type: Microcontroller
    datasheet: https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf
    pins:
      - GPIO0: Boot Mode
      - GPIO1: UART TX
      - GPIO3: UART RX
      - GPIO25: DAC Output A
      - GPIO26: DAC Output B
      - GPIO21: SDA (I2C for OLED)
      - GPIO22: SCL (I2C for OLED)

  - name: PIC16F877A
    type: Microcontroller
    datasheet: http://ww1.microchip.com/downloads/en/DeviceDoc/30292c.pdf
    pins:
      - RA0-RA7: Digital Inputs
      - RC3: SCL (I2C)
      - RC4: SDA (I2C)

  - name: MCP4725
    type: DAC
    datasheet: https://cdn.sparkfun.com/datasheets/BreakoutBoards/MCP4725.pdf
    pins:
      - VCC: 3.3V
      - GND: Ground
      - SDA: GPIO21
      - SCL: GPIO22

  - name: AMS1117-3.3V
    type: Voltage Regulator
    datasheet: https://www.diodes.com/assets/Datasheets/AMS1117.pdf
    pins:
      - VIN: 5V
      - VOUT: 3.3V
      - GND: Ground

connections:
  - from: GPIO25 (ESP32)
    to: VOUT (DAC MCP4725)
  - from: GPIO21 (ESP32)
    to: SDA (MCP4725, OLED)
  - from: GPIO22 (ESP32)
    to: SCL (MCP4725, OLED)
```
