# README.md – Exact Content to Copy and Paste

Copy everything below (from the first line to the last line) and paste it into your GitHub `README.md` file.

---

```
# SBT-DF202 Lab 1 – Digital Forensics Case Handling, Autopsy and Sleuth Kit Analysis

## Repository: SBT-DF202_Lab1_Ch01InChap01

**Student:** Ibrahim Ishaku  
**Student ID:** 2025/FWSD/11334  
**Course:** SBT-DF202 – Practical Laboratories  
**Date:** 31st August, 2026

---

## Project Overview

This repository contains the complete forensic investigation work conducted on the evidence image `Ch01InChap01.dd` as part of the SBT-DF202 Practical Laboratories course. The investigation demonstrates the application of digital forensic principles using Autopsy and The Sleuth Kit (TSK) command-line tools.

### Investigation Objectives

1. **Preserve Evidence Integrity** – Verify the forensic image using cryptographic hashes (MD5 and SHA-256).
2. **Identify Deleted Files** – Use `fls` to list all files, including deleted ones.
3. **Recover Relevant Evidence** – Recover `Income.xls` and other deleted files using `icat`, `blkcat`, and `tsk_recover`.
4. **Perform Keyword Searches** – Search for keywords such as `INCOME`, `financial`, and `report`.
5. **Document Findings** – Produce a repeatable technical forensic report.

---

## Repository Structure

```
SBT-DF202_Lab1_Ch01InChap01/
├── evidence/
│   └── Ch01InChap01.dd              # Forensic disk image (1.5M)
├── recovered/
│   ├── Income.xls                   # Recovered Excel file (14K)
│   ├── DELETED_Billing_Letter.doc   # Recovered deleted file
│   ├── DELETED_confirmation.txt     # Recovered deleted file
│   └── DELETED_Regrets.doc          # Recovered deleted file
├── logs/
│   ├── evidence_info.txt            # Evidence metadata
│   ├── source_image_md5.txt         # MD5 hash of source image
│   └── source_image_sha256.txt      # SHA-256 hash of source image
├── reports/
│   └── manual_report.txt            # Forensic analysis report
├── screenshots/                     # Screenshots of all steps
└── README.md                        # This file
```

---

## Evidence Integrity

The source evidence image `Ch01InChap01.dd` was verified using the following cryptographic hashes:

| Hash Type | Value |
|-----------|-------|
| **MD5** | `a117773bcf1fc88ec0ab8e0a349fbccb` |
| **SHA-256** | `3ce8053e4f3d9c8ab98b3aadb2480685efb8e4980d34297b83bd5a09b1a7b122` |

---

## Key Findings

### Deleted Files Identified

| File Name | Metadata Address | Status |
|-----------|------------------|--------|
| Billing Letter.doc | 8 | Deleted |
| confirmation.txt | 11 | Deleted |
| letter1.txt | 15 | Deleted |
| Regrets.doc | 17 | Deleted |

### Recovered Files

| File Name | Size | MD5 Hash | SHA-256 Hash |
|-----------|------|----------|--------------|
| Income.xls | 14K | `6a2e65afc5af4fc5f9da2859df134eac` | `8d7cd7204d3dae161a8fa879e184865b3bc4a57a4e688abd522a9ff03f62252d` |
| DELETED_Billing_Letter.doc | 24K | – | – |
| DELETED_confirmation.txt | 227 bytes | – | – |
| DELETED_Regrets.doc | 23K | – | – |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Autopsy** | Forensic case creation and analysis |
| **img_stat** | Image file type and size information |
| **mmls** | Partition layout analysis |
| **fsstat** | File system information |
| **fls** | File and directory listing (including deleted files) |
| **istat** | File metadata examination |
| **icat** | File recovery |
| **blkcat** | Block-level data extraction |
| **tsk_recover** | Bulk file recovery |
| **md5sum / sha256sum** | Cryptographic hash verification |
| **file** | File type identification |

---

## Commands Used

### Evidence Preparation

```bash
# Create working directories
mkdir -p ~/SBT-DF202_Lab1_{evidence,recovered,reports,screenshots,logs}

