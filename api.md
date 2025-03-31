# API – Human Interface Module (HIM)

**Subsystem Role:**  
The Human Interface Module (HIM) is responsible for handling user input via tactile buttons, providing system feedback through an OLED screen and analog waveform output, and communicating over UART with the upstream microcontroller (ESP32 for MQTT) and downstream subsystems (Motor, Microphone). Messages are formatted according to the class protocol and embedded between bytes 4–61 of the packet.

**Microcontroller:**  
ESP32-WROOM-S3 (used in schematic)

**Inputs:**
- 3 tactile pushbuttons (TS04-86-50-BK series) for user control
- Button input triggers messages like motor control, mode switching, or temp request which inturn, gives out a waveform like structure. 

**Outputs:**
- OLED display via I2C (feedback, speed, temperature, etc.)
- MCP4725 DAC via I2C (analog waveform output)
- LED debug (message activity)

---

## ✅ Message Type 10 – Button Press (HIM → Microcontroller or Motor)

Button press events are detected on digital GPIO pins and trigger this message to notify the system of user interaction.

| Byte # | Variable Name | Data Type | Min | Max | Example |
|--------|---------------|-----------|-----|-----|---------|
| 1 | message_type | `uint8_t` | 10 | 10 | 10 |
| 2 | button_id | `uint8_t` | 1 | 3 | 2 |
| 3 | action_code | `uint8_t` | 0 | 3 | 1 |

- `button_id`: Which physical button (mapped to GPIO)
- `action_code`: 0 = Pressed, 1 = Held, 2 = Released, etc.

---

## ✅ Message Type 20 – Display Update (Microcontroller → HIM)

Display instructions from the main system to OLED (e.g., speed, temp, status)

| Byte # | Variable Name | Data Type | Min | Max | Example |
|--------|---------------|-----------|-----|-----|---------|
| 1 | message_type | `uint8_t` | 20 | 20 | 20 |
| 2 | display_code | `uint8_t` | 0 | 5 | 3 |
| 3 | value | `int16_t` | -32768 | 32767 | 1200 |

- `display_code`: 0 = Temp, 1 = Motor speed, 2 = Status
- `value`: Signed number to display (e.g., temp = 2350 → 23.5°C)

---

## ✅ Message Type 30 – Motor Command (HIM → Motor Driver)

When user requests a motor action via button press.

| Byte # | Variable Name | Data Type | Min | Max | Example |
|--------|---------------|-----------|-----|-----|---------|
| 1 | message_type | `uint8_t` | 30 | 30 | 30 |
| 2 | motor_id | `uint8_t` | 1 | 2 | 1 |
| 3 | set_speed | `int8_t` | -100 | 100 | 75 |

- `set_speed`: Signed speed command (-100 = full reverse, 100 = full forward)

---

## ✅ Message Type 40 – Temperature Request (HIM → Microcontroller)

User requests current temp for display or logic.

| Byte # | Variable Name | Data Type | Min | Max | Example |
|--------|---------------|-----------|-----|-----|---------|
| 1 | message_type | `uint8_t` | 40 | 40 | 40 |
| 2 | sensor_id | `uint8_t` | 0 | 2 | 1 |

---

## ✅ Message Type 41 – Temperature Reading (Microcontroller → HIM)

Microcontroller returns current temperature reading to HIM for display.

| Byte # | Variable Name | Data Type | Min | Max | Example |
|--------|---------------|-----------|-----|-----|---------|
| 1 | message_type | `uint8_t` | 41 | 41 | 41 |
| 2 | sensor_id | `uint8_t` | 0 | 2 | 1 |
| 3–4 | temp_val | `int16_t` | -32768 | 32767 | 2350 |

- `temp_val`: 2350 → 23.5°C (×100 scaling)

---

### ✅ Notes

- All messages fit inside bytes 4–61 of the class message format.
- The prefix (0xA5), suffix (0x5A), sender, and receiver bytes are handled in the packet structure and not shown in these tables.
- All HIM messages are parsed using UART receive interrupt handlers and decoded based on `message_type`.
- Out-of-range values or incorrect lengths are ignored or NACKed.
- Valid messages trigger visual feedback via the onboard LED (`LED_DEBUG`).

---

