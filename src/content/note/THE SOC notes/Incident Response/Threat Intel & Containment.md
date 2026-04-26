---
title: "Threat Intel & Containment"
description: "My personal SOC notes and research on Threat Intel & Containment."
publishDate: "2026-04-19T00:00:00Z"
tags: ["Incident Response"]
---

# Threat Intel & Containment

### **Containment Phase:**

Stop an incident from spreading, gather info on the adversary, and limit damage using evidence (e.g., IDS, SIEM) to build Indicators of Compromise (IoCs).

### **Containment Strategies:**

- **Entire Isolation**:
    
    Aggressive: Fully isolate infected devices (network or physical). it  Stops spread fast. but at the cost of Alerting adversary, may rush their goals or shift focus. its Considered Aggressive, adversary’s likely reaction, intel level.
    
- **Controlled Isolation**:
    
    Less aggressive: Monitor adversary without full cutoff. it Gathers intel on adversary behavior. Risky if they act destructively (e.g., wipe data).  “Cover stories” (e.g., maintenance) to mask monitoring. Consider: Risk, existing intel, resources to monitor/stop them.
    

### **Threat Intelligence:**

Info on malicious actors (e.g., IPs, hashes, domains, TTPs). it help us to Understand adversary aims and methods to tailor containment.

- **TTPs**:
    - *Tactics*: High-level goals (e.g., steal data, damage systems).
    - *Techniques*: Tools/methods (e.g., phishing, privilege escalation).
    - *Procedures*: Full attack chain (e.g., phishing → lateral movement).

**Platforms:**

**OpenCTI**: Share threat intel (e.g., tal0nix adversary report). **Feeds**: Digital Side, AlienVault, threatfeeds.io for predictive insights. **SIEM**: Arm with IoCs for pre-incident alerts.

### **Linking Phases:**

Bridges identification/scoping to eradication/recovery. Better intel → better scope → smarter containment.

### **Risk of Rushing:**

Eradicating without scoping risks missing compromised systems. “Whack-a-mole”: Fixing one system may leave others vulnerable.

### **Feedback Loops:**

Intel from containment aids recovery/lessons learned.Assess: Was detection sufficient? Too much SIEM noise?Ongoing process to improve defenses.