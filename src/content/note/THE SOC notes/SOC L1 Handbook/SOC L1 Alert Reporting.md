---
title: "SOC L1 Alert Reporting"
description: "My personal SOC notes and research on SOC L1 Alert Reporting."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC L1 Handbook"]
---

# SOC L1 Alert Reporting

### Alert Funnel

L1 analysts receive alerts via SIEM, EDR, or ticketing platforms; most are closed as False Positives or handled at L1, while complex True Positives are escalated to L2 for remediation.

**Funnel Breakdown**:

- L1: Handles 100 alerts.
- L2: Receives 10 True Positives.
- DFIR: 1 incident requiring formal response.

![image.png](SOC%20L1%20Alert%20Reporting/image.png)

### Alert Reporting

 Document investigation for closure or escalation, especially for True Positives.

**Purposes**:

**Context for Escalation**: Saves L2 time, provides initial context. **Record Keeping**: Alerts stored indefinitely (vs. SIEM logs, 3-12 months). **Skill Improvement**: Enhances L1 skills through summarization.

**Report Format (5Ws Approach)**:

- **Who**: Affected user/entity (e.g., login, command execution).
- **What**: Action/event sequence.
- **When**: Start/end timestamps.
- **Where**: Device, IP, or website involved.
- **Why**: Reasoning for verdict (True/False Positive).

### Alert Escalation

Pass True Positive alerts needing deeper investigation or remediation to L2.

**When to Escalate**:

- Indicates major cyberattack (requires DFIR).
- Needs remediation (e.g., malware removal, host isolation, password reset).
- Requires external communication (customers, management, law enforcement).
- L1 needs senior support.

**Steps**:

1. Reassign alert to L2 on shift.
2. Notify L2 (chat, in-person, or formal request per team policy).
3. L2 reviews report, validates alert, researches further, and initiates Incident Response for major incidents.

**Dashboard Procedure**:

- Write report, set verdict, move alert to "In Progress."
- Assign to L2 for review.

### SOC Communication

Coordinate with departments (e.g., IT, HR) during/after triage.

**Examples**:

- Verify admin privileges with IT.
- Confirm employee details with HR.

**Crisis Communication Cases**:

- **L2 Unavailable (Critical Alert)**: Call L2, then L3, then manager using emergency contacts.
- **Account Compromise (e.g., Slack/Teams)**: Avoid breached chat; use phone/email.
- **Alert Overload**: Prioritize per workflow, inform L2.
- **Misclassification (Post-Triage)**: Notify L2 immediately.
- **SIEM Log Issues**: Investigate what’s possible, report to L2 or SOC engineer.

![678ecc92c80aa206339f0f23-1743520519371.svg](SOC%20L1%20Alert%20Reporting/678ecc92c80aa206339f0f23-1743520519371.svg)

![678ecc92c80aa206339f0f23-1743520519371.svg](SOC%20L1%20Alert%20Reporting/678ecc92c80aa206339f0f23-1743520519371%201.svg)

![678ecc92c80aa206339f0f23-1743610529685.svg](SOC%20L1%20Alert%20Reporting/678ecc92c80aa206339f0f23-1743610529685.svg)