#!/usr/bin/env python3
"""
Extract the main application partition from firmware_dump.bin
Offset: 0x20000 (131072 bytes)
Size:   0x120000 (1,179,648 bytes)
"""

INPUT_FILE = "firmware_dump.bin"
OUTPUT_FILE = "app.bin"
OFFSET = 0x20000
SIZE = 0x120000

with open(INPUT_FILE, "rb") as f:
    f.seek(OFFSET)
    data = f.read(SIZE)

with open(OUTPUT_FILE, "wb") as out:
    out.write(data)

print(f" {OUTPUT_FILE} created successfully")
print(f"   Extracted {SIZE} bytes from offset {OFFSET}")
