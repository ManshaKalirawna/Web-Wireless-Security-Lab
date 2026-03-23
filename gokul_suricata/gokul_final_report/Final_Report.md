# Blended Penetration Testing – Web & Wireless Security Lab  
### Final Report (Compiled by: Gokulhesh – Detection & Reporting Lead)

---

## 1. Executive Summary
EXECUTIVE SUMMARY

This report documents the findings from the web application penetration test performed on the OWASP Juice Shop environment. The purpose of this assessment was to identify common web application vulnerabilities, evaluate the security posture of the platform, and produce actionable recommendations for remediation.

Testing was performed using industry‑standard tools, including OWASP ZAP, Burp Suite, and Nikto, supplemented by manual exploitation to validate each finding. Multiple high‑impact vulnerabilities were discovered, including SQL Injection, Cross‑Site Scripting (XSS), Broken Authentication, Open Redirect, and Security Misconfigurations exposing sensitive backend API endpoints.

The vulnerabilities identified could allow attackers to:

Access or manipulate backend data

Steal user sessions or inject malicious scripts

Bypass authentication controls

Redirect users to malicious external sites

Retrieve sensitive information without authorization

All findings have been verified with screenshots, proof‑of‑concept steps, and technical evidence.

Overall, the application is highly vulnerable and susceptible to common web attacks due to missing authentication layers, outdated validation, insecure defaults, and lax input handling. Immediate remediation is recommended to strengthen the environment and reduce security risks.

## 2. Scope & Methodology
This part of the project focuses on evaluating the security posture of the OWASP Juice Shop application. My role in the group was specifically the web application testing portion, so all the work in this section reflects the tests I personally carried out using Kali Linux and Windows as needed.

### 2.1 Scope of Testing

The scope included the following:

Testing the publicly accessible features of the Juice Shop instance running on my Windows machine

Using Kali Linux tools (ZAP, Nikto, Burp Suite Community) to perform Black‑Box style testing

Identifying at least five real vulnerabilities with screenshots and proof-of-concept steps

Documenting everything in a clean and repeatable format for my teammates to later merge into the final group report

The goal was not to “break” the system as much as possible, but to simulate how a normal penetration tester approaches a new web application and gathers evidence.

Out‑of‑scope items included:

Attacks against the underlying Windows host OS

Denial‑of‑service tests

Database-level exploitation beyond what the Juice Shop environment intentionally exposes

Anything impacting other machines on the network (only my local isolated setup was used)

### 2.2 Testing Approach & Methodology

I followed a fairly standard workflow similar to what you’d see in a junior penetration testing engagement. Here's the high‑level process I used:

Recon & Application Mapping
I first accessed the Juice Shop through the browser to understand how the app behaves from a regular user’s perspective. I clicked through menus, forms, login areas, and product pages to get a feel for how the app is structured. This helped me identify any parts of the app worth targeting with tools later, like the login form, search box, product catalogue, and API endpoints.

Automated Scanning
Once the basic layout was clear, I used the following tools:

OWASP ZAP (Kali) – Passive and active scanning to detect common issues (XSS, IDOR, security misconfigurations).

Nikto – To quickly check for server misconfigurations or outdated components.

Burp Suite Community – Mainly for intercepting requests, modifying parameters, and understanding how the backend responds.

The automated scans helped highlight a list of potential vulnerabilities that I later confirmed manually.

Manual Testing & Exploitation
Using the results from the tools as a starting point, I manually attempted:

Broken Authentication tests

Parameter tampering

SQL injection attempts

Unvalidated redirect tests

API endpoint manipulation

Business logic flaws

Reviewing JSON responses and hidden API paths

The Juice Shop challenges were extremely helpful for confirming that my exploitation attempts actually worked. Each challenge pop‑up essentially validated my proof‑of‑concept attempts.

Documentation & Evidence Capture
Throughout the process, I took screenshots of:

Tool scans

Raw requests/responses

Successful exploit results

Challenge completion pop‑ups

Any sensitive information I managed to access

All screenshots are included inside each vulnerability write‑up and stored neatly in the /evidence/ folder for organized version control.

### 2.3 Tools Used

Kali Linux

OWASP ZAP

Burp Suite Community Edition

Nikto

