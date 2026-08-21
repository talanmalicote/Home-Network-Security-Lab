# Home Network Security Lab

A fully virtualized security lab used to practice offensive/defensive networking: scripted vulnerability scanning, segmented firewall zones, traffic analysis, and IDS tuning.

## Overview

Built in **VirtualBox/VMware**, this lab simulates a small segmented network to practice:

1. **Reconnaissance** — Python + Nmap automation to scan hosts, enumerate services, and identify exploitable vulnerabilities.
2. **Network segmentation** — pfSense firewall rules and VLAN-style zones.
3. **Traffic analysis** — Wireshark captures to validate firewall behavior during scripted attack simulations.
4. **Intrusion detection** — Snort IDS tuned to detect port scans, brute-force attempts, and abnormal payload signatures.

## Lab topology

```
                 +-------------------+
                 |   pfSense (FW)    |
                 +--------+----------+
                          |
        +-----------------+------------------+
        |                                     |
 +------+-------+                     +-------+------+
 |  Zone: LAN   |                     |  Zone: DMZ   |
 |  (trusted)   |                     | (test hosts) |
 +--------------+                     +--------------+
        |                                     |
   Attacker VM  <---- Snort IDS ---->    Target VMs
   (Kali-style)                        (Metasploitable, etc.)
```

## Repo structure

```
home-network-security-lab/
├── nmap/
│   └── scan_hosts.py              # Python wrapper for automated Nmap scans
├── pfsense/
│   └── firewall-rules-notes.md    # Firewall zone rules and segmentation notes
├── snort/
│   └── local.rules                # Custom Snort detection rules
├── wireshark/
│   └── capture-analysis-notes.md  # Notes/checklist for validating captures
└── README.md
```

## What it detects

- Port scans (TCP SYN/connect scans, aggressive Nmap timing)
- Brute-force login attempts (SSH/RDP/FTP)
- Abnormal payload signatures generated during scripted scans

## Tech stack

VirtualBox · VMware · Python · Nmap · pfSense · Wireshark · Snort · Ubuntu/Linux

