---
title: "macOS Forensics"
description: "My personal SOC notes and research on macOS Forensics."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/apple.gif"

---

# macOS Forensics

## intro:

### NeXTSTEP and MacOS X:

introduced In 2001, classic MacOS was replaced by MacOS X, a Unix-like OS derived from NeXTSTEP (developed by NeXT Computer).

**Impact on HFS+**: HFS+ (Hierarchical File System Plus) lacked Unix features like hard linking and permissions. Updates were made to HFS+ to support these features, ensuring compatibility with MacOS X. These adaptations extended HFS+’s relevance until 2017.

### MacOS X, OS X, and macOS:

**Classic MacOS**: Versions up to MacOS 9 (pre-2001).

**MacOS X (2001–2012)**: Branded with Roman numeral “X” for version 10 (e.g., MacOS X 10.0 Cheetah). Minor version increments: 10.1 (Puma), 10.2 (Jaguar), 10.3 (Panther), etc.

**OS X (2012–2016)**: Rebranded as OS X, dropping “Mac” (e.g., OS X 10.8 Mountain Lion).

**macOS (2016–present)**: Rebranded as macOS for consistency with watchOS, iOS, tvOS, iPadOS (e.g., macOS 10.12 Sierra). Continued 10.xx versioning until macOS Big Sur (macOS 11, 2020). Major version increments post-Big Sur: macOS 12 (Monterey), macOS 13, etc.

**File System Transition**: **HFS+**: Used until 2017; limited by lack of modern features (e.g., SSD optimization, advanced security).

**APFS (Apple File System)**: Previewed: macOS 10.12 Sierra (2016). Adopted: macOS 10.13 High Sierra (2017) and later. Improvements: SSD optimization, enhanced security, modernized features. Now used across all Apple devices (Mac, iPhone, iPad, Apple Watch, Apple TV).

## Hierarchical File System Plus (HFS+):

AKA. macOS Extended.  Primary file system for macOS up to macOS 10.12 Sierra (2016); used in older Apple devices (e.g., iPod) before 2017. Released by Apple in 1998, replacing HFS. Not designed for Unix-like systems; retrofitted for compatibility with Unix-like macOS X (2001) to support features like hard linking and permissions.

### Structure:

**Components**:

- **Sectors**: Smallest unit, typically 512 bytes.
- **Allocation Blocks**: Composed of one or more sectors; addressed using 32-bit addresses (max 2^32 blocks).
- **Boot Volume**: Resides in sectors 0 and 1 for volume booting.
- **Volume Header**: Sector 2; stores metadata (e.g., allocation block size, volume creation timestamp).
- **Allocation File**: Tracks used/unused allocation blocks.
- **Catalog File**: B-tree structure cataloging all files and directories.
- **Extents Overflow File**: Records allocation blocks assigned to each file.
- **Attributes File**: Stores file attributes.
- **File Names**: Encoded in UTF-16, up to 255 UTF-16 code units.

### Limitations:

 By 2017, HFS+ was outdated, replaced by APFS due to significant shortcomings.

**Retrofitted Unix Features**: Lacked native support for Unix-like features (e.g., file permissions, hard links), requiring Apple to add them for macOS X compatibility.

**Date Limitation**: Could not support dates beyond 6 February 2040.

**Concurrency Issues**: Multiple processes could not access the file system concurrently, causing performance bottlenecks, especially on SSDs.

**Timestamp Resolution**: Limited to 1-second resolution (modern systems use nanoseconds).

**No Snapshot Capability**: Lacked features like NTFS’s Volume Shadow Copies, limiting backup and recovery options.

## **Apple File System (APFS):**

Previewed: macOS 10.12 Sierra (2016). Default File System: macOS 10.13 High Sierra (2017) and later. Used Across: macOS, iOS, watchOS, tvOS, iPadOS. **Purpose**: Addresses HFS+ shortcomings, modernizing Apple’s file system with enhanced performance, security, and scalability.

### Structure:

**Partitioning**:

Uses GUID Partition Table (GPT) for disk partitioning. 

**Containers and Volumes**:

One or more **containers** exist within a partition.

Each container holds multiple **volumes**, positioned contiguously without gaps.

**Free Space**: Shared across all volumes within a container.

**Management Tool**:

**`diskutil`**: macOS command-line tool for managing disks and volumes.

Ex.

`diskutil apfs list`: Lists APFS containers and volumes.

`diskutil apfs listUsers, listSnapshots, listVolumeGroups`: Manage APFS-specific features.

### Salient Features:

**Improvements Over HFS+**:

