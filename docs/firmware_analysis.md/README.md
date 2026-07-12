# Firmware Analysis

## Extraction Process

After dumping `firmware_dump.bin`, we extracted the main application partition:

**Offset**: `0x20000` (131072 bytes)  
**Size**: `0x120000` (1,179,648 bytes)

See `scripts/extract_app_partition.py`

## String Analysis

We generated a clean strings file from the app binary to search for:

- WiFi SSID and password
- URLs and server addresses
- MQTT broker and topics
- Certificate / key material
- GPIO and peripheral configuration
- NVS keys (`light_cfg`, etc.)

**Key findings from strings:**
- WiFi SSID and password were present in the configuration data
- MQTT endpoint: `mqtts://a3t6ykt1iuvh5v-ats.iot.us-west-2.amazonaws.com:8883`
- OTA URL pattern: `http://192.168.%d.%d/p076_ota/p076_firmware.bin`
- LED config stored under NVS key `light_cfg`
- GPIO[10] and GPIO[7] configured as output + open-drain + pull-up

See `docs/key_findings.md` for the full summarized list.
