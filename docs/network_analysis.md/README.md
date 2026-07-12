# Network Traffic Analysis (Wireshark)

## Files Analyzed
- `daybetter_5ace_1.pcap`
- `daybetter_5ace_2.pcap`

## Key Observations

- Bulb IP: `10.42.0.204` (MAC: Espressif_dc:5a:cc)
- Remote server: `54.68.14.206` on TCP port **8883**
- Protocol: **TLSv1.2** (MQTT over TLS)
- DNS query resolved `a3t6ykt1iuvh5v-ats.iot.us-west-2.amazonaws.com` (AWS IoT Core)
- Heavy TLS "Application Data" packets after handshake
- Local ARP traffic present (normal)
- **No unencrypted MQTT topics or payloads** visible
- No HTTP traffic in the captures

## Useful Wireshark Filters
- `tls.port == 8883`
- `dns`
- `arp`

## Conclusion
The bulb uses standard, properly encrypted MQTT over TLS to AWS IoT. No plaintext credentials or commands were visible in the captured traffic.