- **Timestamp Resolution**: Supports 1-nanosecond timestamps (vs. HFS+’s 1-second).
- **Date Support**: Handles dates beyond 6 February 2040.
- **Full Disk Encryption**: Native support for robust encryption.
- **Snapshots**: Creates read-only, point-in-time file system instances.
- **Redirect-on-Write**: Crash-protection feature; writes data to new blocks, releasing old ones after completion to prevent corruption.
- **File Capacity**: Supports up to 2^63 files (vs. HFS+’s 2^32 allocation blocks).
- **Forensic Challenges**: Full disk encryption and hardware/software encryption complicate forensic investigations.

## Directory Structure and Domains:

macOS has a Unix-like operating system, resulting in a directory structure with similarities to Unix/Linux systems.

### Directory Structure:

The root directory (`/`) contains several directories familiar to Unix/Linux users.

- **opt**: Stores files for optional software, such as Homebrew.
- **sbin**: Contains system binaries like launchd, ping, and mount.
- **bin**: Holds essential binaries such as chmod, rm, and echo.
- **dev**: Includes device files, such as those for Bluetooth accessories.
- **private**: Contains three main subdirectories:
    - **var**: Stores variable files, like logs.
    - **etc**: Holds configuration files, like the hosts file.
    - **tmp**: Contains temporary files.
    - Modifying these directories often requires sudo privileges.

### Domains in macOS:

macOS organizes files into four domains based on their intended use: Local, System, User, and Network.

**- Local Domain:**

Contains resources shared by all users on the local computer. Located in the `/Applications` and `/Library` directories.

- **`/Applications`**: Stores user applications like Discord, Adobe Reader, and Python.
- **`/Library`**: Holds configuration files shared across all users.
- **`/Developer`**: May exist within /Applications or /Library, containing files for the Xcode utility.

Managed by the system but modifiable by users with administrative privileges.

**-System Domain:**

Contains Apple-developed software and configurations. Located in the `/System` directory. Users, even with root privileges, cannot modify or remove files in this domain.

**-User Domain:**

Contains user-specific data and files. Located in the `/Users` directory, with a subdirectory for each user (e.g., `/Users/<user>`). ncludes a hidden `/Users/<user>/Library` directory for user-specific configurations and application data. By default, users cannot access another user’s files.

**-Network Domain:**

Contains network resources like network printers, SMB share servers, and other computers. Managed by network administrators, shared among local network users.

## File Formats:

### .plist Files:

 Property list files that store system configurations, similar to the Windows Registry. Critical for system settings and forensic investigations due to their configuration data.

come in 2 different formats:  **XML**: Readable with a text editor. **Binary Large Object (BLOB)**: Requires Xcode (available via the App Store) to read.

### .app Files:

Application executables typically located in the /Applications directory.  Launch applications when executed, similar to .exe files in Windows.  Bundled files; contents can be viewed by selecting “Show Package Contents” from the right-click menu.

### .dmg Files:

 Disk image files used in macOS. Easily mounted; commonly used for installers.  Can be formatted with Apple-supported file systems (e.g., APFS, HFS+, FAT).

### .kext Files:

 Kernel extension files, analogous to drivers in Windows. Provide third-party apps access to the macOS kernel. Deprecated in newer macOS versions (e.g., post-Big Sur).

**Security**: Loading third-party .kext files requires: User permission. Restart in recovery mode. Enabling kernel extension loading.

### .dylib Files:

Dynamically loaded library files containing shared code used by multiple programs. Similar to .dll files in Windows or .so files in Linux.

### .xar Files:

eXtensible ARchive files used for installers or browser extensions.  Replaced .pkg (Package installer) files.

## Forensic Challenges:

Apple’s macOS is designed with features that make forensic investigations difficult, resisting efforts to simplify data acquisition, even under pressure from entities like US government agencies.

### Challenges in Acquiring a macOS Disk Image:

**Access to Physical Disk:**

Gaining physical access to SSDs in macOS devices is challenging. In newer MacBooks, SSDs are soldered to the motherboard, making removal risky and potentially damaging to the drive or device. Older devices with removable SSDs use proprietary interfaces, complicating connection to imaging hardware.

**Hardware Encryption:**

SSDs are encrypted with hardware-based encryption, preventing data recovery without the original hardware. Starting with T2-based chips, encryption keys are stored in a secure enclave separate from the CPU. If the motherboard fails or is unavailable, the encrypted SSD data is unrecoverable. Removing the SSD for imaging is ineffective without the encryption key.

**FileVault Encryption:**

