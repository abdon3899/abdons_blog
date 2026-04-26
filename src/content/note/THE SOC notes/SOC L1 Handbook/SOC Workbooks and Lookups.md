---
title: "SOC Workbooks and Lookups"
description: "My personal SOC notes and research on SOC Workbooks and Lookups."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC L1 Handbook"]
---

# SOC Workbooks and Lookups

### Assets & Identities

 Use inventories to determine if activity (e.g., file sharing) is expected or suspicious.

**Identity Inventory**:

- Catalogue of employees (user accounts), services (machine accounts), and details (privileges, contacts, roles).
- Helps understand user context (e.g., roles, working hours of G.Baker, R.Lund).

**Asset Inventory**:

- List of computing resources (servers, workstations) in the IT environment.
- Provides context on assets (e.g., purpose, location of HQ-FINFS-02).

### Network Diagrams

**Network Diagrams**:

- Visual schema of locations, subnets, and connections.
- Helps analyze network activity (e.g., services on ports, subnet roles).

**Attack Path Reconstruction**:

- Threat actor (103.61.240.174) performs VPN brute force on TCP/10443.
- Gains VPN access, assigned IP (10.10.0.53) in VPN Subnet.
- Scans Database Subnet (172.16.15.0/24), blocked by firewall.
- Switches to Office Subnet (172.16.23.0/24) for further targets.

### Workbooks Theory

Ensures L1 follows all steps, preventing missed evidence or incorrect verdicts. Streamlines analysis for junior analysts.

**SOC Workbooks**:

- Structured documents (aka playbooks/runbooks/workflows) defining steps to investigate/remediate threats consistently.
- Created by senior analysts to support L1 analysts in avoiding mistakes.

**Workbook Structure** (e.g., Atypical Login Alerts):

- **Enrichment**: Gather context using Threat Intelligence, identity inventory.
- **Investigation**: Analyze data, SIEM logs to determine if activity is expected.
- **Escalation**: Escalate to L2 or communicate with user if needed.

![image.png](SOC%20Workbooks%20and%20Lookups/image.png)