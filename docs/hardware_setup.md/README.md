# Hardware Setup & Wiring

## Required Hardware
- SparkFun Serial Basic (CH340 USB-to-UART adapter)
- Arduino Uno (used only as a safe 3.3V power source)
- Jumper wires
- The smart bulb (ESP8684-based)

## Pin Connections

| SparkFun / Arduino | Bulb Test Point     | Notes                              |
|--------------------|---------------------|------------------------------------|
| SparkFun TX        | Bulb RX (GPIO20)    | Main UART transmit                 |
| SparkFun RX        | Bulb TX (GPIO19)    | Main UART receive                  |
| GND                | GND                 | Common ground (very important)     |
| Arduino 3.3V       | Bulb VCC / 3.3V     | Power only — never use mains power |
| -                  | GPIO9 ("09" pad)    | Ground for bootloader mode         |
| -                  | EN (Reset)          | Ground together with GPIO9         |

<p align="left">
  <img src="images/PCBPinConnections.jpg" width="200">
</p>


## Bootloader Entry Procedure (What Finally Worked)

1. Connect all wires as shown above.
2. Ground **both** the "09" (GPIO9) pad **and** the EN pad.
3. Unplug the Arduino USB power for **8 seconds**.
4. Plug the Arduino USB back in while keeping both pins grounded.
5. You should see in the serial monitor:
ESP-ROM:esp32c2-eco4-20240515
...
boot:0x4 (DOWNLOAD(UART0))
waiting for download
6. Release the ground connections after you see the "waiting for download" message.

## Important Notes
- Never power the bulb from 120V AC while working on it.
- Use the Arduino only as a 3.3V regulated supply.
- The combination of grounding **both** GPIO9 and EN was critical — GPIO9 alone was not enough on this board.
