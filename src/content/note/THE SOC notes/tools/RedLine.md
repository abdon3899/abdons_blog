---
title: "RedLine"
description: "My personal SOC notes and research on RedLine."
publishDate: "2026-04-19T00:00:00Z"
tags: ["tools"]
---

# RedLine

robust **incident response and memory analysis tool** developed by FireEye (now Trellix), designed to help cybersecurity professionals investigate and analyze suspicious activities on endpoints, particularly Windows systems.

1. **Choose a Data Collection Method**:
    - **Standard Collector**: Quick, minimal data collection (preferred for this task).
    - **Comprehensive Collector**: Extensive data collection, takes longer (up to an hour or more).
    - **IOC Search Collector (Windows only)**: Collects data matching specific Indicators of Compromise (IOCs).
2. **Launch Redline**:
    - Open Redline and select **"Create a Standard Collector"**.
    - Choose the target platform (e.g., Windows).
3. **Configure the Script**:
    - Click **"Edit your script"** to customize data collection options across five tabs:
        - **Memory**: Process listings, drivers enumeration (uncheck Hook Detection and Acquire Memory Image for this exercise).
        - **Disk**: Disk partitions, volumes, and file enumeration.
        - **System**: Machine/OS info, registry hives, user accounts, prefetch cache (Windows-specific options).
        - **Network**: Network info, browser history (useful for investigating malicious activities).
        - **Other**: Services, tasks, and file hashes.
    - Click **OK** after configuring.
4. **Save the Collector**:
    - Click **"Browse"** to select an **empty folder** (e.g., "Analysis") to save the collector script and analysis file.
    - A **RunRedlineAudit.bat** file will be created in the folder.
5. **Run the Script**:
    - Execute the **RunRedlineAudit.bat** script as an Administrator.
    - A command prompt window will open, indicating the script is running. This process takes **15-20 minutes**.

1. **What is an IOC?**
    - **Indicators of Compromise (IOCs)** are artifacts that indicate potential system or network intrusions, such as file hashes (MD5, SHA1, SHA256), IP addresses, domain names, file paths, registry keys, or unique strings.
2. **Using IOC Editor**:
    - Create and customize IOC files (e.g., `Keylogger.ioc`) using **IOC Editor**.
    - Define specific IOCs like file names, hashes, or strings.
    - Save the IOC file for use in Redline.
3. **IOC Search Collector in Redline**:
    - Select **IOC Search Collector** in Redline.
    - Load the `.ioc` file you created.
    - Redline will automatically detect and list the IOCs to search for.
4. **Configure Data Collection**:
    - Click **"Edit your script"** to choose what data to collect (e.g., memory, disk, system, network).
    - Save the collector to an empty folder, generating a `RunRedlineAudit.bat` file.
5. **Run the Collector**:
    - Execute the `RunRedlineAudit.bat` file as Administrator.
    - Wait for the analysis to complete (typically 15-20 minutes).

[https://www.youtube.com/watch?v=tCIEYCWTdk4](https://www.youtube.com/watch?v=tCIEYCWTdk4)