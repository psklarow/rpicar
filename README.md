# rpicar
A hardware interface designed to integrate a Raspberry Pi into a vehicle electrical system.  
Provides safe power control, audio I/O, digital automotive signals, CAN bus connectivity, and optional SPI display support.

---

## Features

- Automotive‑grade power management with controlled shutdown
- Power button and ignition‑triggered startup
- Audio output amplifier and dual audio inputs (electret mic + AUX)
- Protected digital I/O for 12 V vehicle signals
- CAN bus interface (MCP2515 + MCP2551, SPI1)
- Optional 1.8" SPI LCD display connector
- Reverse‑gear detection, auxiliary inputs, and 12 V output

---

## Power System

### Power Delivery
- Regulated power for Raspberry Pi
- Supports safe shutdown via GPIO5
- Power button input for manual control

### Power‑On Logic
Triggered by:
- Short press of power button  
**or**
- Ignition key active (`IGN_KEY = 12 V`)

Action:
- Relay **K1** engages → system powers on

### Power‑Off Logic
Triggered by:
- Long press of power button  
**or**
- Raspberry Pi shutdown request (`GPIO5 = HIGH`)

Action:
- Relay **K1** disengages → system powers off

---

## Audio Interfaces

### Audio Output
- Drives external audio amplifier (e.g., navigation voice)
- Input from Raspberry Pi **PWM0 / GPIO12**
- Routed to **EX_AUDIO_IN**

### Audio Inputs
- **Electret microphone input** with preamp + ADC (voice recognition)
- **Auxiliary audio input** routed to output amplifier

### J9 – Audio Input Connector
| Pin | Signal            | Description               |
|-----|-------------------|---------------------------|
| 1   | EX_AUDIO_IN       | Auxiliary audio input     |
| 2   | AUDIO_GND         | Audio ground              |
| 3   | ELECTRET_MIC_IN   | Electret microphone input |

---

## Digital I/O

All inputs include resistor + Zener diode protection.

### J6 – Multi‑Function I/O
| Pin | Signal     | Direction | RPi GPIO | Description |
|-----|------------|-----------|----------|-------------|
| 3   | AUX1_12V   | IN/OUT    | 23 / 24  | Aux input/output (12 V = HIGH) |
| 4   | AUX2_12V   | IN        | 17       | Aux input (12 V = HIGH) |
| 5   | AUX3_12V   | IN        | 4        | Aux input (12 V = HIGH) |
| 2   | REV_LIGHT  | IN        | 22       | Reverse gear signal |
| 1   | 12V+ OUT   | OUT       | —        | 12 V output via 220 Ω |

### J1 – Ignition Input
| Pin | Signal   | Direction | RPi GPIO | Description |
|-----|----------|-----------|----------|-------------|
| 3   | IGN_KEY  | IN        | 9        | Ignition key sense (12 V = HIGH) |

---

## CAN Bus Interface

- **MCP2515** CAN controller (SPI1)
- **MCP2551** CAN transceiver

### J2 – CAN Connector
| Pin | Signal | Description |
|-----|--------|-------------|
| 1   | CANL   | CAN Low     |
| 2   | CANH   | CAN High    |

---

## Optional 1.8" LCD Display

Supports SPI LCD modules such as Waveshare 1.8" LCD.

### J7 – LCD Connector
| Pin | Signal | Description |
|-----|--------|-------------|
| 1   | +3.3V  | Power supply |
| 2   | GND    | Ground |
| 3   | DIN    | SPI data in |
| 4   | CLK    | SPI clock |
| 5   | CS     | Chip select |
| 6   | DS     | Data/command |
| 7   | RST    | Reset |
| 8   | BL     | Backlight control |

---

## System Behavior Summary

- System powers on via ignition or short button press
- Raspberry Pi boots and controls peripherals
- Shutdown occurs via long button press or Raspberry Pi GPIO5 signal
- Relay K1 controls main power state
