---
title: "Detection Engineering"
description: "My personal SOC notes and research on Detection Engineering."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/Detection.gif"

---

# Detection Engineering

its the continuous process of building and operating threat intelligence analytics to identify potentially malicious activity or misconfigurations that may affect your environment. It requires a cultural shift with the alignment of all security teams and management to build effective threat-rich defense systems.

## Detection Types:

Threat detection can be viewed from two perspectives, each comprising two categories: 

### **Environment-based** detection:

focuses on looking at changes in an environment based on configurations and baseline activities that have been defined. Within this detection, we have Configuration detection and Modelling.

### **Configuration Detection**

Under this detection, we use current knowledge of the known environment and infrastructure to identify misalignments. Configurations can cross domains, including network, asset or identity.

its pros are The easiest form of detection to create and maintain in static environments. Under perfect conditions and coverage, it detects all malicious activity. Individuals with different expertise can execute the detection.Easy to combine with other detections for forensics and response.

but its cons are Difficult to maintain in dynamic environments. Limited visibility reduces effectiveness. There’s an assumption of knowledge of the working infrastructure and configurations for effectiveness. Frequent configuration changes can result in high false positives.

### modelling:

involves defining baseline operations and activities, then identifying deviations from these baselines. This approach assumes that malicious activity can be effectively distinguished from benign behavior. It works by creating asset or activity profiles that include baseline events, timeframes, and data thresholds. 

the key benefits of this method is its ability to detect unknown adversary activities based on changes in the model rather than specific threat characteristics, making it particularly useful in static environments where it is easier to maintain. 

it also have some the challenges like. It provides no context about threat activity during investigations, making it difficult to understand the nature of detected anomalies. Maintaining the model becomes challenging in dynamic environments, and its effectiveness is reduced by limited visibility. Additionally, it assumes in-depth knowledge of the infrastructure and configurations, and there is a risk of incorporating existing malicious activity into the model, which can compromise its accuracy.

### **Threat-based** detection:

focuses on elements associated with an adversary’s activity, such as tactics, tools and artefacts that would identify their actions. Under this, we have Indicators and Threat Behavior detections.

### **Indicator Detection :**

relies on using pieces of information, known as indicators, to identify the state and context of an element or entity. These indicators can be good (e.g., whitelists for legitimate activities) or bad (e.g., blacklists or malware IPs for malicious resources). Indicators of Compromise (IOCs) are often derived from investigations into malicious events, allowing analysts to craft detections based on observed threat activities and adapt them as adversaries evolve. 

This method offers several benefits, including being the fastest detection to create and deploy, providing specific threat context, enriching data sources, and being practical for scoping environments post-investigation. 

However, it also faces challenges, such as its effectiveness depending on the adversary’s rate of change, being retroactive (requiring observation of the indicator first), limited processing capacity for indicators, and the risk of false detections due to unknown indicator expiry or change timelines.

### Threat behavior detection:

focuses on analyzing an adversary’s Tactics, Techniques, and Procedures (TTPs) to identify attacks, independent of specific indicators. This approach makes detection more scalable and efficient, allowing analysts to prioritize response and mitigation efforts rather than spending time understanding how alerts were triggered. It can also be integrated with established workflows and playbooks, providing best practices for investigations.

 The benefits of this method include its resilience to the adversary’s rate of change, ease of tuning and adaptation across environments, low false positive rates, and compatibility with defensive playbooks and automated remediation plans. 

However, it also presents challenges, such as requiring large amounts of data for comprehensive coverage due to adversary complexity, moderate difficulty in initial implementation due to baseline assessments, limited detection of only similar threat behaviours based on set analytics, and the need for modifications when reusing detections across industries. Combining threat behaviour detection with other methods, such as model-based or configuration detection, can enhance overall defence systems by reducing false positives and improving accuracy.

### Detection as Code (DaC):

is a structured approach that applies software engineering best practices to the creation and management of detection processes. By treating detections as code, detection engineers and analysts can scale their efforts to adapt to rapidly changing environments and evolving adversary capabilities. DaC introduces a code-driven workflow that integrates key elements of Continuous Integration/Continuous Development (CI/CD), such as **version control and automation workflows.** Version control ensures changes to detection rules and processes are tracked, reviewed, and tested, improving detection quality. Automation enables efficient testing and quick deployment of detections into production.

The benefits of DaC include:

- **Customisable and Flexible Detections:** Using common languages like Sigma and YARA makes DaC vendor-agnostic, allowing deployment across various SIEM, EDR, and XDR solutions.
- **Test-Driven Development:** Ensures early identification of blind spots and false positives, improving detection efficacy and documentation.
- **Team Collaboration:** CI/CD workflows foster collaboration between security teams, breaking down silos.
- **Code Reusability:** Emerging detection patterns allow engineers to reuse code for similar functions, speeding up the detection process and reducing redundancy.

## Methodologies:

### Detection Gap Analysis (Threat Modelling):

Identifies areas for improving threat detection by analyzing the environment.
**Reactive Approach:** Reviews recent internal incident reports to identify missed detection opportunities and lessons learned.
**Proactive Approach:** Uses frameworks like MITRE ATT&CK and threat intelligence to map potential attack vectors and adversary TTPs.

### Data source Identification and Log Collection:

Identifies relevant data sources and logs needed to detect identified threats. Determines available logs and identifies gaps in log collection to ensure comprehensive coverage.

### Baseline Creation:

Establishes normal behavior baselines to distinguish between legitimate and malicious activities. Involves collaboration across departments and categorizes baselines into:
**High-level:** Broad, OS-independent standards guided by security policies.
**Technical:** OS-specific configurations outlining system functions, behaviors, and policies (e.g., OS hardening, network activities, IAM, application policies).