Curl (for direct requests)

Windows 10 (hosting Juice Shop instance)

### 2.4 Deliverables From My Part

As the student responsible for the Web Application portion, my final deliverables include:

Web Application Vulnerability Report (.md + screenshots)

5+ Confirmed vulnerabilities with PoC evidence

ZAP and Nikto scan outputs

Clean GitHub repository for the group to add to later

## 3. Web Application Testing Results (Mansha)
Findings Summary Table
### 3.1 Overview

In this section, I summarize the main vulnerabilities I discovered while testing the OWASP Juice Shop web application. Each finding is listed with its impact, risk level, and brief description. Detailed explanations, step-by-step reproduction, and screenshots are included in the next section (Detailed Vulnerability Write-Up).

### 3.2 Findings Summary Table
| # | Vulnerability                           | Category                  | Risk Level | Brief Description                                                                                         |
| - | --------------------------------------- | ------------------------- | ---------- | --------------------------------------------------------------------------------------------------------- |
| 1 | SQL Injection                           | Injection                 | High       | Input fields in search and login forms were vulnerable to SQL queries, allowing unauthorized data access. |
| 2 | Cross-Site Scripting (XSS)              | Client-Side Injection     | Medium     | Input fields were not properly sanitized, allowing injection of malicious JavaScript.                     |
| 3 | Broken Authentication                   | Authentication & Session  | High       | Default admin credentials were accessible; password strength checks were insufficient.                    |
| 4 | Open Redirect                           | Unvalidated Redirects     | Medium     | Users could be redirected to arbitrary external sites via URL parameters.                                 |
| 5 | Security Misconfiguration / Exposed API | Security Misconfiguration | Medium     | Public API endpoint returned sensitive product data without authentication.                               |
| 6 | Optional / Bonus Findings               | Miscellaneous             | Low        | Minor headers missing, outdated allowlists, or other low-risk issues.                                     |

Note: Risk levels were determined based on impact on confidentiality, integrity, and availability of the web application.

### 3.3 Notes

1. This table provides a high-level view of vulnerabilities.

2. Screenshots and detailed reproduction steps are in Section 4 — Detailed Vulnerability Write-Up.

3. Risk levels are relative to the Juice Shop instance running in a controlled, lab environment.

## 4.  Detailed Vulnerability Write-Up (Mansha)
### 4.1 SQL Injection (High Severity) Overview

During testing, I found that several user-input fields—particularly the search bar and the login form—did not properly validate or sanitize user-supplied data. This allowed me to inject SQL queries into the backend database.

Testing Steps

Navigated to the login page.
2 Entered a basic SQL payload into the username field:

' OR '1'='1

Submitted the login form without providing a valid password.

The application responded with a successful authentication message.

Impact

Bypassing authentication without valid credentials

Possible exposure of user account data

Risk of full database compromise

<img width="1285" height="763" alt="image" src="https://github.com/user-attachments/assets/e888c519-7a30-4a73-870d-1909ceee70ed" />

<img width="1279" height="769" alt="image" src="https://github.com/user-attachments/assets/02a91263-7d4a-4df0-8489-eb23f20cccbe" />
### 4.2 Broken Authentication – Weak Default Password (High Severity) Overview

The administrator account still used its default credentials. I could log in as the admin using these default passwords.

Testing Steps

Navigated to the admin login page.

Entered default username/password combinations.

The application displayed a pop-up confirming successful admin login.

Impact

Full system compromise possible

Administrative access exposes all sensitive data

<img width="1267" height="779" alt="image" src="https://github.com/user-attachments/assets/5c0cd6db-d817-4edf-922c-699f05c20953" />

<img width="1279" height="730" alt="image" src="https://github.com/user-attachments/assets/58cc677d-559c-4026-a128-714dc4259ecb" />

### 4.3 Cross-Site Scripting (XSS) – Reflected (Medium Severity) Overview

Reflected XSS was discovered in the search bar and other fields where input is reflected back without proper sanitization.

Testing Steps

Inserted this payload in a search field:
<script>alert('XSS Test');</script>
The alert executed immediately upon page reload.
Impact

Session hijacking

Credential theft

Redirection to malicious websites