# Calculate hashes
md5sum Ch01InChap01.dd | tee ../logs/source_image_md5.txt
sha256sum Ch01InChap01.dd | tee ../logs/source_image_sha256.txt
```

### Deleted File Identification

```bash
# List all files including deleted
fls Ch01InChap01.dd

# List only deleted files recursively
fls -r -d Ch01InChap01.dd

# Examine metadata for Income.xls
istat Ch01InChap01.dd 13
```

### File Recovery

```bash
# Recover Income.xls
icat Ch01InChap01.dd 13 > ~/SBT-DF202_Lab1_recovered/Income.xls

# Recover deleted files
icat Ch01InChap01.dd 8 > ~/SBT-DF202_Lab1_recovered/DELETED_Billing_Letter.doc
icat Ch01InChap01.dd 11 > ~/SBT-DF202_Lab1_recovered/DELETED_confirmation.txt
icat Ch01InChap01.dd 17 > ~/SBT-DF202_Lab1_recovered/DELETED_Regrets.doc

# Block-level recovery
blkcat Ch01InChap01.dd 19 > ~/SBT-DF202_Lab1_recovered/Income_xls_blkcat.xls

# Bulk recovery
tsk_recover Ch01InChap01.dd ~/SBT-DF202_Lab1_recovered/
```

### Hash Verification

```bash
md5sum ~/SBT-DF202_Lab1_recovered/Income.xls
sha256sum ~/SBT-DF202_Lab1_recovered/Income.xls
file ~/SBT-DF202_Lab1_recovered/Income.xls
```

---

## How to Use This Repository

### 1. Clone or Download

```bash
git clone [repository-url]
cd SBT-DF202_Lab1_Ch01InChap01
```

### 2. Verify the Evidence Image

```bash
cd evidence
md5sum Ch01InChap01.dd
sha256sum Ch01InChap01.dd
```

### 3. Review the Report

```bash
cat reports/manual_report.txt
```

### 4. Examine Recovered Files

```bash
ls -la recovered/
file recovered/Income.xls
```

---

## File System Information

| Attribute | Value |
|-----------|-------|
| File System Type | FAT12 |
| Volume Name | NO NAME |
| File System Size | 1.5M |
| Image Type | Raw (dd) |
| Partition Table | None (volume image) |

---

## Challenges Encountered

1. **Download Issue:** The evidence image initially downloaded as an HTML page (179 KB) instead of the actual forensic image. This was resolved by using the correct direct download link.

2. **Shared Folder Configuration:** VMware shared folder access required troubleshooting to mount `sf_Downloads` at `/media/sf_Downloads/`.

3. **Autopsy Interface:** The Autopsy "Deleted Files" view was not accessible. All analysis was completed using The Sleuth Kit command-line tools.

4. **mmls Output:** The `mmls` command produced no output because the image is a volume image (no partition table).

---

## Recommendations

1. **Use Sleuth Kit for Consistent Results:** When Autopsy interface is unavailable, the command-line tools provide reliable, repeatable results.

2. **Always Verify Hashes:** Cryptographic hashes are essential for evidence integrity.

3. **Document Every Step:** Repeatable processes are critical for forensic admissibility.

4. **Use Read-Only Mounting:** Never modify the original evidence image.

---

## References

1. ICDFA. (2026). *SBT-DF202 – Module 1: Digital Investigation Procedures – Course Materials*.

2. ICDFA. (2026). *SBT-DF202 – Module 2: Autopsy and The Sleuth Kit – Course Materials*.

3. Autopsy Documentation. (2026). *Autopsy User Guide*. https://www.autopsy.com/

4. Sleuth Kit Documentation. (2026). *The Sleuth Kit User Guide*. https://www.sleuthkit.org/sleuthkit/

5. NIST. (2014). *Guide to Integrating Forensic Techniques into Incident Response*. NIST SP 800-86.

---

## Author

**Ibrahim Ishaku**  
Fellowship in Web Application Security & Digital Forensics  
International Cybersecurity and Digital Forensics Academy (ICDFA)  
Student ID: 2025/FWSD/11334

---

## License

This repository is for educational purposes only. All evidence images and recovered files are training materials provided by ICDFA.

---

## Contact

For questions regarding this investigation, please contact the ICDFA Directorate of Training.

---

*Follow Evidence. Find Truth.*
```
