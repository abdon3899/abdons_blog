---
title: "Identification"
description: "My personal SOC notes and research on Identification."
publishDate: "2026-04-19T00:00:00Z"
tags: ["Incident Response"]
---

# Identification

Spot incidents quickly to limit damage and speed recovery. Combines people, processes, and tech for effective detection.

### **Triad of Identification:**

**People**: All staff report anomalies, not just IT/security. **Process**: Follow procedures to interpret alerts and notify the right people. **Technology**: Tools generate alerts for potential incidents.

### **Security Alerts:**

Signals of threats/incidents that trigger response. Understand type/severity using expertise, tools, and vigilance.

### **Tech & Expertise:**

Use tools (e.g., EDR, IDPS, SIEM) for detection: *Aurora EDR, Wazuh* (Endpoint). *Snort* (IDPS). *ELK, Splunk* (SIEM).

### Culture of Learning:

Top-down priority: Train all to spot/report threats. Policies/procedures (legal-reviewed) guide reporting.

## **Scoping Phase**

Determine incident extent (affected systems, data, impact) to guide mitigation.

### **Asset Inventory:**

List assets for quick reference:

- *Example*:
    - Domain Controller: DC-01, 172.16.1.10, Windows Server 2019.
    - Web Server: WEBSVR-01, 172.16.1.110, Ubuntu 20.04.

### **Spreadsheet of Doom (SoD):**

Track IoCs (e.g., IPs, domains, hashes) with context:

- *Example*:
    - IP: 188.40.75.132, Malware Hosting, AlienVault.
    - Domain: groupmarketingonline.icu, Phishing, VirusTotal.

### **Identification-to-Scoping Transition:**

Clear communication and process link phases. Insights from identification refine scoping.

### **Feedback Loop:**

**Steps**:

- *Event Notification*: Issue reported.
- *Documentation*: Detail incident/systems.
- *Evidence Collection*: Gather logs, traffic.
- *Artefact Identification*: Analyze for clues.
- *Pivot Points*: New findings loop back.

**Benefits:** Faster, effective responses. Ongoing learning and threat awareness. Ensures privacy/data protection compliance.