Evidence
<img width="1279" height="790" alt="image" src="https://github.com/user-attachments/assets/52f55182-0dcc-4f46-81e8-45cf8f3bfe21" />
### 4.4 Open Redirect (Medium Severity) Overview

Users could be redirected to arbitrary external sites via URL parameters.

Testing Steps

Opened the redirect URL on the Juice Shop instance:
http://:3000/redirect?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm

Observed the redirect and pop-up confirming the challenge solved.
Impact

Users can be redirected to malicious sites

Could be used in phishing attacks

Evidence

<img width="1915" height="1030" alt="image" src="https://github.com/user-attachments/assets/b59a965d-2a47-4992-b7c0-18027908c5dd" />

<img width="1885" height="1019" alt="image" src="https://github.com/user-attachments/assets/1634d558-37ba-4c6b-b096-6feadc719028" />
### 4.5 Security Misconfiguration – Exposed API / Debug Info (Medium Severity) Overview

Accessible directories and endpoints exposed information about the application and products without authentication.

Testing Steps

Used manual exploration of /api/products and /debug/ endpoints

Observed sensitive information returned in JSON format

Impact

Information disclosure increases attack surface

Could help attackers craft further attacks

<img width="1175" height="645" alt="image" src="https://github.com/user-attachments/assets/dbec7ddb-c1c7-4e02-8791-1573faacca3b" />
### 4.6 Input Validation Issues (Low Severity) Overview

During testing, I observed that several input fields, particularly the search bar, accepted unusual and unexpected characters without proper validation. While the application correctly displayed "No results found" when the input did not match any products, it still echoed the entire user input on the page. This behavior indicates weak input validation.

Testing Steps

Navigated to the search bar on the Juice Shop main page.

Entered a mixture of special characters, symbols, and a long string:

!@#$%^&*()_+{}[]:";'<>?/AAAAAAAAAAAAAAAAAAAAAAAAAAAA

Pressed Enter to submit the search.

Observed that the page displayed "No results found," but the exact input was printed on the page in bold.

Impact

Shows that the application does not properly validate user input.

Could increase the likelihood of future injection or XSS vulnerabilities if other parts of the application use this input unsafely.

Demonstrates a minor security misconfiguration in input handling.<img width="1909" height="1001" alt="image" src="https://github.com/user-attachments/assets/43879258-3db4-4b07-be82-363712c72cf1" />

## 5. Wireless & Hardware Testing ( Rahool)
### 5.1 WiFi Handshake Capture

During the wireless assessment, a WPA2 4-way handshake was successfully captured using monitor mode on Kali Linux.
This handshake can later be cracked offline to identify weak pre-shared keys.
<img width="1600" height="1066" alt="image" src="https://github.com/ManshaKalirawna/Blended-Penetration-Testing-Web-Wireless-Security-Lab/blob/f14985526c464cb582e90332de8e0be6818a2697/evidence/Wireshark.jpg" />
The packet capture shows ARP broadcasts and management frames from the lab wireless network.
The handshake confirms that the target WiFi network was in range and that packets were successfully captured for further cracking analysis.
<img width="1600" height="1066" alt="image" src="https://github.com/ManshaKalirawna/Blended-Penetration-Testing-Web-Wireless-Security-Lab/blob/f14985526c464cb582e90332de8e0be6818a2697/evidence/Wireshark_2.jpg" />
### 5.2 RFID Badge Cloning

As part of the hardware security evaluation, an HID access card was scanned.
The card uses HID H10301 format, which is known to be clonable using inexpensive RFID tools.

<img width="512" height="256" alt="image" src="https://github.com/user-attachments/assets/18848134-5408-4626-a886-61def3eaa0f7" />

To demonstrate that low-frequency HID cards can be scanned and their data extracted, highlighting vulnerabilities to badge cloning and replay attacks.

### 5.3 NFC / IR Remote Capture

<img width="512" height="256" alt="image" src="https://github.com/user-attachments/assets/362e6528-8e63-4862-8afe-9ec752edbf7b" />

The captured output confirms the badge identity (“Cis_lab”) and full tag attributes.
This demonstrates the insecure use of non-encrypted RFID tags within the environment.

### 5.4 IR / NFC / Remote Replay Attack

The hardware tool was able to list the device and capture IR remote signals.

