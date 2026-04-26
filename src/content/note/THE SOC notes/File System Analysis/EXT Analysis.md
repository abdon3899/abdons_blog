---
title: "EXT Analysis"
description: "My personal SOC notes and research on EXT Analysis."
publishDate: "2026-04-19T00:00:00Z"
tags: ["File System Analysis"]
---

# EXT Analysis

## EXT File System ( EXT4)

The Extended File System (EXT), introduced in April 1992, was the first file system designed for Linux, incorporating the Virtual File System (VFS).

### **Evolution**:

- **EXT**: Initial version, supported 2 GB file systems.
- **EXT2**: Non-journaling, improved scalability.
- **EXT3**: Added journaling for reliability.
- **EXT4**: Modern standard with larger file/volume sizes and enhanced features.

| Feature | EXT2 | EXT3 | EXT4 |
| --- | --- | --- | --- |
| **Maximum File Size** | 2 TiB | 2 TiB | 16 TiB |
| **Maximum Volume Size** | 32 TiB | 32 TiB | 1 EiB |
| **Journaling** | No | Yes (metadata, optional data) | Yes (with checksums) |
| **Backward Compatibility** | N/A | Can mount EXT2 | Can mount EXT2 & EXT3 |
- **EXT4 Advantages**:
    - Supports larger files/volumes.
    - Enhanced journaling with checksums for integrity.
    - Backward compatibility with EXT2/EXT3.

### **Common Components**:

- **Superblock**: Metadata about the file system (e.g., size, status).
- **Inodes**: Metadata for files (e.g., ownership, permissions, data block pointers).
- **Block Groups**: Groups of blocks for data storage.
- **Data Blocks**: Store file content.
- **Bitmaps**: Track allocated/free blocks and inodes.
- **File Creation Process**:
    1. Allocate an inode for metadata (permissions, ownership, timestamps).
    2. Write file content to data blocks.
    3. Link file name to inode in directory structure.
    4. Update superblock, bitmaps, and metadata for consistency.

### **EXT4 File System Structure:**

**Block Organization**:

Divides disk into equal-sized blocks (e.g., 1024, 2048, or 4096 bytes). Blocks grouped into **block groups** with consistent block counts (except the last group). Blocks numbered from 0 to N.

**Key Structures**:

- **Superblock**: Defines file system metadata (e.g., block size, inode count).
- **Inodes**: Store file metadata and pointers to data blocks.
- **Directory Structures**: Link file names to inodes.
- **Bitmaps**: Track block/inode allocation status.

### **Superblock Analysis:**

Stores critical file system metadata (e.g., block size, inode count, free blocks).

- **Structure** (from Linux kernel ext4_super_block):
    
    `struct ext4_super_block {
        __le32 s_inodes_count;         */* Inodes count */*
        __le32 s_blocks_count_lo;      */* Blocks count */*
        __le32 s_r_blocks_count_lo;    */* Reserved blocks count */*
        __le32 s_free_blocks_count_lo; */* Free blocks count */*
        __le32 s_free_inodes_count;    */* Free inodes count */*
        __le32 s_first_data_block;     */* First Data Block */*
        __le32 s_log_block_size;       */* Block size */*
        __le32 s_log_cluster_size;     */* Allocation cluster size */*
        __le32 s_blocks_per_group;     */* # Blocks per group */*
        __le32 s_clusters_per_group;   */* # Clusters per group */*
        __le32 s_inodes_per_group;     */* # Inodes per group */*
        __le32 s_mtime;                */* Mount time */*
        __le32 s_wtime;                */* Write time */*
        __le16 s_mnt_count;            */* Mount count */*
        __le16 s_max_mnt_count;        */* Maximal mount count */*
        __le16 s_magic;                */* Magic signature */*
        __le16 s_state;                */* File system state */*
        __le16 s_errors;               */* Behavior when detecting errors */*
        __le16 s_minor_rev_level;      */* Minor revision level */*
        __le32 s_lastcheck;            */* Time of last check */*
        __le32 s_checkinterval;        */* Max. time between checks */*
        __le32 s_creator_os;           */* OS */*
        __le32 s_rev_level;            */* Revision level */*
        __le16 s_def_resuid;           */* Default UID for reserved blocks */*
        __le16 s_def_resgid;           */* Default GID for reserved blocks *//* ... */*
    };`
    

**Key Field: s_log_block_size**:

Located at offset 0x18 (7th field, 4 bytes). Represents block size as 2^(10 + s_log_block_size) bytes.

**Analysis Tools**:

