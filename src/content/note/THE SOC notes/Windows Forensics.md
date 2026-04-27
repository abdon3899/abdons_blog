---
title: "Windows Forensics"
description: "My personal SOC notes and research on Windows Forensics."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/win.gif"

---

# Windows Forensics

## **Forensic Artifacts:**

are essential pieces of information that provide evidence of human activity,small footprints of activity left on the computer system.

## **Windows Registry:**

collection of databases that contains the system's configuration data. This configuration data can be about the hardware, the software, or the user's information. you can open it from regedit.exe , [Hive](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-hives#:~:text=Registry%20Hives.%20A%20hive%20is%20a%20logical%20group,with%20a%20separate%20file%20for%20the%20user%20profile.) is a group of Keys, subkeys, and values stored in a single file on the disk.

### **Structure of the Registry:**

- HKEY_CURRENT_USER:  Stores configuration data for the currently logged-in user, including folders, display settings, and Control Panel preferences. Associated with the user profile and abbreviated as HKCU.
- HKEY_USERS: Stores all active user profiles. HKEY_CURRENT_USER is a subkey, abbreviated as HKU.
- HKEY_LOCAL_MACHINE: Stores computer-specific configuration data for all users, abbreviated as HKLM.
- HKEY_CLASSES_ROOT: Is a subkey of `HKEY_LOCAL_MACHINE` that maps file types to programs, abbreviated as HKCR. Since Windows 2000, settings are stored in both `HKEY_LOCAL_MACHINE` (default settings for all users) and `HKEY_CURRENT_USER` (user-specific settings that override defaults). HKEY_CLASSES_ROOT merges these two sources. When writing to HKCR, changes are stored in `HKLM\Software\Classes` unless the key exists in `HKCU\Software\Classes`, in which case changes go there instead.
- HKEY_CURRENT_CONFIG: Stores hardware profile information used during system startup.

## **Registry on the disk:**

The majority of these hives are located in the `C:\Windows\System32\Config` directory and are:

- **DEFAULT** (on `HKEY_USERS\DEFAULT`)
- **SAM** (on `HKEY_LOCAL_MACHINE\SAM`)
- **SECURITY** (on `HKEY_LOCAL_MACHINE\Security`)
- **SOFTWARE** (on `HKEY_LOCAL_MACHINE\Software`)
- **SYSTEM** (on `HKEY_LOCAL_MACHINE\System`)

### user information hives:

NTUSER.DAT (`C:\Users\<username>\AppData\Local\Microsoft\Windows`)

USRCLASS.DAT (`C:\Users\<username>\`)

### **Amcache Hive:**

Windows creates this hive to save information on programs that were recently run on the system. located at `C:\Windows\AppCompat\Programs\Amcache.hve`

### **Transaction Logs and Backups:**

Transaction logs:stored as .LOG files (e.g., SAM.LOG), record recent changes made to registry hives, often including updates not yet reflected in the hives. Multiple logs may exist with extensions like .LOG1, .LOG2, etc. It's important to examine these logs during registry forensics. 

Registry backups: on the other hand, are copies of registry hives stored in the RegBack directory (`C:\Windows\System32\Config\RegBack`). These backups are updated every ten days and can be useful for investigating recently deleted or modified registry keys.

## data acquisition:

when encountering either a live system or a system image. its a best practices that we to create an image or copy of the required data before performing forensic analysis. there are some tools which can help us copy the registers:

### [KAPE](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape) :

is a live data acquisition and analysis tool for acquiring registry data. While it's primarily command-line based, it also includes a GUI interface. The GUI's interface allows users to select settings for registry data extraction. 

### [Autopsy](https://www.autopsy.com/):

gives you the option to acquire data from both live systems or from a disk image. After adding your data source, navigate to the location of the files you want to extract, then right-click and select the Extract File(s) option. 

### [FTK Imager](https://www.exterro.com/ftk-imager):

similar to Autopsy and allows you to extract files from a disk image or a live system by mounting the said disk image or drive in FTK Imager. Another way yo extract Registry files  is through the Obtain Protected Files option. This option allows you to extract all the registry hives to a location of your choosing. However, it will not copy the `Amcache.hve` file, which is often necessary to investigate evidence of programs that were last executed.

## Exploring Windows Registry:

### [AccessData's Registry Viewer](https://accessdata.com/product-download/registry-viewer-2-0-0):

has a similar user interface to the Windows Registry Editor. There are a couple of limitations, though. It only loads one hive at a time, and it can't take the transaction logs into account.

### **Zimmerman's Registry Explorer:**

Registry Explorer, one of Eric Zimmerman's [tools](https://ericzimmerman.github.io/#!index.md), its a powerful forensics utility that can load multiple hives and transaction logs simultaneously. Its 'Bookmarks' feature provides quick access to forensically significant registry keys, streamlining investigations.

### [RegRipper](https://github.com/keydet89/RegRipper3.0):

extracts forensically important data from registry hives and generates a text report with sequential results. It comes in both CLI and GUI versions. Note that RegRipper cannot process transaction logs - Registry Explorer must be used first to merge these with registry hives.

## System Information and System Accounts:

here i’ll mention some things that you should look at first when reading registry data

- **OS Version:** this contains some info. about your os  it could be found at `SOFTWARE\Microsoft\Windows NT\CurrentVersion`
- **Current control set:** Control Sets in the SYSTEM hive manage system startup configuration. There are typically two: ControlSet001 (current boot configuration) and ControlSet002 (last known good configuration). Their locations will be:`SYSTEM\ControlSet001 and 002`
- **Computer Name:**  `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`
- **Computer Name:** found at ****`SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`
- **Time Zone Information:** found at ****`SYSTEM\CurrentControlSet\Control\TimeZoneInformation`
- **Network Interfaces and Past Networks:** found at  `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces` Each interface has a GUID subkey containing TCP/IP settings like IP addresses, DHCP, subnet mask, and DNS servers. The past networks could be found at  `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged or \Managed`
- **AutoStart Programs (Autoruns):** can be found on multiple locations  `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run , NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce , SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce, SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run, SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- **SAM hive and user information:**  The SAM hive stores account, login, and group information at: `SAM\Domains\Account\Users`

## Usage or knowledge of files/folders:

- **Recent Files: Recent Files:** Windows tracks recently opened files for each user through Windows Explorer. This data is stored in the NTUSER hive at: `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` Another interesting piece of information in this registry key is that there are different keys with file extensions, such as `.pdf`, `.jpg`, `.docx`
- **Office Recent Files:** Microsoft Office tracks recently opened documents in the NTUSER hive at: `NTUSER.DAT\Software\Microsoft\Office\VERSION`in Office 365, Microsoft links the location to the user's live id. The recent files can be found at: `NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU`
- **ShellBags:**  Windows saves folder layout preferences per user in the shell settings. These settings track Most Recently Used files and folders and are stored in user hives at: `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags, USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU, NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU, NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags` ShellBag Explorer provides an easy-to-use interface for parsing and viewing ShellBag data from hive files.
- **Open/Save and LastVisited Dialog MRUs:**  Windows remembers file locations from Open/Save dialogs, allowing us to track recently accessed files through these registry keys:`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU, NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`
- **Windows Explorer Address/Search Bars:** Windows Explorer address bar entries and search history can be found in: `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths or WordWheelQuery`

## Evidence of Execution:

- **UserAssist:** Windows tracks applications launched through Windows Explorer in UserAssist registry keys, storing launch times and execution counts. Command line programs are not tracked. Found in NTUSER hive under each user's GUID at:`NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`
- **ShimCache:**  (also called Application Compatibility Cache) tracks application compatibility and execution on Windows. Located in `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`, it stores executable file names, sizes, and last modified times.
- **AmCache:** tracks program executions, storing details like paths, timestamps, and SHA1 hashes. Located at: `C:\Windows\appcompat\Programs\Amcache.hve` Last executed programs can be found at: `Amcache.hve\Root\File\{Volume GUID}\`
- **BAM/DAM:** Background Activity Monitor tracks background application activity, while Desktop Activity Moderator manages device power consumption. Both are components of Windows Modern Standby system. they are founded at `SYSTEM\CurrentControlSet\Services\bam or dam\UserSettings\{SID}`

## External Devices/USB device forensics:

During forensic analysis, identifying USB and removable drives connected to a system is crucial. 

- **Device identification:** These registry locations track connected USB devices by storing their vendor ID, product ID, version, and connection timestamps for device identification. `SYSTEM\CurrentControlSet\Enum\USBSTOR OR USB`
- **First/Last Times:** tracks USB device connection history, including first, last connection, and last removal times: `SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####` where #### is  0064 for first connection time, 0066 for last connection time, and 0067 for last removal time
- **USB device Volume Name:** Drive names can be found at:`SOFTWARE\Microsoft\Windows Portable Devices\Devices`



## **The File Allocation Table (FAT):**

is a file system that indexes file locations on storage devices. Used by Microsoft since the 1970s, it remains supported but is no longer the default file system.  FAT rangs from FAT12 to FAT16 to FAT32, where the increasing number indicates larger maximum storage capacity. FAT12 is very rare nowadays. One limitation of FAT is that its maximum file size is only 4GB. ExFAT serves as an alternative to NTFS but without security features, and is commonly used with SD cards in cameras. 

## **The NTFS file system:**

the New Technology File System (NTFS) to add these features. This file system was introduced in 1993 with the Windows NT 3.1. However, it became mainstream since Windows XP. The NTFS file system resolves many issues present in the FAT file system and introduces a lot of new features. Some of them are 

- **Journaling:** NTFS logs metadata changes in $LOGFILE to help recover from crashes and defragmentation.
- **Access Controls:** NTFS implements user-based access controls for files and directories, unlike FAT.
- **Volume Shadow Copy:** Tracks file changes to enable version restoration. Ransomware often targets these shadow copies to prevent data recovery.
- **Alternate Data Streams:** NTFS feature allowing multiple data streams in one file. Used by browsers to mark downloaded files and can be exploited by malware.
- **Master File Table:** The MFT is NTFS's extensive database for tracking volume objects, more comprehensive than FAT's system. it contain some critical files like
    - **$MFT:** The Master File Table is the first volume record, pointed to by the VBR. It tracks cluster locations and maintains a directory of all volume files.
    - **$LOGFILE:** Maintains file system integrity through transactional logging.
    - **$UsnJrnl:** The Update Sequence Number Journal tracks file changes.
    
    ### **MFT Explorer:**
    
    [GitHub - EricZimmerman/MFTECmd: Parses $MFT from NTFS file systems](https://github.com/EricZimmerman/MFTECmd)
    

## Recovering deleted files:

When a file is deleted, the file system only removes the entry pointing to the file's location on disk. The actual file contents remain until that space is overwritten by new files or disk maintenance. Deleted files may be recoverable from unallocated disk space by analyzing file structures using hex editors.

**Disk Image:** a bit-by-bit copy of a drive which allows investigators to make multiple copies of evidence for analysis while preserving the original disk and avoiding the need for specialized hardware.

use **Autopsy**

## Evidence of Execution:

### **Windows Prefetch files**

Windows stores program execution data in prefetch files (`C:\Windows\Prefetch`) to speed up frequently used applications. These `.pf` files track program launch times, execution counts, and associated files/handles. 

PECmd.exe:

To run Prefetch Parser on a file and save the results in a CSV,  use this command: `PECmd.exe -f <path-to-Prefetch-files> --csv <path-to-save-csv>` , for parsing a whole directory, use this command: `PECmd.exe -d <path-to-Prefetch-directory> --csv <path-to-save-csv>`

### **Windows 10 Timeline:**

The Windows 10 Timeline is an SQLite database that tracks recent application usage and focus time, stored at: `C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db` 

WxTCmd.exe:  `WxTCmd.exe -f <path-to-timeline-file> --csv <path-to-save-csv>`

### **Windows Jump Lists**

Jump lists provide quick access to recently used files from the taskbar through right-clicking an application's icon. The data is stored in: `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations` These lists track application execution times and history through AppIDs.

JLECmd.exe : `JLECmd.exe -f <path-to-Jumplist-file> --csv <path-to-save-csv>`

## File/folder knowledge:

### **Shortcut Files:**

 ****Windows creates shortcut files tracking when files are opened locally or remotely, storing access timestamps and file paths. in: `C:\Users\<username>\AppData\Roaming\Microsoft\Windows or Office\Recent\`

LECmd.exe: `LECmd.exe -f <path-to-shortcut-files> --csv <path-to-save-csv>` Shortcut file timestamps indicate first access (creation date) and last access (modification date).

### **IE/Edge history**

includes files opened in the system as well, whether those files were opened using the browser or not. `C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat` 

## External Devices/USB device forensics:

### Setup api dev logs for USB devices:

 When any new device is attached to a system, information related to the setup of that device is stored in the `setupapi.dev.log`. stored at : `C:\Windows\inf\setupapi.dev.log`

### **Shortcut files:**

shortcut files are created automatically by Windows for files opened locally or remotely It can provide us with information about the volume name, type, and serial number. Recalling from the previous task, this information can be found at: `C:\Users\<username>\AppData\Roaming\Microsoft\Windows - Office\Recent\`