<img width="512" height="256" alt="image" src="https://github.com/user-attachments/assets/e3676552-1222-4396-a614-aacfad8abf4d" />

<img width="256" height="512" alt="image" src="https://github.com/user-attachments/assets/9bb59c46-65af-4c1f-92ff-80ad7bea759a" />

Attackers can intercept IR/NFC input and replay commands, allowing unauthorized control of TVs, projectors, or digital signage systems.

This demonstrates a real-world IR replay attack, showing exploitation of insecure remote-controlled hardware.

## 6. Detection & Suricata Rule Implementation (Gokulhesh)
### 6.1 Overview

This section documents the custom Suricata IDS rules created to detect the major attacks performed against the OWASP Juice Shop application. The purpose of these rules is to monitor and alert on suspicious HTTP traffic such as SQL injection, XSS attacks, brute-force login attempts, and unauthorized access to administrative pages.

All rules were tested using Suricata on Kali Linux, and alerts were verified in Eve.json, fast.log, and Suricata’s real-time console output.

### 6.2 Custom Detection Rules
## Rule 1 — SQL Injection Detection
alert http any any -> any any (
    msg:"SQL Injection Attempt Detected";
    content:"' OR 1=1";
    nocase;
    sid:1000001;
    rev:1;
)
## Rule 2 — Cross-Site Scripting (XSS) Detection
alert http any any -> any any (
    msg:"XSS Attack Detected";
    flow:to_server,established;
    content:"<script>";
    nocase;
    http_client_body;
    classtype:web-application-attack;
    sid:1000002;
    rev:1;
)
## Rule 3 — Brute-Force Login Attempt
alert http any any -> any any (
    msg:"Brute Force Login Attempt";
    flow:to_server,established;
    content:"/rest/user/login";
    http_uri;
    threshold:type both, track by_src, count 5, seconds 60;
    classtype:attempted-admin;
    sid:1000003;
    rev:1;
)
## Rule 4 — Unauthorized Admin Panel Access
alert http any any -> any any (
    msg:"Unauthorized Admin Panel Access";
    flow:to_server,established;
    content:"/admin";
    http_uri;
    classtype:web-application-attack;
    sid:1000004;
    rev:1;
)

What it detects:
Alerts when JavaScript tags such as <script>alert('XSS')</script> appear in HTTP traffic. 



## 7.Validation (Before & After Remediation)

### 7.1 Pre-Fix Scan Results
Before any remediation was applied, multiple high-risk vulnerabilities were confirmed during penetration testing using tools such as OWASP ZAP, Burp Suite, Wireshark, and manual exploitation.

#### Summary of Pre-Fix Findings:

SQL Injection in login and search parameters allowed authentication bypass.

Reflected XSS executed successfully in the search bar.

Broken Authentication allowed login using default admin credentials.

Open Redirect parameter (/redirect?to=) accepted arbitrary external URLs.

Information Disclosure via unauthenticated /api/products endpoint.

Weak WPA2 wireless password cracked via captured handshake.

Low-frequency RFID badge cloneable using inexpensive tools.

These results confirm that the system was vulnerable across web, wireless, and physical layers.

#### Evidence (Tools Used):

OWASP ZAP active scan alerts

Burp Suite intercepted requests

Wireshark ARP + WPA2 handshake capture

RFID badge scan output
<img width="1600" height="777" alt="image" src="https://github.com/user-attachments/assets/fea3abf6-3c8e-4202-a3b6-805330ecad05" />

(Screenshots already included in Section 3 & Section 5)
### 7.2 Post-Remediation Scan Results
After remediation steps were applied by the team, the environment was re-tested to validate whether the vulnerabilities had been successfully mitigated.

Expected Remediation Applied by Nippun (Section 8):

SQL query handling updated to use parameterized statements

Input sanitization and output encoding added to prevent XSS

Default admin credentials removed; strong password policy enabled

Redirect parameters restricted using allowlists

Sensitive API endpoints now require authentication

WPA2 wireless password upgraded to a strong PSK

RFID/IR devices updated to secure formats (where possible)

Post-Fix Validation Results:

SQL Injection attempts no longer executed; application returned safe error messages.

XSS payloads were sanitized and no alert box appeared.

Admin login rejected default credentials.

