# nmap-network-recon-lab
Hands-on Nmap network reconnaissance and enumeration lab on Kali Linux, mapped to MITRE ATT&amp;CK with Blue Team / SOC detection analysis
## 📌 Project Overview

This project is a hands-on active network reconnaissance and enumeration lab built to practice host discovery, port scanning, service/version detection, and OS fingerprinting using Nmap on Kali Linux. By analyzing every scan through a Blue Team / SOC Analyst lens, I am bridging offensive scanning techniques with MITRE ATT&CK mapping, log analysis, and defensive hardening.

> **Scope & Authorization:** Local discovery scans were restricted to my isolated VMware virtual subnet (`192.168.133.0/24`). External scans were performed exclusively against `scanme.nmap.org`, the Nmap Project's officially authorized test host.

## 🏗️ Architecture & Tech Stack

**Host Hardware:** Dell Laptop (Intel Core i5, 10th Gen) / Windows OS

**Hypervisor:** VMware Workstation Pro

**Attacker Machine:** Kali Linux (amd64) — `192.168.133.131/24`

**Targets:** Local VMware NAT Subnet (`192.168.133.0/24`) & `scanme.nmap.org` (`45.33.32.156`)

**Tooling:** Nmap 7.99

## 🚀 Lab Progress

- [x] Identified local virtual subnet (`192.168.133.0/24`) via `ip -br a`.
- [x] Executed local host discovery ping sweep (`nmap -sn`), mapping 4 active hosts and VMware MAC OUIs (`00:50:56`) — **MITRE T1018**.
- [x] Ran basic unprivileged TCP Connect port scan (`nmap scanme.nmap.org`) across the top 1,000 common ports — **MITRE T1595.001**.
- [x] Performed service and version enumeration (`nmap -sV`), identifying `OpenSSH 6.6.1p1` and `Apache httpd 2.4.7` on Ubuntu Linux — **MITRE T1595.002**.
- [x] Executed privileged OS fingerprinting (`sudo nmap -O`) to analyze TCP/IP stack responses — **MITRE T1592.004**.
- [x] Documented SOC detection signals and defensive hardening controls (banner minimization, SSH allow-listing, SIEM sweep alerts).
- [ ] Run Nmap scans against my local Windows 11 VM with Sysmon enabled to capture and analyze the resulting Event IDs.

## 💡 Key Learnings

* **VMware & OneDrive Conflicts:** Resolved an *"All pipe instances are busy"* VMware crash by terminating the hung `VMware Workstation VMX` process, clearing stale `.lck` folders, and relocating the VM directory out of OneDrive onto a dedicated local drive (`D:\VMs`).
* **Banner Leakage vs. OS Fingerprinting:** Because scan traffic traversed VMware NAT and a home router, `sudo nmap -O` flagged `test conditions non-ideal` and guessed `Actiontec embedded (91%)`. Cross-correlating this with `nmap -sV` banners (which confirmed Ubuntu Linux) demonstrated why SOC analysts must never rely on a single telemetry source during triage.
* **Defender Visibility:** Unprivileged TCP Connect scans (`-sT`) complete full three-way handshakes that leave clear audit trails in host logs, while unhardened service banners give attackers actionable version intelligence without a single exploit being run.

* ## 📸 Lab Evidence
