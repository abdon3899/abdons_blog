---
title: "File Carving"
description: "My personal SOC notes and research on File Carving."
publishDate: "2026-04-19T00:00:00Z"
tags: ["File System Analysis"]
---

# File Carving

## File Systems Overview:

 Organizes data into files and directories for efficient storage, retrieval, and updates.

**Windows File Systems**:

**FAT**: Oldest (1977), simple, used in USBs/memory cards (FAT12, FAT16, FAT32, exFAT). Components: Boot sector, File Allocation Table (FAT), storage space.

**NTFS**: Advanced, introduced with Windows NT.

**Master File Table (MFT)**: Records file/directory attributes (size, permissions, timestamps). **Attributes/Data Runs**: Files split into attributes; large files use non-resident data runs. **System Files**: E.g., $MFT, $DATA (forensic significance).

**Linux File Systems** (e.g., ext2, ext3, ext4):

**Inode Structure**: Stores metadata (size, ownership, permissions, timestamps) without filename. **Blocks/Inodes**: Files in fixed-size blocks; inodes track blocks with indirect references.

### File Headers and Footers:

 Distinct sequences (magic bytes) marking file start (headers) and sometimes end (footers). it  Identifys file format, boundaries, and structure for carving.

**Common Examples**:

| **File Type** | **Header (Magic Bytes)** | **Footer / End Marker** | **Notes** |
| --- | --- | --- | --- |
| JPEG | FF D8 FF E0 (or FF D8 FF) | FF D9 | Common image format |
| PNG | 89 50 4E 47 0D 0A 1A 0A | 49 45 4E 44 AE 42 60 82 | Chunk-based structure |
| PDF | 25 50 44 46 (%PDF) | 25 25 45 4F 46 (%%EOF) | Portable Document Format |
| DOCX | 50 4B 03 04 (ZIP signature) | 50 4B 05 06 (ZIP end) | ZIP archive of XML files |
| GIF | 47 49 46 38 39 61 (GIF89a) or 47 49 46 38 37 61 (GIF87a) | 00 3B | Ends with semicolon |
| ZIP | 50 4B 03 04 | 50 4B 05 06 | General archive format |

look here for more info https://filesig.search.org/

### Metadata:

Data about data (e.g., creation date, permissions).

**Types**:

- **Embedded**: Within file (e.g., EXIF in JPEGs: camera settings, location).
- **Extended**: OS-stored attributes (e.g., permissions, last accessed date).
- **External**: Separate records (e.g., logs, database entries).

### File Carving:

Extracts files from raw data using signatures, bypassing file system metadata. we can use it with , **Recovering Deleted Files**: Data remains until overwritten post-deletion. **Corrupted Drives**: Works when file system is damaged. **Hidden/Tampered Files**: Uncovers concealed data.

**Process**:

1. **Scan Signatures**: Search for headers.
2. **Determine Boundaries**: Use headers/footers.
3. **Reconstruct Files**: Extract data between boundaries.

### **Data Recovery vs. File Carving**:

**Data Recovery**:

Relies on metadata (directories, FAT/MFT). Works with intact metadata. Uses standard recovery software.

**File Carving**:

Bypasses metadata. Works with missing/corrupted metadata. Uses signature-based tools (e.g., Autopsy).

**Limitations of Data Recovery**:

Fails without metadata. Struggles with fragmentation.

**File Carving Role**:

Overcomes metadata loss. Handles fragmented files (advanced techniques required).

## File Carving Tools

Extract files from raw data using patterns (e.g., headers/footers), bypassing file system metadata; ideal for damaged/corrupted/missing file systems.

### **Hex Editors**:

 View/edit raw file data in hexadecimal (address, hex bytes, ASCII representation). **Used in** Manual analysis of USB disk images, low-level data inspection. **Strength**: Precise control over raw data. **Limitation**: Manual process, time-intensive.

### **Binwalk**:

Analyzes/extracts data from binary files (e.g., memory dumps, embedded systems). **Used in** Identifies signatures, embedded files, executable code. **Strength**: Effective for binary data analysis. **Limitation**: Ineffective with fragmented or encrypted data.

### **Scalpel**:

Lightweight, open-source; scans for file signatures using a configuration file. **Used in** Targeted recovery (e.g., images, documents). **Strength**: Fast, precise for specific file types. **Limitation**: Limited with fragmented files or complex data structures.

### **Foremost**:

Open-source, configuration file-based; similar to Scalpel. **Used in** Forensic investigations needing precise signature rules. **Strength**: Flexible, rule-based recovery. **Limitation**: May struggle with fragmented files.

### **PhotoRec**:

 Recovers 480+ file types in one pass. **Used in** Broad recovery of various file types. **Strength**: Comprehensive, robust recovery. **Limitation**: Generates noise (recovers irrelevant files).

### **EnCase**:

