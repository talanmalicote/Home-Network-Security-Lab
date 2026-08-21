# pfSense Firewall Rules & Segmentation Notes

## Zones

| Zone | Interface | Subnet | Purpose |
|---|---|---|---|
| LAN | em0 | 192.168.56.0/24 | Trusted management/attacker workstation |
| DMZ | em1 | 192.168.57.0/24 | Target/vulnerable test VMs |
| WAN | em2 | DHCP (host-only/NAT) | Internet access for updates only |

## Rule set (summary)

### LAN → DMZ
- Allow TCP 22 (SSH) from LAN management host only, for lab administration.
- Allow ICMP echo for connectivity testing.
- Deny all other LAN → DMZ traffic by default (deny-by-default posture).

### DMZ → LAN
- Deny all by default. DMZ hosts should never reach the LAN zone directly —
  this is the rule scripted attacks are used to validate ("did the target
  VM manage to pivot back into LAN?").

### DMZ → WAN
- Deny all outbound by default, to prevent a compromised target VM from
  reaching out (simulates a real segmentation boundary / egress filtering).

### LAN → WAN
- Allow outbound HTTP/HTTPS/DNS for updates and tool downloads.

## Logging

- Enable logging on all deny rules for DMZ ↔ LAN traffic — these logs are
  what get validated against Wireshark captures during scripted attack runs.
- Firewall logs exported and cross-referenced with `snort/local.rules` alerts.

## Validation process

1. Apply rule set above.
2. Run `nmap/scan_hosts.py` from the DMZ against LAN to confirm it is blocked.
3. Run `nmap/scan_hosts.py` from LAN against DMZ to confirm expected access.
4. Capture traffic with Wireshark on the pfSense span/mirror port during each
   test (see `wireshark/capture-analysis-notes.md`).
5. Confirm Snort raises alerts for the scan traffic (see `snort/local.rules`).
