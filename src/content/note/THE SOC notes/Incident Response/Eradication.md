---
title: "Eradication"
description: "My personal SOC notes and research on Eradication."
publishDate: "2026-04-19T00:00:00Z"
tags: ["Incident Response"]
---

# Eradication

### **Eradication:**

Remove threats and their traces while avoiding mistakes that worsen the situation. the  **Main Goals is to** Eradicate threats (prioritize critical systems). Recover to normal operations. while doing so u should put into your consideration 

### **Avoid Premature Eradication**:

Rushing from scoping skips full understanding. Risks: Adversary speeds up damage, spreads persistence, or starts “whack-a-mole” cycle. Fix: Use intel-driven scoping first.

### **Expect Initial Failure**:

Even with good scoping, first attempts may fail. Adversaries adapt, get sneakier—use feedback loops with scoping to refine plans.

### **Eradication Methods:**

**Automated**: AV/EDR for simple malware (less effective vs. sophisticated threats).

**Complete Rebuild**: Wipe/reinstall system (clean but causes downtime, data loss).

**Targeted Cleanup**: Precise removal without tipping off adversary (needs good scoping, minimal downtime).

### **Remediation Phase:**

the process of Identifying vulnerabilities/misconfigurations from incident. and the steps of remediation are  **Network Segmentation:** Limit communication, boost visibility. **IAM Review:** Restrict compromised/privileged accounts (least privilege). **Patch Management:** Fix exploited flaws, roll out patches org-wide.

### **Recovery Phase:**

Restore normal operations with a stronger security posture. 

**Steps:**

**Test & Monitor**: Use pen tests/simulations to validate fixes, create feedback loop.

**Backups**: Restore from data/config backups (automated scripts or cloud images best).

**Action Plan**: *Near-term*: Critical fixes now (e.g., patch key systems). *Mid/Long-term*: Bigger changes (e.g., network redesign) with approvals.