- **Hexdump**:
    - Command: `sudo dd if=/dev/loop0 bs=1024 count=1 skip=1 | hexdump -C`
    - Example Output: s_log_block_size = 02000000 (little-endian → 00000002).
    - Calculation: 2^(10 + 2) = 4096 bytes (block size).
- **dumpe2fs**:
    - Command: `sudo dumpe2fs /dev/loop0`
    - Output: Human-readable metadata (block size, block count, inodes, free inodes, inode size).

**Forensic Value**:

Reveals file system configuration (e.g., block size, available inodes). Detects tampering or corruption in superblock metadata.

### **Inode Analysis with debugfs:**

**Tool**: `debugfs` (interactive file system debugger).

**Command**: `sudo debugfs /dev/loop0`

- **Inspecting Inodes**:
    
    `Inode: 2   Type: directory    Mode:  0755   Flags: 0x80000
    Generation: 0    Version: 0x00000000:00000003
    User:     0   Group:     0   Project:     0   Size: 4096
    File ACL: 0
    Links: 3   Blockcount: 8
    Fragment:  Address: 0    Number: 0    Size: 0
    ctime: 0x675789e1:914449a4 -- Tue Dec 10 00:22:57 2024
    atime: 0x675789e2:9c794070 -- Tue Dec 10 00:22:58 2024
    mtime: 0x675789e1:914449a4 -- Tue Dec 10 00:22:57 2024
    crtime: 0x67490506:00000000 -- Fri Nov 29 00:04:22 2024
    Size of extra inode fields: 32
    Inode checksum: 0xdad691c3
    EXTENTS:
    (0):15
    (END)`
    

**Key Fields**:

**Inode Number**: Unique identifier (e.g., 2).

**Type**: Directory or file.

**Mode**: Permissions (e.g., 0755).

**Timestamps**:

- ctime: Last metadata change.
- atime: Last access.
- mtime: Last content modification.
- crtime: Creation time.

**Size**: Directory size (e.g., 4096 bytes).

**Blockcount**: Number of blocks allocated.

**Inode Checksum**: Validates inode integrity.

**Forensic Value**:

Timestamps support timeline analysis. Permissions and ownership reveal access controls. Checksum detects tampering.

### **Verifying EXT4 Partition:**

 EXT4 partition on /dev/loop0, mounted at /mnt/ext4_partition.

- **Command**: `sudo lsblk`
    
    `NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    loop0         7:0    0   100M  0 loop /mnt/ext4_partition
    loop1         7:1    0  25.2M  1 loop /snap/amazon-ssm-agent/7993
    loop2         7:2    0  26.3M  1 loop /snap/amazon-ssm-agent/9881
    loop3         7:3    0     4K  1 loop /snap/bare/5
    (...)
    nvme0n1     259:0    0    40G  0 disk 
    └─nvme0n1p1 259:1    0    40G  0 part /`
    

**Forensic Value**: Confirms the presence and mount point of the EXT4 partition for analysis.

## Forensic Analysis of EXT

### **EXT4 Overview**

**Purpose**: Primary Linux file system (EXT4) with inodes, journaling, and data blocks for efficient storage and forensic artifacts.

**Key Components**:

- **Inodes**: Metadata for files/directories (permissions, timestamps, data block pointers).
- **Journaling**: Logs changes to prevent corruption and track activity.
- **Data Blocks**: Store file content, recoverable if not overwritten.
- **Superblock**: File system metadata (block size, inode count).
- **Bitmaps**: Track block/inode allocation.

### **Inode Metadata**

 Stores file/directory info (e.g., i_mode, i_uid, i_size, timestamps: i_atime, i_mtime, i_ctime, i_crtime, i_dtime).

**Analysis**:

- **Command**: ls -al /mnt/ext4_partition (list files).
- **Command**: sudo stat test_file2.txt (view inode metadata, e.g., inode 11, permissions, timestamps).
- **Command**: sudo debugfs /dev/loop0, then stat <11> (detailed inode data: permissions 0755, timestamps, block pointers).

**Example Change**:

Change permissions: sudo chmod 755 /mnt/ext4_partition/test_file2.txt. Re-run debugfs: Updates i_mode (to 0755), i_ctime, and checksum.

**Forensic Value**:

Timestamps for timeline analysis. Permissions/ownership for access control. i_dtime for deleted files.

### **Recovering Deleted Files**

 Deleted files persist until overwritten; recoverable via inode or content search.

**Process**:

1. **Search Content**: sudo strings -t d /dev/loop0 | grep -i "AAAAAAAA" (finds offset, e.g., 100671488).
2. **Calculate Block**: echo $((100671488 / 4096)) (block 24578, using 4096-byte block size).
3. **Extract Block**: sudo dd if=/dev/loop0 bs=4096 skip=24578 count=1 of=/tmp/recovered_file.
4. **Verify**: cat /tmp/recovered_file (e.g., shows AAAAAAAA, BBBBBBBB, etc.).

