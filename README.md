# Blended Penetration Testing: Web & Wireless Security Lab

> A comprehensive penetration testing exercise evaluating both web application
> and wireless network security in a controlled lab environment.
> Simulates real-world attack chains spanning application, network, and 
> physical hardware layers.

---

## Overview

| Detail | Info |
|---|---|
| Vulnerabilities Found | 9 across web and wireless layers |
| Web Testing Tools | Burp Suite, OWASP ZAP, Nikto, Curl |
| Network & Wireless | Wireshark, Aircrack-ng, Suricata IDS 7.0 |
| Hardware Security | Flipper Zero (RFID, NFC, IR replay) |
| Target Application | OWASP Juice Shop (Node.js) |
| OS Environment | Kali Linux 2024.2 / Windows 10 Pro 22H2 |
| Deliverables | Technical report, executive summary, PoC evidence |

---

## Project Scope

This lab simulates a real-world blended penetration test where attackers 
exploit weaknesses across multiple layers — from web application vulnerabilities 
to wireless network attacks to physical hardware security.

Three distinct assessment areas were covered:

- **Web application security** — manual and automated testing against a 
  deliberately vulnerable target
- **Wireless network security** — WPA2 handshake capture and cracking 
  demonstration
- **Hardware security** — RFID, NFC, and IR attack surface testing using 
  a Flipper Zero

---

## Web Application Assessment

**Target:** OWASP Juice Shop (deliberately vulnerable Node.js web application)  
**Host OS:** Windows 10 Pro 22H2  
**Testing OS:** Kali Linux 2024.2

**Methodology:** Automated scanning with OWASP ZAP and Nikto combined with 
manual interception and exploitation using Burp Suite. All findings manually 
verified before documentation.

### Vulnerabilities Identified

| # | Vulnerability | Severity | Verified |
|---|---|---|---|
| 1 | SQL Injection (SQLi) | Critical | PoC evidence |
| 2 | Cross-Site Scripting (XSS) | High | PoC evidence |
| 3 | Broken Authentication | High | PoC evidence |
| 4 | Insecure Direct Object Reference (IDOR) | High | PoC evidence |
| 5 | Open Redirect | Medium | PoC evidence |

Each finding includes annotated screenshots, Burp Suite request/response 
captures, and remediation recommendations.

---

## Wireless Network Assessment

**Tools:** Wireshark 4.2.0, Aircrack-ng Suite 1.7  
**Scope:** WPA2-protected network (controlled lab environment)

- Captured WPA2 4-way handshake using monitor mode on wlan0
- Demonstrated dictionary attack risk against weak passphrases using Aircrack-ng
- Inspected ARP traffic and packet metadata using Wireshark
- Documented pivot potential from compromised wireless access to internal 
  web targets

---

## Intrusion Detection — Suricata IDS

**Tool:** Suricata IDS 7.0.0  
**Outputs:** Eve.json, Fast.log

- Deployed Suricata IDS alongside the test environment to detect attack traffic 
  in real time
- Wrote custom detection rules targeting SQLi patterns, XSS payloads, and 
  brute-force authentication attempts
- Analysed structured alert logs (Eve.json) and fast alert logs (Fast.log) 
  for post-exploitation review
- Demonstrated how a blue team defender would detect the exact attacks 
  performed during the web assessment

> This component reflects both offensive and defensive perspectives — 
> understanding how attacks are detected is as important as executing them.

---

## Hardware Security Assessment

**Hardware:** Flipper Zero (Firmware 0.95+), HID 125kHz Reader, 
IR Transmitter/Receiver

- Scanned and read low-frequency RFID card data (Facility Code & Card ID) 
  using HID 125kHz reader
- Demonstrated NFC card interaction and data capture
- Captured and replayed IR remote-control signals using built-in 
  IR transmitter/receiver
- Documented physical-layer attack vectors and how hardware weaknesses 
  can be exploited to bypass access controls or pivot into internal systems

---

## Deliverables

- **Technical penetration testing report** — full methodology, findings, 
  severity ratings, and remediation recommendations
- **Executive summary** — business impact written for non-technical 
  stakeholders
- **Annotated PoC evidence** — Burp Suite captures, ZAP scan results, 
  Suricata alert logs, Wireshark packet captures, Flipper Zero outputs
- **Custom Suricata rules** — detection rules written to identify attack 
  patterns observed during testing
- **Live demo** — safe exploit demonstrations with documented business impact

---

## Key Takeaways

- Automated scanners (ZAP, Nikto) flagged known issues but missed 
  logic-based vulnerabilities — manual Burp Suite testing was essential 
  for complete coverage
- WPA2 networks remain vulnerable to offline dictionary attacks when 
  weak passphrases are used, even with modern encryption
- Physical-layer tools like the Flipper Zero expose attack surfaces 
  that are routinely overlooked in standard assessments
- Deploying Suricata IDS alongside offensive testing provided direct 
  insight into how defenders detect and respond to the same attacks — 
  reinforcing the value of a blended red/blue approach
- Real-world attackers chain multiple layers — web, wireless, and 
  physical — and a complete security assessment must do the same

---

## Tools & Technologies

**Web Application Testing**  
`Burp Suite 2023.12` `OWASP ZAP 2.14.0` `Nikto 2.5.0` `Curl 7.88.1` 
`Mozilla Firefox 120+`

**Network & Wireless**  
`Wireshark 4.2.0` `Aircrack-ng 1.7` `Linux Monitor Mode Drivers`

**Intrusion Detection**  
`Suricata IDS 7.0.0` `Eve.json` `Fast.log`

**Hardware Security**  
`Flipper Zero (Firmware 0.95+)` `HID 125kHz Reader` 
`IR Transmitter/Receiver`

**Environment**  
`Kali Linux 2024.2` `Windows 10 Pro 22H2` `OWASP Juice Shop (Node.js)`

**Documentation & Dev**  
`VS Code` `Nano` `Git` `GitHub`

---

## Disclaimer

All testing was conducted in a controlled lab environment for educational 
purposes only. No real systems, networks, devices, or individuals were 
targeted without explicit permission. This project follows responsible 
disclosure principles and ethical penetration testing guidelines.
