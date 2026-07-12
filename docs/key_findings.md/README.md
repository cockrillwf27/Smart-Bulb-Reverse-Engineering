# Key Findings Summary

## Hardware / Chip
- SoC: ESP8684H (ESP32-C2)
- Flash: 2 MB embedded
- Bootloader: Requires GPIO9 + EN grounded

## WiFi Credentials
- WiFi SSID and password were found in the firmware configuration data

## LED Control
- Likely PWM pins: **GPIO[10]** and **GPIO[7]**
- Configuration: Output + Open-Drain + Pull-up
- Stored in NVS under key `light_cfg`

## Communication
- Protocol: **MQTT over TLS**
- Broker: `a3t6ykt1iuvh5v-ats.iot.us-west-2.amazonaws.com:8883` (AWS IoT Core)
- No hardcoded WiFi credentials in code (stored in NVS)
- OTA update pattern uses local network HTTP

## Security Observations
- Communication is encrypted (TLS)
- Device is cloud-dependent (AWS IoT)
- Configuration stored in NVS (not hardcoded in binary)