FileVault adds a user-password-based encryption layer, blocking data access without credentials. Data remains encrypted until unlocked with the user’s password or an organization’s access key. Even booting into recovery mode (ideal for imaging) requires unlocking FileVault to access data. Investigators can acquire a disk image, but it remains encrypted without the password or key.

**Full Disk Access:**

 Live system imaging requires granting Full Disk Access (FDA) to imaging tools. macOS restricts access to certain disk areas unless explicitly allowed. FDA is granted via Settings > Privacy & Security > Full Disk Access. Without FDA, live imaging tools cannot access all necessary data.

**System Integrity Protection:**

System Integrity Protection (SIP) restricts access to critical system components. SIP prevents unauthorized kernel access, code injection, debugging, or modifications, even with root privileges. SIP may block access to memory or disk areas during imaging. Disabling SIP requires booting into recovery and running csrutil disable in the terminal, but this risks losing volatile data or altering the disk.

### Possibilities for Mac Acquisition:

 Despite challenges, forensic data acquisition from macOS is possible through specific methods.

**Methods**:

**Proprietary Tools**: 

Use tools like Magnet AXIOM or Cellebrite, grant Full Disk Access, and image a live system.

**Recovery Mode with Password**:

If the user password is known and the device is accessible, boot into recovery, disable security features (e.g., SIP), and use terminal tools like dd, hdiutil, or dc3dd for imaging. dd and dc3dd imaging is covered in the “Forensic Imaging” room. This is the best method without proprietary tools.

**Mac Sharing or Target Mode**:

After booting into recovery and unlocking the drive, enable Mac Sharing Mode (for logical acquisition) or Target Mode (for full disk imaging) via Firewire or Thunderbolt connection to another device. Limitation: Target Mode is unavailable on newer Macs with Apple silicon chips.

## Mounting APFS with apfs-fuse:

Use the open-source apfs-fuse utility to mount an APFS-formatted drive on a Linux system, enabling access to macOS disk images.nA macOS disk image (mac-disk.img) is provided in the home directory of a Linux VM for exploration.

### Checking APFS Container Info

apfsutil is used to list volumes in the APFS container. `apfsutil mac-disk.img` Lists volumes (e.g., System, Preboot, Recovery, Data, VM) with UUIDs, names, capacity consumed, and FileVault status.

### Mounting Using APFS-Fuse

 Root permissions are needed to mount the disk image.  `apfs-fuse` mounts images as read-only (no write capabilities). Instructions for assigning read permissions to non-root users are in the GitHub readme, but root is used here.

 Mount the disk image to the mac/ directory.`*apfs-fuse mac-disk.img mac/*` or `*apfs-fuse -v 4 mac-disk.img mac*`

## tools:

To effectively analyze macOS forensic artifacts, understanding the primary artifact types and their associated tools is critical. Below is a concise summary of the key artifact types—Plist Files, Database Files, and Logs—and the tools/techniques used to parse them, based on the provided information.

### Plist Files:

property list (plist) files are common macOS artifacts, storing configuration data in either XML or binary (BLOB) format.

**XML Plist Files**: Readable with standard utilities like cat, more, or head on any system.

**BLOB Plist Files**:

**macOS**: Use the built-in `plutil` utility with the command:

**Linux**: Use `plistutil` (must be installed) with the command: `plistutil -p <file>.plist`

### Database Files:

Artifacts like chat history, browsing history, and application usage are stored in database formats, typically SQLite.

**DB Browser for SQLite**: A cross-platform tool (Windows, Linux, macOS) for querying SQLite databases.

**APOLLO**: A Python-based tool for collecting and parsing macOS databases into a unified database. Supports multiple modules to identify queries for specific artifacts and create event timelines. `python3 apollo.py`

### Logs:

macOS logs provide critical forensic data and are categorized into three types: 

**Apple System Logs (ASL):**

Location: `/private/var/log/asl/` , Contains logs like utmp, wtmp, and login details.

**macOS (Live System)**: `open -a Console /private/var/log/asl/<log>.asl`

**Linux/Windows**: Use mac_apt, a Python-based tool for parsing ASL and other artifacts into CSV, JSON, or SQLite formats. `python3 mac_apt.py`

**System Logs:**

Location: `/private/var/log/system.log` ,  Similar to Linux syslog, stored in plain text but rotated into .gz files.

**Reading**: Use text editors or utilities like cat, more, or head.

**Searching Compressed Logs**: Use zgrep to search across rotated .gz files: `zgrep BOOT_TIME system.log*`

**Unified Logs:**

Location: `/private/var/db/diagnostics/*.tracev3, /private/var/db/uuidtext` , Comprehensive logs in a proprietary format, requiring specialized parsing.

