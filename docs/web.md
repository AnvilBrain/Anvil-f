# Anvil-F Web Interface Guide

The Anvil-F Web Interface provides browser-based control over the device and is optimized for both desktop and mobile use.

The interface is intended for visual workflows, quick field diagnostics, and convenient operation without requiring direct CLI interaction.

---

## Overview

When the Web UI is launched, the user is presented with two operating modes (v1.0 beta):

- WiFi Analyzer (experimental)  
- WiFi Pentest  

Each mode is designed for a different workflow.

---

## Operating Modes

### WiFi Analyzer (Experimental)

The Analyzer mode is designed to evaluate the health and quality of a selected access point and surrounding RF conditions.

**Capabilities**

- passive environment analysis  
- signal strength evaluation  
- channel utilization estimation  
- retry rate assessment  
- anomaly detection heuristics  

**Output Metrics**

- RSSI of the target AP  
- channel load estimation  
- retry rate status  
- anomaly indicators, including:
  - weak signal
  - channel congestion
  - deauthentication activity
  - potential evil twin presence
  - ARP spoofing indicators
  - beacon flood detection

**Status**

This feature is currently experimental and results should be treated as advisory.

---

### WiFi Pentest Mode

Pentest mode provides visual access to the core wireless testing features available in the CLI.

This mode is intended for interactive workflows and field testing scenarios.

**Available Functions**

- network scanning (2.4 GHz / 5 GHz)  
- target access point selection  
- client discovery  
- deauthentication operations (all clients or targeted)  
- WPA handshake capture  

The interface is optimized for use on phones, tablets, and desktop browsers.

---

## Wi-Fi Behavior During Operations

Certain operations require the device to switch Wi-Fi modes or enter promiscuous mode.

This affects:

- client scanning  
- handshake capture  
- deauthentication  
- analyzer runs  

---

### What to Expect

During active operations:

- the Anvil-F access point may temporarily disappear  
- the Web UI connection may drop  
- the device will automatically restore the AP after the job completes  
- results will become available after reconnection  

The duration depends on the configured operation time.

---

## Reconnection Behavior

The Web UI attempts to automatically restore session state after reconnecting.

On reload, the interface will try to:

- restore the selected target  
- detect whether a job is running  
- retrieve the last operation results  

In unstable RF environments, a short delay before data appears is normal.

---

## Limitations (v1.0 beta)

- Analyzer relies on heuristic detection  
- AP interruptions during active operations are expected behavior  
- Only one active job is supported at a time  
- Performance depends on ESP32-C5 capabilities  
- Some UI states may briefly desynchronize during reconnect events  

---

## Recommended Workflow

1. Start Web mode using the CLI command `web`  
2. Connect to the Anvil-F access point  
3. Open `http://192.168.4.1` in a browser  
4. Select the desired operating mode  
5. Perform the required operation  
6. Wait for automatic AP recovery if a scan or attack is running  

---

## Future Improvements

Planned enhancements include:

- improved progress visibility  
- stronger reconnect resilience  
- historical analysis storage  
- extended RF telemetry  
- multi-target monitoring  
- enhanced mobile UX