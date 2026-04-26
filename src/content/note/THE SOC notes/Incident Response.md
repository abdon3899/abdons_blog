---
title: "Incident Response"
description: "My personal SOC notes and research on Incident Response."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/IR.gif"

---

# Incident Response

A cybersecurity function focused on detecting, managing, and mitigating adversarial attacks while minimizing impact and recovery time. Coupled with Digital Forensics: Investigates incidents to determine root causes and gather evidence. its Goals are Contain malware infections. Identify and remediate vulnerabilities. Coordinate technical and non-technical personnel.

**Events are** An observed occurrence (e.g., user login, email sent, antivirus blocking malware).while  **Incidents are** A security policy violation with negative impact (e.g., ransomware, data exfiltration, DoS).

**incident Response Process consists of six phases:**

[**Preparation**](/abdons_blog/notes/the-soc-notes/incident-response/preparation/)

- Establish IR procedures, policies, and tools.
- Train personnel, define roles, and create IR plans/playbooks.

[**Identification** ](/abdons_blog/notes/the-soc-notes/incident-response/identification/)

- Detect and confirm security incidents.
- Use monitoring tools (SIEM, EDR, logs).

[**Threat Intel & Containment**](/abdons_blog/notes/the-soc-notes/incident-response/threat-intel-containment/)

- Determine the extent of the incident (affected systems, data at risk, impact).
- Isolate affected systems to prevent spread.
- Preserve forensic evidence.

[**Eradication** ](/abdons_blog/notes/the-soc-notes/incident-response/eradication/)

- Remove malware, backdoors, and adversarial artifacts.
- Restore systems to normal operations.

[**Lessons Learned** ](/abdons_blog/notes/the-soc-notes/incident-response/lessons-learned/)

- Conduct a post-incident review, update IR plans, and improve defenses.