**macOS (Live System)**: Use the built-in log utility: `log show --last 1m`

**Linux/Windows or Offline Analysis**: mac_apt: Parse Unified Logs into CSV, JSON, or SQLite formats (as with ASL). **Mandiant’s Unified Log Parser**: Convert .tracev3 logs to CSV/JSON. Requires compilation from source on the VM.

## System Information:

### OS Version

found in `/System/Library/CoreServices/SystemVersion.plist` , A plist file containing OS details, typically in XML format.

**Access Method**:

Use cat to read XML-format plist: `cat /System/Library/CoreServices/SystemVersion.plist`

For binary plist files, use: `plutil -p /System/Library/CoreServices/SystemVersion.plist`

### Serial Number

found at `/private/var/folders/*/<DARWIN_USER_DIR>/C/locationd/consolidated.db`  or `/private/var/folders/*/<DARWIN_USER_DIR>/C/locationd/cache_encryptedA.db` , Stored in SQLite databases under the TableInfo table.

Use **DB Browser for SQLite**:

Open the database. Navigate to **Browse Data** tab. Select **TableInfo** table to view the serial number.

### OS Installation Date

found at `/private/var/db/.AppleSetupDone`   or `/private/var/db/softwareupdate/journal.plist` .AppleSetupDone: File timestamps indicate OS installation (Birth) and update (Change) dates. journal.plist: Provides detailed installation and update history.

**Using stat on .AppleSetupDone**: `stat -x /private/var/db/.AppleSetupDone`

- On Linux, use stat without -x.

**Using cat on journal.plist**: `cat /private/var/db/softwareupdate/journal.plist`

### Time Zone

found at `/etc/localtime`  or `/Library/Preferences/.GlobalPreferences.plist`   or `/Library/Preferences/com.apple.timezone.auto.plist`

`/etc/localtime:` Symlink to the current time zone. .`GlobalPreferences.plist:` Includes current and historical time zones/languages. `com.apple.timezone.auto.plist:` Indicates if auto time zone adjustment is active.

**Check current time zone**: `ls -la /etc/localtime`

**Check time zone history**: `plutil -p /Library/Preferences/.GlobalPreferences.plist`

**Check auto time zone**: `plutil -p /Library/Preferences/com.apple.timezone.auto.plist`

### Boot, Reboot, and Shutdown Times

found at `/private/var/log/system.log (and rotated .gz files)`  or  `Unified Logs (/private/var/db/diagnostics/*.tracev3, /private/var/db/uuidtext)`

**`System Logs**:` Contain BOOT_TIME and SHUTDOWN_TIME entries with local and Epoch timestamps.

**`Unified Logs**:` Include boot, shutdown, screen lock/unlock, and login events, filterable by loginwindow subsystem.

**System Logs**:

`zgrep BOOT_TIME system.log.*` or `zgrep SHUTDOWN_TIME system.log.*`

**Unified Logs**:

**On a live system**: `log show --info --predicate 'eventMessage contains "com.apple.system.loginwindow" and eventMessage contains "SessionAgentNotificationCenter"'`

**Offline analysis**: Use **DB Browser for SQLite** to query Unified Logs, filtering by loginwindow subsystem. View events like screen lock, Touch ID, or WakeFromSleep with timestamps. Alternatively, use mac_apt or Mandiant’s Unified Log Parser to convert logs to CSV/JSON.

## Network information:

### Network Interfaces

found at `/Library/Preferences/SystemConfiguration/NetworkInterfaces.plist` , A plist file (typically XML) listing all network interfaces, their types, and configurations.

**Read with cat for XML format:**`cat /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist`

**binary plist, use:** `plutil -p /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist`

**Key Information**:

BSD Name: Interface identifier (e.g., en0, en1). SCNetworkInterfaceType: Type of interface (e.g., IEEE80211 for Wi-Fi, Ethernet for wired). UserDefinedName: Human-readable name (e.g., Wi-Fi, Thunderbolt 1). IOMACAddress: MAC address of the interface.

### DHCP Settings

found at `/private/var/db/dhcpclient/leases/<interface>.plist (e.g., en0.plist)` , A plist file (typically XML) containing DHCP lease details for a specific network interface.

**Read with cat (requires sudo due to permissions):** `sudo cat /private/var/db/dhcpclient/leases/ <itrfacename>.plist`

**binary plist, use:** `sudo plutil -p /private/var/db/dhcpclient/leases/<itrfacename>.plist`

**Key Information**:

