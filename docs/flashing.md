# Anvil-F Flashing Guide

This document describes how to correctly flash Anvil-F firmware onto an ESP32-C5 device.

The firmware binaries are expected to be located in:

```
Anvil-f/firmware/
```

---

## Requirements

Before flashing, ensure the following:

- Python 3.8 or newer is installed
- `esptool` is installed
- ESP32-C5 board is connected via USB
- Correct serial port is known

Install esptool if needed:

```
pip install esptool
```

---

## Firmware Files

After building the project, the required binaries are located in the `firmware` directory.

Typical layout:

```
firmware/bootloader/bootloader.bin
firmware/partition_table/partition-table.bin
firmware/anvil-f.bin
```

Always use binaries from the same build.

---

## Critical Note for ESP32-C5

ESP32-C5 uses a different bootloader offset than classic ESP32 chips.

**Bootloader offset:** `0x2000`  
**Partition table:** `0x8000`  
**Application:** `0x10000`

Using the wrong bootloader offset will result in `invalid header` errors or boot loops.

---

## Step 1 — Erase Flash (Recommended)

Before the first installation, perform a full chip erase:

```
esptool.py --chip esp32c5 --port COM12 erase_flash
```

Replace `COM12` with your actual serial port.

---

## Step 2 — Flash Firmware

Run the following command from the project root.

### Windows (cmd)

```
esptool.py --chip esp32c5 --port COM12 --baud 460800 write_flash ^
  0x2000 firmware/bootloader/bootloader.bin ^
  0x8000 firmware/partition_table/partition-table.bin ^
  0x10000 firmware/anvil-f.bin
```

### Linux / macOS

```
esptool.py --chip esp32c5 --port /dev/ttyUSB0 --baud 460800 write_flash \
  0x2000 firmware/bootloader/bootloader.bin \
  0x8000 firmware/partition_table/partition-table.bin \
  0x10000 firmware/anvil-f.bin
```

---

## Step 3 — Verify Boot

After flashing, open a serial monitor at 115200 baud.

A successful boot should show:

- ESP-ROM banner
- bootloader start
- Anvil-F initialization logs

If the device repeatedly prints `invalid header`, check:

- bootloader offset is `0x2000`
- correct binaries are used
- flash was erased before programming

---

## Troubleshooting

### Device not detected

- correct COM/TTY port
- USB cable supports data
- drivers are installed
- board is powered

---

## Recommended Best Practices

- Always erase flash before first install
- Always use binaries from the same build
- Do not change offsets unless you know what you are doing