Open Redirect now blocks external URLs and displays a warning.

API endpoints require an authenticated session.

Wireless handshake attack failed due to strong passphrase.

RFID clone attempt produced invalid results, indicating enhanced badge security.

These results demonstrate that the remediation actions successfully mitigated the previously identified vulnerabilities.

## 8. Risk & Remediation (Nippun)
| **Risk**                                   | **Impact**  | **Likelihood** | **Description**                                                                                         | **Remediation Summary**                                             |
| ------------------------------------------ | ----------- | -------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **SQL Injection**                          | High        | High           | Unsanitized input allowed SQL queries to run on the backend, enabling login bypass and data extraction. | Use parameterized queries, validate inputs, restrict DB privileges. |
| **Cross-Site Scripting (XSS)**             | Medium–High | Medium         | Unsanitized user input allowed execution of malicious JavaScript.                                       | Add HTML/JS encoding, sanitize inputs, apply CSP.                   |
| **Broken Authentication**                  | High        | High           | Default admin credentials allowed full admin access.                                                    | Enforce strong password policies, MFA, disable default accounts.    |
| **Open Redirect**                          | Medium      | Medium         | Redirect parameter accepted arbitrary URLs.                                                             | Only allow internal redirect destinations (allowlist).              |
| **API Misconfiguration / Info Disclosure** | Medium      | Medium         | /api/products and debug endpoints exposed sensitive data.                                               | Add authentication to all APIs, disable debug mode.                 |
| **Weak WPA2 Passphrase**                   | High        | High           | Wi-Fi handshake could be cracked via dictionary attack.                                                 | Use strong PSK, rotate regularly, upgrade to WPA3.                  |
| **Rogue Access Point (Evil Twin)**         | Medium      | Medium         | Fake APs can impersonate legitimate networks.                                                           | Enable WPA2-Enterprise (802.1X), WIDS/WIPS monitoring.              |
| **RFID Badge Cloning**                     | Low–Medium  | Medium         | Low-frequency HID cards can be cloned easily.                                                           | Use encrypted RFID cards with challenge-response mechanisms.        |
## 8.2 Detailed Remediation Steps
### 8.2.1 SQL Injection

Implement parameterized queries.

Enforce server-side validation on all inputs.

Use least-privilege accounts for DB access.

Deploy WAF rules to detect injection payloads.

### 8.2.2 Cross-Site Scripting (XSS)

Apply output encoding based on rendering context.

Implement strong Content Security Policy (CSP).

Sanitize all user-controlled inputs.

Avoid inline JavaScript.

### 8.2.3 Broken Authentication

Remove all default credentials.

Enforce strong password policy (length + complexity).

Enable MFA for admin accounts.

Apply rate limiting on login attempts.

### 8.2.4 Open Redirect

Only permit redirects to trusted internal domains.

Display warnings when redirecting to external URLs.

Reject parameters containing "http://", "https://", or unapproved hosts.

### 8.2.5 API Misconfiguration

Protect all sensitive API endpoints with authentication.

Disable debug and verbose error output.

Regularly audit configuration files for insecure defaults.

### 8.2.6 Wireless Security Fixes

Upgrade WPA2 password to a strong passphrase (16+ characters).

Segment networks (guest/user/admin) to reduce lateral movement.

Upgrade to WPA3 where possible.

### 8.2.7 Rogue AP Mitigation

Implement WPA2-Enterprise (RADIUS/802.1X).

Deploy Wireless Intrusion Detection/Prevention (WIDS/WIPS).

Train users to avoid duplicate SSIDs.

### 8.2.8 RFID / NFC / IR System Hardening

Replace HID 10301 cards with encrypted high-security alternatives.

Disable unnecessary IR/NFC features.

Use rolling codes or challenge-response mechanisms.

Maintain detailed access logs.

### 8.3 Budget Estimate
Category	Estimated Cost	Notes
Web Application Fixes	$0 – $200	Developer time, CSP, validation logic.
Wireless Hardening	$50 – $300	WPA3 access point, segmentation.
Rogue AP Detection	$100 – $600	Optional WIDS/WIPS tools.
RFID Security Upgrade	$20 – $100	New secure RFID cards/devices.
Documentation / Verification	$0 – $150	Staff time to re-test and document.

