
# WiFi Fucker Pro

**WiFi Fucker Pro** is a professional WiFi penetration testing framework
designed for red team operations. It integrates AI-driven password generation,
multi-channel parallel scanning, parallel handshake capture, WPS attacks,
PMKID harvesting, Evil Twin deployment, and automated hash cracking into a
single interactive console.

Developer: **@DarkWebist**

---

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Main Menu Options](#main-menu-options)
- [Command-Line Arguments](#command-line-arguments)
- [File Structure](#file-structure)
- [Logging and Database](#logging-and-database)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## Features

| Category | Capabilities |
|---|---|
| **Scanning** | Multi-channel parallel scanning across 2.4 GHz and 5 GHz bands |
| **Handshake Capture** | Multi-target parallel WPA/WPA2 handshake capture with deauthentication |
| **PMKID Attack** | PMKID harvesting for WPA2/WPA3 networks using hcxdumptool |
| **WPS Attack** | Pixie Dust attack and PIN brute-force via Reaver |
| **Password Cracking** | AI-based smart wordlist generation + Hashcat (GPU) / Aircrack-ng |
| **Evil Twin** | Fake access point with Captive Portal support |
| **Deauth DoS** | Deauthentication attack against all visible networks |
| **MAC Spoofing** | Anonymity via MAC address randomization |
| **Network Mapping** | Client tracking and network topology mapping |
| **Database** | SQLite storage of all discovered networks and attack history |
| **Reporting** | JSON/CSV export of attack results |

---

## Requirements

### Hardware

- A wireless network adapter that supports **monitor mode** and **packet
  injection** (e.g., Alfa AWUS036ACH, Alfa AWUS036NHA, TP-Link TL-WN722N v1).

### Software

| Package | Purpose |
|---|---|
| `python3` (3.8+) | Runtime |
| `aircrack-ng` | Scanning, handshake capture, deauth, basic cracking |
| `hashcat` | GPU-accelerated hash cracking |
| `reaver` | WPS Pixie Dust and PIN brute-force |
| `hcxdumptool` | PMKID capture |
| `hcxpcapngtool` | PMKID hash conversion |
| `hostapd` | Evil Twin AP creation |
| `dnsmasq` | DHCP/DNS for Evil Twin |

All Python dependencies are from the standard library. No `pip install`
required.

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/darkwebist/WI-FI_Fucker.git
cd WI-FI_Fucker
```

2. Install system dependencies (Kali / Debian / Ubuntu)

```bash
sudo apt update
sudo apt install -y aircrack-ng hashcat reaver hcxdumptool hcxtools hostapd dnsmasq
```

3. Verify your wireless adapter

```bash
sudo airmon-ng
```

Make sure your adapter is listed and supports monitor mode.

---

Usage

Run the script with root privileges:

```bash
sudo python3 wififucker_pro.py
```

Interactive Mode

Once launched, you will see a professional terminal menu:

```
╔══════════════════════════════════════════════════════════════════════════════╗
║  MAIN ATTACK VECTORS                                                        ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  [1]  Full Auto Attack (Scan → Capture → Crack)                             ║
║  [2]  Multi-Target Parallel Handshake Capture                               ║
║  [3]  PMKID Attack (WPA2/WPA3)                                              ║
║  [4]  WPA3 Downgrade + Capture                                              ║
║  [5]  AI Password Cracking (Hashcat + Smart Wordlist)                       ║
║  [6]  WPS Attack (Pixie Dust + PIN Bruteforce)                              ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  ADDITIONAL ATTACKS                                                         ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  [7]  Evil Twin Professional (Fake AP + Captive Portal)                     ║
║  [8]  Deauth DoS (All Networks)                                             ║
║  [9]  MAC Spoofing (Anonymity)                                              ║
║  [10] Network Mapping & Client Tracking                                     ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  DATABASE & REPORTS                                                         ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  [11] Database (All Found Networks)                                         ║
║  [12] Generate JSON/CSV Report                                              ║
║  [0]  Exit                                                                  ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

Type the number of the desired operation and press Enter.

---

Main Menu Options

[1] Full Auto Attack

Executes the complete attack chain automatically:

1. Scans all channels in parallel.
2. Selects WPA/WPA2 targets.
3. Captures handshakes from all targets simultaneously.
4. Generates an AI-based smart wordlist for each target.
5. Cracks passwords using Hashcat (or Aircrack-ng as fallback).
6. Saves results to the database.

[2] Multi-Target Parallel Handshake Capture

Scans for networks, displays them, and allows you to select multiple targets
(comma-separated indices). Captures WPA handshakes from all selected targets
in parallel using deauthentication.

[3] PMKID Attack

Captures PMKID hashes from WPA2/WPA3 networks using hcxdumptool, converts
them to Hashcat format with hcxpcapngtool, and optionally cracks them with
an AI-generated wordlist.

[4] WPA3 Downgrade + Capture

Forces WPA3 transition mode networks to downgrade to WPA2 and captures the
handshake. (Placeholder — extendable.)

[5] AI Password Cracking

Takes a hash/cap file and a network name as input, generates a smart
wordlist based on the ESSID, and cracks the hash with Hashcat (or
Aircrack-ng).

[6] WPS Attack

Performs a Pixie Dust attack first. If successful, retrieves the WPA
passphrase using the recovered PIN. Falls back to PIN brute-force if
needed.

[7] Evil Twin Professional

Creates a rogue access point with the same ESSID as the target and launches
a Captive Portal via hostapd + dnsmasq. Configuration files must be
placed at /tmp/evil_twin.conf and /tmp/evil_dnsmasq.conf.

[8] Deauth DoS

Sends deauthentication packets to all visible access points, disconnecting
all clients.

[9] MAC Spoofing

Randomizes the MAC address of the wireless interface for anonymity.

[10] Network Mapping & Client Tracking

Maps the local wireless environment and tracks associated clients per
access point.

[11] Database

Displays all networks stored in the SQLite database, including previously
cracked passwords.

[12] JSON/CSV Report

Exports attack results to a structured JSON or CSV file for reporting.

---

Command-Line Arguments

```bash
sudo python3 wififucker_pro.py [OPTIONS]
```

Argument Description
--stealth Enable stealth mode (reduces logging verbosity)
--auto Run in fully automatic mode
--threads N Set the number of threads (default: CPU cores × 4)
-o, --output FILE Save results to a JSON file

Examples

```bash
# Stealth mode with custom thread count and output file
sudo python3 wififucker_pro.py --stealth --threads 32 -o results.json

# Fully automatic mode
sudo python3 wififucker_pro.py --auto
```

---

File Structure

```
WI-FI_Fucker/
├── wififucker_pro.py    # Main framework script
├── README.md            # This file
```

Runtime Files

Path Purpose
/tmp/chan_*.csv Per-channel scan results
/tmp/hs_*.cap Captured handshake files
/tmp/pmkid_*.pcapng Raw PMKID captures
/tmp/pmkid_*.hc22000 Converted PMKID hashes (Hashcat format)
/tmp/ai_wordlist_*.txt AI-generated wordlists
/tmp/wififucker_pro.db SQLite database
/var/log/wififucker_pro.log Activity log

Clean up temporary files when done:

```bash
sudo rm -f /tmp/chan_*.csv /tmp/hs_*.cap /tmp/pmkid_* /tmp/ai_wordlist_* /tmp/wififucker_pro.db
```

---

Logging and Database

Logging

All activity is logged to /var/log/wififucker_pro.log with timestamps
and severity levels (DEBUG, INFO, WARNING, ERROR, CRITICAL).

Database

The SQLite database (/tmp/wififucker_pro.db) contains two tables:

· networks — all discovered access points with BSSID, ESSID, channel,
  encryption, signal strength, clients, handshake/PMKID/WPS status, and
  cracked password.
· attacks — attack history with method, success/failure, password,
  duration, and timestamp.

---

Disclaimer

This tool is intended exclusively for authorized security assessments,
penetration testing on networks you own, or educational research in
controlled lab environments.

Unauthorized access to computer networks is illegal. The developer
(@DarkWebist) assumes no liability for misuse or damage caused by this
software. Use at your own risk and only on systems you are explicitly
authorized to test.

---

License

This project is provided as-is for educational and research purposes. No
formal license is applied. All rights reserved by the developer.

```

