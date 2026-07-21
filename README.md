# SOC Home Lab

A hands-on blue-team home lab documenting my work across the core skills of a Security Operations Center (SOC) analyst: **SIEM & log analysis, detection engineering & incident response, network traffic analysis, and threat intelligence**.

Each lab is built in an isolated virtual environment, mapped to the [MITRE ATT&CK](https://attack.mitre.org/) framework where relevant, and written up with the queries, configs, and screenshots that produced the results.

> **Why this repo exists:** to demonstrate practical, reproducible SOC skills — not just tool familiarity, but how I investigate, detect, and document security events.

---

## Lab environment

A segmented, **host-only** virtual network (no internet, no home LAN) built in VirtualBox:

| Machine | Role | IP |
|---------|------|-----|
| Kali Linux | Attacker | 192.168.56.10 |
| Windows 11 Enterprise (eval) | Victim endpoint | 192.168.56.20 |
| Ubuntu Server | SIEM host (Splunk) | 192.168.56.30 |

Full build details in [00-lab-setup](./00-lab-setup/). Network diagram: [`diagrams/`](./diagrams/).

---

## Labs

| # | Lab | Domain | Tools | ATT&CK | Status |
|---|-----|--------|-------|--------|--------|
| 00 | [Home Lab Setup](./00-lab-setup/) | Foundation | VirtualBox, Kali, Windows 11, Ubuntu | — | ✅ Complete |
| 01 | [SIEM with Splunk Free](./01-siem-splunk/) | SIEM | Splunk | — | ✅ Complete |
| 02 | [Windows Log Ingestion](./02-windows-log-ingestion/) | SIEM | Sysmon, Universal Forwarder | — | 🚧 Planned |
| 03 | [Threat Hunting in the SIEM](./03-threat-hunting/) | SIEM / Detection | Splunk (SPL) | T1059, T1087 | 🚧 Planned |
| 04 | [Simulating Attacks with Atomic Red Team](./04-atomic-red-team/) | Detection & IR | Atomic Red Team | Multiple | 🚧 Planned |
| 05 | [Incident Response Writeup](./05-incident-report/) | Detection & IR | — | — | 🚧 Planned |
| 06 | [Network Traffic Analysis](./06-wireshark-analysis/) | Network | Wireshark | — | 🚧 Planned |
| 07 | [Detecting Malicious Traffic](./07-pcap-malicious-traffic/) | Network | Zeek, Suricata, Wireshark | — | 🚧 Planned |
| 08 | [IOC Enrichment & Threat Hunting](./08-ioc-enrichment/) | Threat Intel | VirusTotal, AbuseIPDB, OTX | — | 🚧 Planned |
| 09 | [Static Malware Triage](./09-static-malware-triage/) | Threat Intel | strings, pestudio | — | 🚧 Planned |

---

## Skills demonstrated

- **SIEM administration & log analysis** — Splunk install, data ingestion, SPL search, dashboards
- **Detection engineering** — writing and tuning detections, MITRE ATT&CK mapping
- **Incident response** — investigation, timeline building, IR report writing
- **Network analysis** — packet capture, protocol analysis, IDS (Zeek/Suricata)
- **Threat intelligence** — IOC enrichment, OSINT, static malware triage

---

## Safety & ethics

All activity happens on machines I own inside an isolated, host-only lab with no route to the internet or any production network. No systems were tested without authorization. Malware samples (if any) are never committed to this repo and are only ever analyzed statically or in a disposable, network-isolated sandbox.

---

## About

Built by **Aidan Tracy** as part of preparing for a SOC analyst role. Feedback welcome via issues.
