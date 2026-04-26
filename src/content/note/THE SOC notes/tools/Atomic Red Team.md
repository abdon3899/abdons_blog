---
title: "Atomic Red Team"
description: "My personal SOC notes and research on Atomic Red Team."
publishDate: "2026-04-19T00:00:00Z"
tags: ["tools"]
---

# Atomic Red Team

an open-source framework developed by **Red Canary** for security testing and adversary emulation. It allows security teams to test their defenses by executing small, focused "atomic" tests that mimic real-world attack techniques aligned with the **MITRE ATT&CK®** framework.

## **Key Features of Atomic Red Team**

**Modular & Lightweight**

- Each test is self-contained and focuses on a single ATT&CK technique.
- No need for complex infrastructure—tests can run on individual machines.

**Cross-Platform Support**

- Works on **Windows, Linux, macOS, and cloud environments (AWS, Azure, GCP)**.
- Supports **containers (Kubernetes)** and **SaaS platforms (Office 365, Google Workspace, Azure AD)**.

**MITRE ATT&CK-Aligned**

- Each test maps to a specific ATT&CK technique (e.g., `T1003.001` for LSASS memory dumping).
- Helps validate detection capabilities against known adversary behaviors.

**Multiple Execution Methods**

- Supports **PowerShell, Bash, CMD, and manual execution**.
- Can be automated or run interactively.

**Self-Documenting Tests**

- Each test includes:
    - **Description** of the technique.
    - **Prerequisite checks** (e.g., verifying tools like Mimikatz or ProcDump).
    - **Cleanup commands** to revert changes after testing.

## **Atomic Test File Anatomy**

Each MITRE ATT&CK technique (e.g., `T1003.001`) consists of two files:

1. **YAML File (`[Technique_ID].yaml`)**
    - *Execution blueprint* containing commands, dependencies, and cleanup instructions.
2. **Markdown File (`[Technique_ID].md`)**
    - *Documentation* with technique details and detection guidance.

```jsx
attack_technique: T1003.001               # MITRE ATT&CK Technique ID
display_name: "OS Credential Dumping: LSASS Memory"
atomic_tests:                             # List of test variations
  - name: "Dump LSASS.exe Memory using ProcDump"
    description: |
      Uses Sysinternals ProcDump to dump LSASS memory
      for offline credential theft.
    supported_platforms: [windows]        # Compatible OS
input_arguments:
  output_file:
    description: "Path for memory dump"
    type: Path
    default: "C:\Windows\Temp\lsass.dmp"
dependencies:
  - description: "ProcDump must exist at specified path"
    prereq_command: "if (Test-Path #{procdump_exe}) {exit 0} else {exit 1}"
    get_prereq_command: |
      Invoke-WebRequest "https://download.sysinternals.com/files/Procdump.zip" -OutFile "$env:TEMP\Procdump.zip"
      Expand-Archive $env:TEMP\Procdump.zip -DestinationPath (Split-Path #{procdump_exe})
executor:
  name: command_prompt                 # Alternatives: powershell, sh, manual
  command: "#{procdump_exe} -accepteula -ma lsass.exe #{output_file}"
  cleanup_command: "del #{output_file} >nul 2>&1"
  elevation_required: true             # Needs admin privileges
```

*Usage*: Replace `#{output_file}` in commands with this value.

**prereq_command**: Validation check (exit 0 = success).

**get_prereq_command**: Auto-download/install if missing.

**cleanup_command** : Critical for reverting system changes.

This guide provides a comprehensive walkthrough of the Invoke-AtomicRedTeam PowerShell module, the official tool for executing Atomic Red Team tests in Windows environments.

## starting with atomic red

### setup:

- **Basic Setup:**
    
    # Start PowerShell with execution policy bypass
    `powershell -ExecutionPolicy Bypass`
    # Import the module (adjust path as needed)
    `Import-Module "C:\Tools\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force`
    # Set the path to your atomics folder
    `$PSDefaultParameterValues = @{"Invoke AtomicTest:PathToAtomicsFolder"="C:\Tools\AtomicRedTeam\atomics"}`
    
- **Verifying Installation:**
    
    # Check available commands
    `Get-Command -Module Invoke-AtomicRedTeam`
    # View help for the main cmdlet
    `help Invoke-AtomicTest`
    

### **Discovering and Analyzing Atomic Tests**

- **Listing Available Tests:**
    
    # List all tests for a technique (brief)
    `Invoke-AtomicTest T1053 -ShowDetailsBrief`
    # View detailed test information
    `Invoke-AtomicTest T1053.005 -ShowDetails`
    
- **Understanding Test Structure**
    
    Each test contains:
    
    **Description**: What the test does
    
    **Attack Commands**: Actual commands executed
    
    **Dependencies**: Required files/tools
    
    **Cleanup Commands**: How to revert changes
    

### **Managing Test Prerequisites**

- **Checking Requirements:**
    
    # Verify if prerequisites are met
    `Invoke-AtomicTest T1127 -CheckPrereqs`
    
- **Automatically Fulfilling Prerequisites:**
    
    # Download and install required components
    `Invoke-AtomicTest T1127 -GetPrereqs`
    
    > Note: Some tests require internet access to download tools.
    > 

### **Executing Atomic Tests**

- **Basic Execution Methods:**
    
    # Run all tests for a technique
    `Invoke-AtomicTest T1053.005`
    # Run specific tests by number
    `Invoke-AtomicTest T1053.005 -TestNumbers 1,2`
    # Run tests by name
    `Invoke-AtomicTest T1053.005 -TestNames "Scheduled Task Startup Script"`
    # Run tests by GUID
    `Invoke-AtomicTest T1053.005 -TestGuids "3fc9fea2-871d-414d-8ef6-02e85e322b80"`
    
- **Advanced Execution Options:**
    
    # Run in interactive mode (prompt for each step)
    `Invoke-AtomicTest T1053.005 -Interactive`
    # Specify custom input arguments
    `Invoke-AtomicTest T1053.005 -InputArgs @{"TaskName"="MyCustomTask"}`
    # Set execution timeout (seconds)
    `Invoke-AtomicTest T1053.005 -TimeoutSeconds 30`
    

### **Cleanup and Post-Test Actions**

- **Cleaning Up After Tests:**
    
    # Execute cleanup commands
    `Invoke-AtomicTest T1053.005 -Cleanup`
    # Verify cleanup (example for scheduled tasks)
    `schtasks /query | findstr "T1053"`
    
- **Logging and Output Management:**
    
    # Enable execution logging
    `Invoke-AtomicTest T1053.005 -ExecutionLogPath "C:\logs\atomic_execution.log"`
    # Keep standard output/error files
    `Invoke-AtomicTest T1053.005 -KeepStdOutStdErrFiles`
    

### **tips and tricks:**

**Always review tests** with `ShowDetails` before execution

**Run in a test environment** first to understand impacts

**Use cleanup commands** religiously to maintain system integrity

**Combine with detection testing** to validate security controls

**Document your tests** including:

- Execution time
- Expected outcomes
- Actual results
- Detection effectiveness

### useful links:

https://www.atomicredteam.io/

https://github.com/redcanaryco/atomic-red-team