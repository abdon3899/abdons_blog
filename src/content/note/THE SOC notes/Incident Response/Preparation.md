---
title: "Preparation"
description: "My personal SOC notes and research on Preparation."
publishDate: "2026-04-19T00:00:00Z"
tags: ["Incident Response"]
---

# Preparation

### **Purpose:**

Ensures the **organization is ready** to detect, respond to, and recover from incidents. Minimizes damage, reduces recovery time, and maintains business continuity. Covers **people, policies, technology, communication, and documentation**.

### **People:**

Most vulnerable to attacks (e.g., phishing, social engineering). Train employees to spot and handle incidents at all stages.

### **CSIRT Teams:**

Form a team with business, technical, legal, and PR experts. Set controlled access policies and notify admins for privileged use.

### **Training:**

Ongoing sessions to build attack awareness and response skills.

### **Documentation:**

Vital for evidence, mitigation, and lessons learned. Requires strong note-taking skills.

### **Policies:**

Guide incident handling. Use warning banners to limit unauthorized actions and privacy expectations. Include monitoring rights, legally reviewed.

### **Communication Plan:**

Assign CSIRT contacts and procedures (e.g., on-call staff).

### **Chain of Custody:**

Track evidence and info flow with templates.

### **Response Procedures:**

Treat as an organization-wide effort with clear roles. Aim to limit damage and recover fast with approved processes.

Equip the CSIRT with tools and solutions to protect and defend the organization’s tech infrastructure. Know your systems to prevent, detect, and mitigate incidents effectively.

### **Asset Inventory Classification:**

Identify and prioritize high-value assets (e.g., infrastructure, data, IP, reputation) to protect confidentiality, integrity, and availability. Maintain hardware/software inventory lists.

*Example*:

- Mail Server: MailSrvr1, Windows Server 2019, 192.168.0.2
- Web Server: WebSrvr1, Ubuntu 20.04, 192.168.0.3

### **Technical Instrumentation:**

Map network devices, cloud platforms, systems, and apps for telemetry. Use tools like anti-malware, EDR, DLP, IDPS, and log collectors. Apply network subnetting (firewalls, DMZs, IP segmentation) for control. Track telemetry with tools like TheHive.

### **Investigation Capabilities:**

Enable CSIRT to validate scripts/installers, contain attacks, and analyze evidence. Use disk/memory imaging, secure storage, and sandboxes. Maintain an incident-handling jump bag with:

- Media drives for evidence.
- Forensic tools (e.g., FTK Imager, EnCase, Sleuth Kit).
- Network tap for traffic monitoring.
- Cables/adapters (USB, SATA, card readers).
- PC repair kit (screwdrivers, tweezers).
- Incident response forms and playbooks.

### **Visibility**

Collect logs/audit data for full network coverage. Monitor threat intelligence (TTPs) and vendor patch advisories. to Detect, assess, and mitigate unauthorized/abnormal activity.

### **Benefits:**

Tracks resource access (who, when, what). Improves processes, policies, and procedures. Provides evidence for incident handling. Simplifies regulatory compliance. Keeps you updated on threats and patches.