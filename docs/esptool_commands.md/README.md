# esptool.py Commands Used
All commands were run with:

python -m esptool

## Chip identification (first successful connection)
python -m esptool --chip esp32c2 --port COM3 --baud 74880 --before no-reset --connect-attempts 100 chip-id

## Flash Identification
python -m esptool --chip esp32c2 --port COM3 --baud 74880 --before no-reset flash-id

## Full Firmware Dump (2MB) - Final Working Command
python -m esptool --chip esp32c2 --port COM3 --baud 460800 --before no-reset read-flash 0x0 0x200000 firmware_dump.bin

## Baud Rate Notes
- 74880 baud was used for initial connection and small commands (chip-id, flash-id). This is the ROM bootloader’s default speed.
- 460800 baud was used for the large firmware dump after the stub flasher loaded into RAM. Higher speeds (921600 baud) caused data corruption on this hardware.
- The --before no-reset flag was required because we manually controlled the reset/EN pin during power-up.