### Log Collection:

Aggregates logs and metadata from identified data sources using centralized systems (e.g., network sensors, Sysmon for host data).

### Rule Writing:

Writes and tests detection rules against collected data sources to identify abnormal patterns. Uses tools like Snort for network traffic analysis and Yara for file data evaluation. Introduces Sigma, a generic signature language for writing detection rules against log files.

### Deployment, Automation & Tuning:

Deploys tested detection rules into production for real-world assessment. Continuously updates and tunes rules to adapt to evolving attack vectors, patterns, and environmental changes. Treats detection as an ongoing process to improve quality and effectiveness.

## Frameworks:

### **MITRE ATT&CK Framework:**

A knowledge base that maps adversarial tactics, techniques, and procedures (TTPs) across platforms like Windows, macOS, Linux, and Mobile. Helps in detection engineering by guiding what to look for during detection gap analysis. Used to understand and track adversary behaviors. ATT&CK framework https://attack.mitre.org/

### **CAR (Cyber Analytics Repository):**

A knowledge base focused on detecting adversary behaviors. Prioritizes detections based on the MITRE ATT&CK framework.

https://car.mitre.org/

### **Pyramid of Pain:**

A framework that illustrates the "pain" adversaries experience when defenders detect and disrupt their TTPs. The higher up the pyramid (e.g., detecting TTPs instead of just IPs or hashes), the more difficult and costly it is for adversaries to adapt.

![image.png](Detection%20Engineering/image.png)

### **Cyber Kill Chain:**

Developed by Lockheed Martin, this framework outlines the seven stages of a cyber attack:

1. **Reconnaissance:** Gathering information about the target.
2. **Weaponization:** Creating malicious tools or payloads.
3. **Delivery:** Transmitting the weaponized payload to the target.
4. **Exploitation:** Triggering the payload to exploit vulnerabilities.
5. **Installation:** Installing malware or backdoors on the target system.
6. **Command & Control (C2):** Establishing communication with the compromised system.
7. **Actions on Objectives:** Achieving the attacker’s goals (e.g., data theft, destruction).

![image.png](Detection%20Engineering/image%201.png)

### **Unified Kill Chain:**

Expands the Cyber Kill Chain into 18 phases, integrating elements from frameworks like MITRE ATT&CK. Provides a more comprehensive view of cyber attacks.

![](Detection%20Engineering/image%202.png)

### **Alerting and Detection Strategy (ADS) Framework:**

Developed by Palantir, the ADS Framework provides a structured approach to documenting and implementing detection content to address challenges like alert fatigue and ineffective incident response. The framework follows a strict flow with the following stages:

**Goal:**Defines the purpose of the alert and the specific behavior or threat it aims to detect.

**Categorisation:** Maps the detection to the MITRE ATT&CK framework, providing context on the TTPs and the stage of the kill chain it addresses.

**Strategy Abstract:** Provides a high-level description of the detection strategy, including what the alert looks for, data sources, enrichment resources, and methods to reduce false positives.

**Technical Context:** Describes the technical environment and tools used for detection, ensuring analysts understand the alert's context and relevance.

**Blind Spots and Assumptions:** Identifies potential gaps where the detection may fail or be bypassed by adversaries, ensuring transparency and improvement opportunities.

**False Positives:** Outlines scenarios where non-malicious activities might trigger alerts, helping to fine-tune the detection to minimize noise.

**Validation:** Tests the detection to ensure it triggers on true-positive events. This involves creating a plan, documenting the process, and testing in a controlled environment.

**Priority:** Sets alerting levels based on the severity and impact of the detected threat, separate from SIEM alerting levels.

**Response:** Provides guidance on how to triage and investigate alerts, ensuring effective incident response and mitigation.

![image.png](Detection%20Engineering/image%203.png)

### **Detection Maturity Level (DML) Model:**

Introduced by Ryan Stillions, the DML model assesses an organization's ability to detect and respond to cyber threats using threat intelligence. The model consists of nine levels, ranging from technical indicators to abstract adversary goals:

**DML-8 Goals:** Detects adversary motives and goals (rarely achievable).

**DML-7 Strategy:** Detects adversary intentions and strategies (requires mature intelligence sources).

**DML-6 Tactics:** Detects adversary tactics based on patterns of events over time.

**DML-5 Techniques:** Detects specific techniques used by adversaries, often tied to individual threat actors or APTs.

**DML-4 Procedures:** Detects sequences of events or procedures used by adversaries.

**DML-3 Tools:** Detects adversary tools during transfer or operation, sometimes requiring reverse engineering.

**DML-2 Host & Network Artefacts:** Detects artifacts left by adversaries, often after the fact.

**DML-1 Atomic Indicators:** Detects threats using basic indicators like IPs and domains from threat intel feeds.

**DML-0 None:** No detection processes in place.

**Key Use Cases of the DML Model:**

1. Provides a common language for communicating threat information.
2. Assesses detection maturity against monitored threat actors.
3. Evaluates the maturity of security vendors and products.
4. Adds context to detection rules (e.g., Yara, Snort, SIEM) by including DML levels.

![image.png](Detection%20Engineering/image%204.png)

in action :

we can create Sigma rules so i detects the iocs  , we use tools like https://uncoder.io/ so we can translate sigma rules to different SIEM platforms like splunk and elastic stack . 

we can also create things in the windows machine that triggers when accessed use this link https://www.ultimatewindowssecurity.com/securitylog/book/page.aspx?spid=chapter7