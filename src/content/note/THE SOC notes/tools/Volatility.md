---
title: "Volatility"
description: "My personal SOC notes and research on Volatility."
publishDate: "2026-04-19T00:00:00Z"
tags: ["tools"]
---

# Volatility

## Introduction to Volatility

 Volatility is the leading framework for extracting digital artifacts from volatile memory (RAM), independent of the system under investigation, providing insight into the runtime state.

Introduces techniques for memory forensics and supports research in the field.

Built on plugins to analyze memory dumps; requires identifying image type first.

### **Versions**:

Two repositories: Volatility (Python 2), Volatility3 (Python 3).

Recommended: Volatility3 

[https://github.com/volatilityfoundation/volatility3](https://github.com/volatilityfoundation/volatility3)

Syntax differs between Volatility2 and Volatility3; 

## Installing Volatility

 Written in Python, works on Windows, Linux, Mac.

### **Pre-Packaged Executable (Windows Only)**:

Download `.whl` file from releases: `https://github.com/volatilityfoundation/volatility3/releases/tag/v1.0.1`.

No dependencies required.

### **Run from Source**:

**Required Dependencies**:

- Python 3.5.3+: `https://www.python.org/downloads/`.
- Pefile 2017.8.1+: `https://pypi.org/project/pefile/`.

**Optional Dependencies** (Recommended):

- yara-python 3.8.0+: `https://github.com/VirusTotal/yara-python`.
- Capstone 3.0.0+: `https://www.capstone-engine.org/download.html`.
- Clone Repository: `git clone <https://github.com/volatilityfoundation/volatility3.git`>.
- **Test Installation**: `python3 vol.py -h`.
- **Linux/Mac Note**: Download symbol files from Volatility GitHub: `https://github.com/volatilityfoundation/volatility3#symbol-tables`.

## Memory Extraction:

Extract memory dumps from bare-metal or virtual machines.

**Bare-Metal Extraction Tools**:

- FTK Imager, Redline, DumpIt.exe, win32dd.exe/win64dd.exe, Memoryze, FastDump.
- Output: Typically `.raw` file (except Redline, which uses its own structure).
- Note: Can be time-intensive.

**Virtual Machine Extraction**:

File Types by Hypervisor:

- VMware: `.vmem`.
- Hyper-V: `.bin`.
- Parallels: `.mem`.
- VirtualBox: `.sav` (partial memory).

## Plugins Overview:

Profiles deprecated (no need for specific OS/build profiles). Plugin Naming: Specify OS (e.g., `windows.info`, `linux.info`, `mac.info`).

**OS Options**: `.windows`, `.linux`, `.mac`.

**Plugins**:

Volatility3 has fewer plugins than Volatility2 (still in development). Use help menu to explore: `python3 vol.py -h`.

## Identifying Image Info:

**Volatility3 Approach**:

- Automatically identifies OS and build (profiles deprecated).

**Plugin**: Use `windows.info`, `linux.info`, or `mac.info` to get host details.

- Syntax: `python3 vol.py -f <file> windows.info`.

In Volatility2, `imageinfo` plugin was used to suggest profiles (not always accurate).

## Listing Processes and Connections:

### **Process Plugins**:

**pslist**: Lists processes from doubly-linked list (like Task Manager).

- Syntax: `python3 vol.py -f <file> windows.pslist`.
- Includes active/terminated processes (with exit times).
- Limitation: Misses processes unlinked by malware (e.g., rootkits).

**psscan**: Scans for `_EPROCESS` structures to find processes.

- Syntax: `python3 vol.py -f <file> windows.psscan`.
- Detects unlinked processes, but risks false positives.

**pstree**: Lists processes by parent process ID (same method as pslist).

- Syntax: `python3 vol.py -f <file> windows.pstree`.
- Useful for understanding process hierarchy.

### **Network Connections**:

**netstat**: Identifies network connections in memory.

- Syntax: `python3 vol.py -f <file> windows.netstat`.
- Unstable on older Windows builds.
- Alternative: Use `bulk_extractor` to extract PCAP (`https://tools.kali.org/forensics/bulk-extractor`).

### **DLLs**:

- **dlllist**: Lists DLLs associated with processes.
    - Syntax: `python3 vol.py -f <file> windows.dlllist`.
    - Useful for identifying malware-related DLLs.
    

## Volatility Hunting and Detection Capabilities:

Hunt for malware/anomalies in memory.

### **malfind**: Detects code injection (e.g., file-less malware).

- Syntax: `python3 vol.py -f <file> windows.malfind`.
- Identifies processes with RWE/RX bits or no mapped file.
- Outputs PIDs, offsets, hex/ASCII/disassembly views.
- Indicators: MZ headers (Windows executables), shellcode.

### **yarascan**: Scans memory against YARA rules for strings/patterns.

- Syntax: `python3 vol.py -f <file> windows.yarascan`.
- Use YARA file or inline rules.
- **Prerequisite**: Understand evasion techniques, malware behavior, and hunting methods.

## Advanced Memory Forensics

 Detect advanced malware (e.g., rootkits) using system objects.

### **Hooking Detection**:

**ssdt**: Identifies SSDT (System Service Descriptor Table) hooking.

- Syntax: `python3 vol.py -f <file> windows.ssdt`.
- SSDT: Kernel table for system functions; malware hooks to redirect pointers.
- Note: Legitimate apps may hook; analyze output carefully.

### **modules**: Lists loaded kernel modules.

- Syntax: `python3 vol.py -f <file> windows.modules`.
- May miss idle/hidden drivers.

### **driverscan**: Scans for drivers in kernel (catches hidden drivers).

- Syntax: `python3 vol.py -f <file> windows.driverscan`.
- Often yields no output; use after `modules`.

### **Other Plugins**:

`modscan`, `driverirp`, `callbacks`, `idt`, `apihooks`, `moddump`, `handles`.

Note: Some are Volatility2-only or third-party.