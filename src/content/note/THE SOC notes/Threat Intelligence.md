---
title: "Threat Intelligence"
description: "My personal SOC notes and research on Threat Intelligence."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/th.gif"

---

# Threat Intelligence

**Definition**: Analysis of data/information using tools/techniques to identify patterns and mitigate risks from existing/emerging threats targeting organizations, industries, or governments.

## **Classifications of Threat Intelligence**

### **Strategic Intel**:

High-level overview of threat landscape.

Maps risks based on trends/patterns for business decisions.

### **Technical Intel**:

Examines attack evidence/artifacts (e.g., IOCs).

Used by Incident Response to analyze/develop defenses.

**Focus of this room**: IOC-based Threat Intelligence.

### **Tactical Intel**:

Assesses adversaries’ TTPs.

Strengthens security controls via real-time investigation.

### **Operational Intel**:

Analyzes adversaries’ motives/intent.

Identifies critical assets (people, processes, tech) at risk.

# **Producers vs. Consumers**

Do you build (produce) or use (consume) threat intelligence?

### **Producers**

- **Role**: Gather, analyze, and share threat intelligence.
    - Create reports, advisories for the cybersecurity community.
- **Examples**: Cybersecurity vendors, research labs, large organizations.
- **Methods**:
    - Network monitoring (internal/external, e.g., honeypots).
    - IOC collection from internal incidents.
- **Requirements**:
    - Large data sets, ability to define normal behavior, analytical capacity.
- **Output**: Analyzed data attributed to threat actors, shared with others.

### **Consumers**

- **Role**: Use threat intelligence from producers to enhance security.
- **Uses**:
    - **Vulnerability Identification**: Leverage CVEs/advisories to find weaknesses.
    - **Prevention/Detection**: Block IOCs or create detection rules.
    - **Incident Response**: Confirm attacks and TTPs for faster response.
    - **Collaboration**: Share validated findings with others.
- **Examples**: Analysts applying external feeds to security operations.

### **Producer vs. Consumer Roles**

| **Role** | **Producer** | **Consumer** |
| --- | --- | --- |
| **Activity** | Collect/analyze data to produce actionable intelligence. | Monitor systems, use external intelligence to understand threats. |
| **Output/Input** | Create/distribute threat intelligence reports to peers, regulators, etc. | Use third-party feeds/reports to improve security posture. |

### **Assessment Framework**

| **Aspect** | **Producer** | **Consumer** |
| --- | --- | --- |
| **Understanding** | Evaluate quality (relevance, accuracy, timeliness) of produced intelligence. | Assess if intelligence enhances security posture effectively. |
| **Collection** | Evaluate ability to gather/analyze data from logs, endpoints, etc. | Evaluate ability to collect/consume external intelligence. |
| **Analytics** | Assess skills in detecting/analyzing threats and communicating findings. | Evaluate ability to process/analyze collected intelligence. |
| **Application** | Evaluate response capability based on produced intelligence. | Evaluate response to threats identified via intelligence. |

![image.png](Threat%20Intelligence/image.png)

![image.png](Threat%20Intelligence/image%201.png)

![image.png](Threat%20Intelligence/image%202.png)