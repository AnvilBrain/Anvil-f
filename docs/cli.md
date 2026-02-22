# Anvil-F CLI Guide

The Anvil-F Command Line Interface (CLI) provides direct control over the device via UART.  
It is intended for fast diagnostics, testing workflows, and low-level interaction with the firmware.

All commands are entered in the serial terminal after the device boots.

---

## Command Syntax

```
command [arguments]
```

Where:

- `< >` — required parameter  
- `[ ]` — optional parameter  
- `#` — index from the last scan result  

---

## Command Reference

### help

Show the list of available commands.

```
help
```

---

### scan

Scan for Wi-Fi networks on both 2.4 GHz and 5 GHz bands.

```
scan
```

**Output includes:** SSID, BSSID, channel, RSSI, security type.

---

### scan24

Scan only the 2.4 GHz band.

```
scan24
```

---

### scan5

Scan only the 5 GHz band.

```
scan5
```

---

## Target Selection

### target

Select the target access point from the last scan results.

```
target <#>
```

**Example**

```
target 3
```

**Notes**

- Requires a prior scan  
- The selected target is stored globally  
- Subsequent operations use this target  

---

## Client Operations

### clients

Scan for clients associated with the selected access point.

```
clients [duration]
```

**Example**

```
clients 15
```

**Output includes:** client MAC, RSSI, packet count, hostname (if available).

---

## Deauthentication

### deauth

Send deauthentication frames to the selected access point (all clients).

```
deauth [duration]
```

**Example**

```
deauth 30
```

---

### deauth_client

Send deauthentication frames to a specific client.

```
deauth_client <#> [duration]
```

**Example**

```
deauth_client 2 15
```

**Notes**

- Requires prior `clients` scan  
- More targeted than full AP deauth  

---

## Handshake Capture

### handshake

Attempt to capture the WPA handshake for the specified client.

```
handshake <client#> [duration]
```

**Example**

```
handshake 1 30
```

**Process**

1. Device switches to target channel  
2. Client reconnection is stimulated via deauth  
3. EAPOL frames are captured  
4. Handshake is stored internally  

---

## Network Analysis

### analyze

Run passive access point analysis.

```
analyze [duration]
```

**Example**

```
analyze 30
```

**Metrics include:** signal strength, channel utilization, retry rate, client count, threat indicators (deauth flood, evil twin, ARP spoofing, beacon flood).

---

## System Commands

### info

Display system and runtime information.

```
info
```

**Output includes:** firmware version, chip info, IDF version, MAC address, free heap, uptime, temperature, current target.

---

### clear

Clear the terminal screen.

```
clear
```

---

### download

Transmit the last captured handshake over UART in Base64 format.

```
download
```

**Decoding example (Linux/macOS)**

```
base64 -d handshake.txt > handshake.cap
```

---

### files

List saved files in device storage.

```
files
```

---

### rm

Delete a file from device storage.

```
rm <filename>
```

---

### web

Start the embedded web interface.

```
web
```

**Behavior**

- Device starts its own access point (`Anvil-WiFi` / `12345678`)  
- Web UI available at `http://192.168.4.1`  
- CLI is not available while web mode is active  

---

## Typical Workflow

```
scan
target 2
clients 15
handshake 0 30
download
```

Or for passive analysis:

```
scan
target 2
analyze 60
```

---

## Operational Notes

- A target must be selected before running `clients`, `deauth`, `deauth_client`, `handshake`, or `analyze`.  
- During active operations the device temporarily reconfigures Wi-Fi — the AP may briefly disappear and will recover automatically.  
- Only one active job is supported at a time.  

---

## Known Limitations (v1.0 beta)

- Analyzer uses heuristic detection — accuracy depends on RF environment  
- Concurrent operations are not supported  
- `files` and `rm` are stubs pending storage implementation