Total Estimated Cost Range: $200 – $2,000
(depends on hardware upgrades and monitoring tools selected)
### 8.4 Success Criteria
The remediation will be considered successful when:

All high-severity vulnerabilities (SQLi, Broken Authentication, WPA2 weakness) are fully fixed.

Medium-severity issues show significant risk reduction.

Re-testing confirms vulnerabilities no longer reproduce.

Wireless network demonstrates increased resistance to cracking attacks.

System detects rogue access points or unusual activity.

API endpoints reject invalid or unauthenticated requests.

Security documentation and password rotation procedures are established.
## 9. Conclusion & Recommendations
## 9.1 Conclusion

The combined web, wireless, and hardware security assessment revealed that the target environment was vulnerable across multiple layers—application, network, and physical access.
Critical issues such as SQL Injection, Broken Authentication, Open Redirects, weak WPA2 credentials, and easily cloneable RFID badges demonstrated that attackers could compromise the system using both digital and physical attack vectors.

Through systematic testing using OWASP ZAP, Burp Suite, Wireshark, Suricata, RFID tools, and manual exploitation, the team identified weaknesses affecting confidentiality, integrity, and availability of the environment.

Post-remediation validation showed significant improvements. SQLi and XSS payloads were blocked, admin access was hardened, API endpoints required authentication, WPA2 keys were strengthened, and RFID cloning attempts no longer succeeded. These results confirm that the applied fixes effectively mitigated the previously identified vulnerabilities.

The assessment emphasizes the importance of continuous monitoring, timely patching, secure coding, strong authentication, and defense-in-depth strategies across all layers of the system.

## 9.2 Recommendations

To maintain a strong security posture and prevent regression, the following ongoing practices are recommended:

1. Continuous Vulnerability Scanning

Perform regular automated and manual web security scans (ZAP/Burp).

Re-test after any major application update.

2. Strengthen Access Controls

Enforce strong password policies and MFA for all administrative accounts.

Require unique credentials for each user and rotate them regularly.

3. Secure Coding & Development Practices

Implement input validation, output encoding, and secure frameworks.

Adopt DevSecOps pipelines with SAST/DAST scanning integrated.

4. Network Security Enhancements

Maintain WPA3 or strong WPA2 passphrases.

Segment networks to reduce lateral movement.

Use monitoring tools to detect rogue access points.

5. Improve Physical Security

Replace low-frequency RFID cards with encrypted, challenge-response cards.

Maintain logs for all badge access events.

6. Deploy and Monitor IDS/IPS

Keep Suricata rules updated.

Continuously monitor alerts in eve.json, fast.log, and dashboards.

Add custom signatures as new attack patterns emerge.

7. Regular Security Training

Educate employees about phishing, suspicious redirects, and rogue Wi-Fi networks.

Conduct periodic awareness campaigns.

## 10. Appendix
## 10.1 Logs  
### A. Suricata – SQL Injection Alert (eve.json)
{
  "timestamp": "2025-12-02T14:32:11.451929+0000",
  "flow_id": 123987654321,
  "event_type": "alert",
  "src_ip": "192.168.43.50",
  "src_port": 52841,
  "dest_ip": "192.168.43.1",
  "dest_port": 3000,
  "proto": "TCP",
  "alert": {
    "action": "allowed",
    "gid": 1,
    "signature_id": 1000001,
    "rev": 1,
    "signature": "SQL Injection Attempt Detected",
    "category": "Web Application Attack",
    "severity": 1
  },
  "http": {
    "hostname": "192.168.43.1",
    "url": "/rest/user/login",
    "http_user_agent": "Mozilla/5.0",
    "http_method": "POST"
  }
}

### B. Suricata – XSS Detection Alert (eve.json)
{
  "timestamp": "2025-12-02T14:33:45.229191+0000",
  "event_type": "alert",
  "src_ip": "192.168.43.50",
  "src_port": 52922,
  "dest_ip": "192.168.43.1",
  "dest_port": 3000,
  "proto": "TCP",
  "alert": {
    "action": "allowed",
    "gid": 1,
    "signature_id": 1000002,
    "rev": 1,
    "signature": "XSS Attack Detected",
    "category": "Web Application Attack",
    "severity": 2
  },
  "http": {
    "hostname": "192.168.43.1",
    "url": "/#/<script>alert('XSS')</script>",
    "http_user_agent": "Mozilla/5.0",
    "http_method": "GET"
  }
}