**Forensic Value**: Recovers deleted files using signatures or inode numbers.

### **Journaling (EXT3/EXT4)**

Logs file system changes (metadata, data) to ensure integrity and track events.

**Forensic Value**:

Recovers metadata for deleted files. Tracks creation, modification, deletion. Detects tampering via inconsistencies.

**Access**: Use debugfs or dumpe2fs to inspect journal logs.

### **Tampering Detection**

**Methods**:

- Timestomping: Altering timestamps (i_atime, i_mtime, etc.).
- Inode Manipulation: Changing permissions or ownership.
- Journal Tampering: Erasing operation logs.

**Detection**:

- Verify inode checksums (i_checksum_lo, i_checksum_hi).
- Compare timestamps with journal logs.
- Check bitmap for unexpected block allocations.

**Tools**: debugfs for inode/journal integrity, dumpe2fs for superblock consistency.

## Timestamps and Timestomping Analysis

Timestamps in EXT4 track file system events, crucial for reconstructing user actions and detecting malicious activity.

**Types**:

- **Access Time (atime)**: Last file read.
- **Modification Time (mtime)**: Last content modification.
- **Change Time (ctime)**: Last metadata change (e.g., permissions).
- **Deletion Time (dtime)**: Time of file deletion (for deleted files).
- **Birth Time (crtime)**: File creation time (EXT4 only).

**Forensic Value**:

Builds event timelines. Correlates with system logs for validation. Detects tampering (e.g., timestomping).

### **Timestomping**

Anti-forensic technique where attackers alter timestamps (atime, mtime, ctime, crtime) to hide activity or blend malicious files with legitimate ones. **Method**: Modify timestamps using tools or change system clock before file operations.

- **Detection**:

Inconsistent timestamps (e.g., atime/mtime in past, but ctime/crtime recent). Cross-reference with journal logs or system logs. Use ctime-based searches to bypass tampered mtime/atime.

### **Analyzing Timestamps**

**Directory**: /mnt/ext4_time

Command: `ls -l /mnt/ext4_time`

Output:

`rw-rw-r-- 1 analyst analyst 22 Jan 5 06:33 normal_file.txt
-rw-rw-r-- 1 analyst analyst 31 Jan 1 2016 timestomped_file.txt`

**Inspect normal_file.txt**:

Command: `stat normal_file.txt`

`Size: 22  Blocks: 8  IO Block: 4096  regular file
Inode: 1024624  Links: 1
Access: (0664/-rw-rw-r--)  Uid: (1001/analyst)  Gid: (1001/analyst)
Access: 2025-01-05 06:33:35.389419110 +0000
Modify: 2025-01-05 06:33:35.389419110 +0000
Change: 2025-01-05 06:33:35.389419110 +0000
Birth: 2025-01-05 06:33:35.389419110 +0000`

**Inspect timestomped_file.txt**:

Command: `stat timestomped_file.txt`

`Size: 31  Blocks: 8  IO Block: 4096  regular file
Inode: 1024627  Links: 1
Access: (0664/-rw-rw-r--)  Uid: (1001/analyst)  Gid: (1001/analyst)
Access: 2016-01-01 12:00:00.000000000 +0000
Modify: 2016-01-01 12:00:00.000000000 +0000
Change: 2025-01-05 06:33:55.001578638 +0000
Birth: 2025-01-05 06:33:42.401109261 +0000`

### **Detecting Timestomping:**

**Issue**: ls -l relies on mtime, which can be falsified (e.g., 2016 for timestomped_file.txt).

**Solution**: Use ctime-based search to reveal true file activity.

Command: `sudo find /mnt/ext4_time -newerct "2025-01-01" ! -newerct "2025-01-06" -ls`

Output:

`1024591  4 drwxrwxrwx 2 root   root   4096 Jan  5 06:33 /mnt/ext4_time
1024624  4 -rw-rw-r-- 1 analyst analyst 22 Jan  5 06:33 /mnt/ext4_time/normal_file.txt
1024627  4 -rw-rw-r-- 1 analyst analyst 31 Jan  1 2016 /mnt/ext4_time/timestomped_file.txt`

**Forensic Value**:

ctime and crtime are harder to fake (require metadata changes). Detects tampered files by comparing all timestamps.

you can use Autopsy

https://www.youtube.com/watch?v=KN8YgJnShPM

https://www.youtube.com/watch?v=ncwn8O70YVg