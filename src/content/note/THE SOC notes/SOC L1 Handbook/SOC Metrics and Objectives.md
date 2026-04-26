---
title: "SOC Metrics and Objectives"
description: "My personal SOC notes and research on SOC Metrics and Objectives."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC L1 Handbook"]
---

# SOC Metrics and Objectives

## Assets & Metrics

**SOC Goal**: Protect confidentiality, integrity, and availability of digital assets by developing, receiving, and triaging alerts.

**L1 Role**: Report True Positives to L2 for remediation.

### **Alerts Count**:

- Measures unresolved alerts in queue.
- Too many (e.g., 80/day): Overwhelming, risks missing threats.
- Too few (e.g., 0/week): Indicates SIEM issues or lack of visibility.
- Ideal: 5–30 alerts/day per L1 analyst.

### **False Positive Rate**:

- Percentage of alerts confirmed as noise (e.g., 94% = 75/80 False Positives).
- High rate (80%+): Reduces vigilance, requires tool/detection rule tuning (False Positive Remediation).
- Ideal: Below 80%, 0% unachievable.

### **Alert Escalation Rate**:

- Percentage of alerts escalated to L2.
- Reflects L1 experience/independence.
- Ideal: Below 50%, preferably below 20%.

### **Threat Detection Rate (TDR)**:

- Percentage of attacks detected/prevented (e.g., 4/6 attacks = 67%).
- Goal: 100% (every missed threat risks severe impact, e.g., ransomware, data exfiltration).

## Triage Metrics

Ensure timely detection, triage, and response to threats.

**Service Level Agreement (SLA)**:

- Contract between SOC and management (or MSSP and clients).
- Defines response timelines.

### **Mean Time to Detect (MTTD)**:

Time from attack start to alert generation (e.g., 12 min for malware C2 connection).

### **Mean Time to Acknowledge (MTTA)**:

 Time from alert to L1 moving it to "In Progress" (e.g., 10 min).

### **Mean Time to Respond (MTTR)**:

Total time from alert to remediation (e.g., 51 min: 10 min MTTA + 6 min to escalate + 35 min L2 cleanup).

![image.png](SOC%20Metrics%20and%20Objectives/image.png)

## Improving Metrics

**Importance for L1**:

- Enhances SOC efficiency, reduces attack success.
- Used to evaluate performance, impacts career growth (e.g., promotion to L2).

**Improvement Strategies**:

- **Alerts Count**: Tune SIEM rules to balance visibility and noise.
- **False Positive Rate**: Adjust detection rules to reduce noise.
- **Escalation Rate**: Build experience to handle more alerts independently.
- **Threat Detection Rate**: Improve detection rules, ensure proper triage.
- **Triage Metrics (MTTD/MTTA/MTTR)**: Act quickly, follow workflows, escalate efficiently.