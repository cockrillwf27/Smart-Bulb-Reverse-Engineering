#!/usr/bin/env python3
"""
Generate a clean list of printable strings from app.bin
"""

import re

INPUT_FILE = "app.bin"
OUTPUT_FILE = "app_strings.txt"

with open(INPUT_FILE, "rb") as f:
    data = f.read()

# Find sequences of 4+ printable ASCII characters
strings = re.findall(rb"[\x20-\x7E]{4,}", data)

with open(OUTPUT_FILE, "w", encoding="utf-8", errors="ignore") as out:
    for s in strings:
        out.write(s.decode("utf-8", errors="ignore") + "\n")

print(f"✅ {OUTPUT_FILE} created with {len(strings)} strings")
