---
title: "Sigma rules"
description: "My personal SOC notes and research on Sigma rules."
publishDate: "2026-04-19T00:00:00Z"
tags: ["tools"]
---

# Sigma rules

A language to describe log events in a structured format, enabling quick sharing of detection methods. "Sigma is to log files as Snort is to network traffic and Yara is to files."  Matches log content to raise threat alerts for investigation, typically within SIEM systems or databases.

writing sigma rules:

SIGMA is written in YAML which is a Human-readable data serialization language, often used for configuration files, an alternative to JSON. its main key features are Case-sensitive. Uses .yml extension. Indents with spaces (not tabs). Comments with #. Key-value pairs with :. Arrays/lists with -.

### Sigma Syntax Overview

**Purpose**: Defines structured log event descriptions in YAML for detection rules.

**Key Fields**:

**Title**: Short, clear name of the rule (e.g., "WMI Event Subscription").

**ID**: Unique UUID (e.g., 0f06a3a5-6a09-413f-8743-e6cf35561297).

- **Related**: Links to other rules (types: Derived, Obsolete, Merged, Renamed, Similar).

**Status**: Rule maturity (Stable, Test, Experimental, Deprecated, Unsupported).

**Description**: Detailed purpose of the rule (e.g., "Detects WMI event subscription persistence").

**Logsource**: Specifies log data source.

- **Product**: E.g., windows.
- **Category**: E.g., wmi_ event.
- **Service/Definition**: Optional subsets or configs.

**Detection**: Defines what to detect (mandatory).

- **Search Identifiers**: Fields/values to match.
- **Condition**: Logic for triggering (e.g., selection).

**FalsePositives**: Known benign triggers (e.g., legitimate WMI use).

**Level**: Severity (Informational, Low, Medium, High, Critical).

**Tags**: Metadata (e.g., MITRE ATT&CK: attack. persistence, attack.t1546.003).

![image.png](Sigma%20rules/image.png)

### **Detection Components**

**Search Identifiers**:

**Lists**: OR logic with - (e.g., EventID: [19, 20, 21]).

**Maps**: AND logic with key-value pairs (e.g., Image|endswith: '/rm').

**Value Modifiers**:

**Transformation**:

- contains: Matches anywhere in field.
- all: Changes OR to AND for lists.
- base64: Matches Base64-encoded values.
- endswith: Matches end of field.
- startswith: Matches start of field.

**Type**: re for regular expressions (Elasticsearch-supported).

**Condition Expressions**:

Logical operators: AND, OR, NOT.

Examples: selection, tools and filter, selection and not 1 of filter_*.

https://github.com/SigmaHQ/sigma/wiki/Rule-Creation-Guide

https://sigmahq.io/

[SigmacheatsheetPDF-1673525775585.pdf](/notes/the-soc-notes/sigma-rules/sigmacheatsheetpdf-1673525775585-pdf/)