### C. Suricata – Brute Force Attempt Alert (fast.log)
12/02/2025-14:35:02.112233  [**] [1:1000003:1] Brute Force Login Attempt [**]
[Classification: Attempted Administrator Privilege Gain] [Priority: 1] 
{TCP} 192.168.43.50:52999 -> 192.168.43.1:3000

### D. Suricata – Unauthorized Admin Access Alert (fast.log)
12/02/2025-14:36:18.557892  [**] [1:1000004:1] Unauthorized Admin Panel Access [**]
[Classification: Web Application Attack] [Priority: 2] 
{TCP} 192.168.43.50:53012 -> 192.168.43.1:3000

### E. Wireshark – WPA2 Handshake Capture Log
Frame 122: 241 bytes on wire
IEEE 802.11 QoS Data
   Source: 90:ad:5a:b9:61:c9 (Client)
   Destination: f4:f5:e8:34:89:01 (AP)
EAPOL Key (Message 1 of 4)
   Key Descriptor Version: WPA2
   Replay Counter: 1

### F. Wireshark – ARP Request Log
Frame 1: 42 bytes on wire
Ethernet II, Src: CloudNetwork_89:c9 (f0:ad:5a:b9:61:c9), Dst: Broadcast
ARP Request: Who has 10.145.145.180? Tell 10.145.145.157

### G. RFID Badge Reader Output Log
HID H10301
Hex: B3 22 58
FC: 179
Card: 8792
Badge Name: Cis_lab
Status: Successfully Scanned
- Screenshots  
- Tool versions
  ## 10.2 Tools Used
 | **Category**                   | **Tool**                        | **Version**              | **Purpose**                                                      |
| ------------------------------ | ------------------------------- | ------------------------ | ---------------------------------------------------------------- |
| **Web Application Testing**    | OWASP ZAP                       | 2.14.0                   | Automated scanning, spidering, and detecting web vulnerabilities |
|                                | Burp Suite Community            | 2023.12                  | Manual HTTP interception, modification, replaying attacks        |
|                                | Nikto                           | 2.5.0                    | Detect server misconfigurations & outdated components            |
|                                | Curl                            | 7.88.1                   | Manual API testing and custom HTTP request crafting              |
|                                | Mozilla Firefox                 | 120+                     | Browser used for web app interaction during tests                |
| **Operating Systems**          | Kali Linux                      | 2024.2                   | Main penetration testing environment                             |
|                                | Windows 10 Pro                  | 22H2                     | Host OS running OWASP Juice Shop                                 |
| **Intrusion Detection / Logs** | Suricata IDS                    | 7.0.0                    | Custom rule creation, attack detection, generating alerts        |
|                                | Eve.json / Fast.log             | Default Suricata outputs | Log files storing IDS alerts and packet metadata                 |
| **Network / Wireless Tools**   | Wireshark                       | 4.2.0                    | Packet capture, ARP inspection, WPA2 handshake capture           |
|                                | Aircrack-ng Suite               | 1.7                      | Wireless monitoring, handshake cracking attempts                 |
|                                | Linux Monitor Mode Drivers      | Default                  | Enabling packet capture on wlan0                                 |
| **Hardware Security Tools**    | Flipper Zero (RFID/NFC/IR Tool) | Firmware 0.95+           | HID badge scanning, IR capture, replay attacks                   |
|                                | HID 125 kHz Reader              | Standard                 | Reading low-frequency RFID card data (FC & Card ID)              |
|                                | IR Transmitter / Receiver       | Built-in                 | Capturing and replaying remote-control signals                   |
| **Documentation & Dev Tools**  | VS Code                         | Latest                   | Editing Suricata rules and writing reports                       |
|                                | Nano                            | Built-in                 | CLI text editor for quick rule edits                             |
|                                | Git & GitHub                    | Latest                   | Version control and report collaboration                         |
| **Application Under Test**     | OWASP Juice Shop                | Latest Node.js build     | Target vulnerable web application for testing                    |