IPAddress: Assigned IP address (e.g., 192.168.1.79). LeaseLength: Duration of the lease in seconds (e.g., 86400 = 24 hours). RouterIPAddress: Gateway/router IP (e.g., 192.168.1.1). SSID: Wi-Fi network name (for wireless interfaces, e.g., 402-5G).

### Wireless Connections

found at `/Library/Preferences/com.apple.wifi.known-networks.plist` , A plist file (often binary) storing historical Wi-Fi connection details, including SSIDs, connection dates, and geolocation data.

**Read with plutil (requires sudo):** `sudo plutil -p /Library/Preferences/com.apple.wifi.known-networks.plist`

### Network Usage

found at `Unified Logs (/private/var/db/diagnostics/*.tracev3, /private/var/db/uuidtext)` , Unified Logs capture network connection events, including SSIDs, DHCP leases, and network changes.

**On a Live System**: Filter logs with log show: `log show --info --predicate 'senderImagePath contains "IPConfiguration" and (eventMessage contains "SSID" or eventMessage contains "Lease" or eventMessage contains "network changed")'`

**Offline Analysis**: Convert Unified Logs to CSV using Mandiant’s Unified Log Parser: `./unifiedlog_parser -i system_logs.logarchive -o logs/output1.csv`

Search the CSV for keywords like IPConfiguration, SSID, or network changed using tools like grep or Excel. Alternatively, use mac_apt to parse Unified Logs into CSV/JSON/SQLite and query with DB Browser.

## Account activity:

### User Accounts and Passwords

 found at `/private/var/db/dslocal/nodes/Default/users/<user>.plist` ,A plist file (often binary) for each user, containing account details, password metadata, and iCloud associations. Timestamps are in Unix Epoch format.

**Read with cat for XML format (requires sudo):** `sudo cat /private/var/db/dslocal/nodes/Default/users/john.plist`

**binary plist, use:** `sudo plutil -p /private/var/db/dslocal/nodes/Default/users/john.plist`

**Key Information**:

creationTime: When the account was created (e.g., 1739202907 = Feb 14, 2025, ~07:15 UTC). failedLoginCount: Number of failed login attempts. failedLoginTimestamp: Time of last failed login (0 if none). passwordLastSetTime: When the password was last changed. iCloud account details (if linked).

### User Login History

found at  `/Library/Preferences/com.apple.loginwindow.plist` , A plist file (often binary) storing details about recent logins and guest account status.

**Read with plutil:** `plutil -p /Library/Preferences/com.apple.loginwindow.plist`

**Key Information**:

lastUser: Current login status (e.g., loggedIn indicates a live system). lastUserName: Last logged-in user (e.g., umair). RecentUsers: List of recently logged-in users (e.g., umair, john). GuestEnabled: Guest account status (e.g., 0 = disabled).

### SSH Connections

found at  `/users/<user>/.ssh/known_hosts`, text file listing IP addresses and public keys of hosts connected via SSH, similar to Linux.

**Read with cat:** `cat /users/umair/.ssh/known_hosts`

**Key Information**:

IP Address: Remote host connected via SSH (e.g., 192.168.1.152). Public Key: Key type and value (e.g., ssh-ed25519, ssh-rsa).

### Privileged Accounts

found at  `/etc/sudoers` ,text file defining users and groups with sudo privileges, similar to Linux.

**Read with cat (requires sudo):**`sudo cat /etc/sudoers`

**Key Information**:

root: Full sudo privileges. %admin: Users in the admin group have sudo privileges. /private/etc/sudoers.d: Additional sudo configurations (if any).

### Login and Logout Events

found at **System Logs**: `/private/var/log/system.log (and rotated .gz files)` **Apple System Logs (ASL)**: `/private/var/log/asl/` ,Logs capturing login (USER_PROCESS) and logout (DEAD_PROCESS) events.

**Search with zgrep:**`zgrep login system.log*`

**ASL**: Convert ASL to CSV using mac_apt, then search: `grep USER_PROCESS asl_ver2.csv`

### Screen Lock/Unlock Events

found at Unified Logs (`/private/var/db/diagnostics/*.tracev3, /private/var/db/uuidtext`) ,Unified Logs capture screen lock (com.apple.sessionagent.screenIsLocked) and unlock (com.apple.sessionagent.screenIsUnlocked) events.

**On a Live System**: Filter with log show (not shown in provided example but implied): `log show --info --predicate 'eventMessage contains "com.apple.sessionagent.screenIsLocked" or eventMessage contains "com.apple.sessionagent.screenIsUnlocked"'`

**Offline Analysis**: Convert Unified Logs to CSV using mac_apt or Unified Log Parser, then search: `grep com.apple.sessionagent.screenIsLocked output.csv`