# Smart Bulb Firmware Reverse Engineering

**Reverse engineered the firmware and communication protocol of a commercial Wi-Fi RGB + CCT smart LED bulb (ESP8684-based) to understand its embedded architecture, LED control mechanisms, and cloud connectivity.**

## Project Overview

As part of an embedded systems and reverse engineering lab, I performed a complete hardware-to-protocol analysis on a retail smart bulb (model markings: a5w2, 9W RGB+CCT). The device uses an Espressif ESP8684 (ESP32-C2 RISC-V) microcontroller with 2 MB embedded flash.

### Objectives
- Establish reliable UART communication and enter bootloader mode
- Dump and parse the firmware
- Analyze firmware strings and NVS configuration data
- Map GPIO pins used for LED control (RGB + tunable white)
- Inspect network traffic to characterize the proprietary communication protocol

## Key Results

- Identified the SoC as **ESP8684H** (single-core RISC-V, 120 MHz, 2 MB flash)
- Developed a reliable bootloader entry method (simultaneous grounding of GPIO9/"09" pad and EN pin during power-up)
- Successfully dumped the full 2 MB firmware image using esptool.py
- Extracted the main application partition and generated clean string artifacts for analysis
- **Located the configured WiFi SSID and password** in the device's firmware strings / configuration data
- Determined that LED configuration is stored in NVS under `light_cfg` / `light_cfg_info`
- Identified **GPIO[10]** and **GPIO[7]** as the likely PWM channels for RGB + warm/cool white control (configured as output + open-drain + pull-up)
- Confirmed the bulb uses **MQTT over TLS** to AWS IoT Core (us-west-2 endpoint)
- Analyzed captured traffic with Wireshark, confirming encrypted MQTT sessions and local network behavior (ARP)
- Documented the full workflow including hardware wiring, esptool commands, baud-rate considerations, and analysis steps

## Tools & Technologies

**Hardware & Interfacing**
- SparkFun Serial Basic (CH340)
- Arduino (isolated 3.3 V power rail)
- UART (TX = GPIO20, RX = GPIO19)
- Strapping pins: GPIO9 + EN for ROM bootloader mode

**Software & Analysis**
- esptool.py (firmware read/write)
- Python (binary extraction, string generation, automation)
- Ghidra (initial binary exploration)
- Wireshark (MQTT/TLS traffic analysis)
- Oracle VirtualBox (Lubuntu VM for Linux-native tooling)

**Protocols**
- UART / SPI (internal flash)
- MQTT over TLS
- HTTP (OTA update pattern)

## Hardware Setup

- SparkFun TX → Bulb RX (GPIO20)
- SparkFun RX → Bulb TX (GPIO19)
- Common GND
- 3.3 V supplied from Arduino (never powered the bulb from mains during analysis)
- Bootloader: Ground both the labeled "09" (GPIO9) and "EN" pads while applying power

See `docs/hardware_setup.md` for detailed wiring notes and power-cycle timing that finally produced reliable `boot:0x4 (DOWNLOAD(UART0))` messages.

## Repository Contents

- `docs/` — Detailed technical notes on hardware setup, esptool commands (including the successful 460800 baud dump), firmware/string analysis, and Wireshark findings
- `scripts/` — Python utilities used for app partition extraction and clean string generation
- `images/` — Screenshots of successful esptool sessions and Wireshark MQTT/TLS traffic

## Skills Demonstrated

- Low-level embedded hardware reverse engineering and debugging
- UART strapping pin manipulation and bootloader interaction
- Firmware dumping, parsing, and string-based analysis
- IoT protocol reverse engineering (MQTT over TLS)
- Scripting and automation for binary analysis (Python)
- Professional documentation of technical workflows

This project complements my prior ESP32 embedded work (autonomous bee hive monitoring system) and strengthens my practical experience with real-world IoT device analysis — directly relevant to secure embedded systems, defense, aerospace, and healthcare technology applications.

## Challenges & Solutions

- Initial esptool "Failed to connect" errors → discovered that GPIO9 alone was insufficient; required grounding both GPIO9 and EN simultaneously with precise power-cycle timing
- Flash read corruption at high baud rates during flash read → stepped down to 460800 baud after the stub flasher loaded (initial 74880 baud used for reliable handshake)
- Distinguishing bootloader vs. application boot messages in noisy serial output

## Conclusion

This hands-on project gave me end-to-end experience reverse engineering a commercial IoT device — from physical hardware access through firmware dumping, string analysis, GPIO mapping, WiFi credential recovery, and protocol characterization. It reinforced my interest in embedded systems security and observability of legacy/control interfaces.

---


**Technologies**: ESP8684 • esptool.py • Ghidra • Python • Wireshark • UART/SPI • MQTT/TLS • Oracle VirtualBox