Professional forensic suite with file carving, data acquisition, analysis, reporting. **Used in** Comprehensive forensic investigations. **Strength**: All-in-one tool, industry-standard. **Limitation**: Complex, typically paid.

## Manual File Carving:

Extracts files from raw data at the byte level without file system metadata, using techniques like header-footer carving and file structure carving.

### **Hex Editor (e.g., Okteta)**:

View and edit raw data in hexadecimal format (address, hex bytes, ASCII).

**Usage**:

1. Open disk image file (may take 1-2 minutes for large files).
2. Check initial sector (e.g., MBR) to assess formatting or corruption.
3. Search for file signatures (e.g., headers like 89504E47 for PNG, footers like 49454E44AE426082).
4. Note start and end offsets of identified files.
5. Select data range (convert offsets to decimal if needed), copy, and save as a new file with appropriate extension (e.g., .png).

### **Command Line Tool (e.g., dd)**:

Extract specific data ranges from a disk image.

**Usage**:

1. Identify start and end offsets of a file from signature search.
2. Calculate file size: End Offset - Start Offset.
3. Run command: `dd if=input_image.img of=output_file.ext bs=1 skip=Start_Offset count=File_Size.`
4. Verify extracted file integrity.

### **Metadata Tool (e.g., ExifTool)**:

Analyze file metadata to verify content and detect anomalies.

**Usage**:

1. Run: `exiftool output_file.ext` on extracted file.
2. Review output (e.g., file type, size, timestamps, resolution) for forensic context.

### **File Analysis Tool (e.g., Binwalk)**:

 Identify and extract file signatures, including fragmented data (e.g., slack space).

**Usage**:

1. Analyze image: binwalk input_image.img (may take 2-3 minutes).
2. Note offsets and file types (e.g., MPEG transport stream at offset 268782594).
3. Verify signatures in hex editor by navigating to offsets.
4. Extract files: `binwalk -e input_image.img` to create an extracted folder.
5. Explore folder (e.g., ls extracted_folder/) for recovered files.

### Challenges of Manual File Carving:

**Fragmented Files**:

- Locate scattered fragments, reconstruct using file format knowledge, handle missing data (partial recovery).

**Time-Intensive**:

- High data volumes (terabytes), complex encodings (encryption), proprietary formats (lack of documentation).

**Technical Barriers**:

- Corruption: Damaged headers/data blocks.
- False Positives: Valid fragments vs. unrelated data.
- Tool Limitations: Standard tools may fail for niche formats.

## Automated File Carving:

Uses specialized tools and configuration files to scan storage media or forensic images for "magic bytes" (headers/footers), automating file identification, extraction, and reconstruction.

**Advantages**:

1. **Efficiency and Scalability**:
    - Speed: Scans multiple file types in parallel (e.g., Scalpel, Foremost, PhotoRec, Autopsy), reducing manual effort.
    - Large-Scale: Handles terabytes of data across devices efficiently.
2. **Enhanced Accuracy**:
    - Algorithmic Detection: Uses signature databases to minimize missed fragments.
    - Minimized Error: Pre-defined signatures ensure consistency.
3. **Support for Different File Types**:
    - Pre-configured for common formats (JPEG, PNG, PDF, DOCX, ZIP).
    - Covers multimedia, documents, and some proprietary formats.
4. **Ease of Use**:
    - User-Friendly: Graphical or command-line interfaces for beginners.
    - Advanced Configuration: Custom config files for specific headers, footers, and sizes.

### **Foremost**:

Scans raw disk images from start to end, extracting files based on signatures.

Detects headers, records bytes until footer, max size, or next header. Saves files into subdirectories (e.g., jpg, pdf) in an output folder.

File: /etc/foremost.conf or custom (e.g., /etc/custom_foremost.conf). Defines: File types, headers/footers (e.g., \x47\x49\x46 for GIF, \xff\xd8\xff for JPG), case sensitivity, max size.

**Usage**:

1. Mount image to confirm emptiness (e.g., `sudo mount image.img /mnt/tmp → ls /mnt/tmp`).
2. Run: `foremost -i input_image.img -o output_dir -c config_file.`
3. Optional: `-t type1,type2` for specific formats (e.g., pdf,jpg,png).
4. Check output folder for carved files; validate with viewers or exiftool.

https://www.youtube.com/watch?v=Cga_BlOsOFg

### **Scalpel**:

Open-source carving tool, optimized for performance, derived from Foremost.

Two-pass process: Identifies boundaries, extracts files, handles fragmentation. File: `/etc/scalpel/scalpel.conf`, similar syntax to Foremost (headers, footers, rules).

- **Usage**:
    1. Run: `scalpel input_image.img -o output_dir -c config_file.`
    2. Check output_dir for extracted files.

https://www.youtube.com/watch?v=qiRmqne8HUQ