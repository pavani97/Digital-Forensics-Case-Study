# 🔍 Digital Forensics Case Study  
## Disk & Memory Forensics with Timeline Analysis

---

## 🧩 Case Scenario

A company employee is suspected of exfiltrating sensitive files using a USB drive and attempting to cover tracks before leaving the organization.

Your task is to analyze disk and memory artifacts, extract forensic evidence, and build an investigation timeline.

---

## 🎯 Objectives

- Analyze disk artifacts from a forensic image  
- Analyze memory dump for suspicious processes and credentials  
- Extract forensic evidence  
- Correlate findings and build a timeline  
- Document everything like a real DFIR report  

---

## 🛠️ Tools Used (All Free)

| Category | Tools |
|--------|-------|
| Disk Analysis | Autopsy |
| Memory Analysis | Volatility 3 |
| Timeline | Plaso (log2timeline) |
| Hashing | FTK Imager |
| OS | Windows / Kali Linux |
| Evidence | Sample forensic images |

---

## 📂 Evidence Used

### Disk Image

**NIST CFReDS Disk Images**  
https://cfreds.nist.gov/

Example images:
- `HackingCase.dd`
- `USB_Analysis.dd`

### Memory Dump

**Volatility Sample Memory Dumps**  
https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples

---

## 🧪 Hands-On Investigation Steps

### 🔹 Step 1: Verify Evidence Integrity

```bash
sha256sum evidence.dd
```

Document hash values to prove evidence integrity.

---

### 🔹 Step 2: Disk Forensics (Autopsy)

- Open Autopsy  
- Create a new case  
- Add disk image  
- Analyze:
  - Deleted files  
  - USB artifacts  
  - Recent documents  
  - Browser history  
  - File timestamps  

**Key Artifacts to Extract:**
- `$MFT`
- `$LogFile`
- USB history
- Recently accessed files

Screenshots are stored in `/screenshots/`.

---

### 🔹 Step 3: Memory Forensics (Volatility 3)

Identify OS profile:
```bash
python vol.py -f memory.dmp windows.info
```

List processes:
```bash
python vol.py -f memory.dmp windows.pslist
```

Find suspicious commands:
```bash
python vol.py -f memory.dmp windows.cmdline
```

Extract credentials:
```bash
python vol.py -f memory.dmp windows.lsadump
```

**Indicators to look for:**
- Unknown processes  
- Command history  
- Network connections  
- Credential dumping activity  

---

### 🔹 Step 4: Timeline Creation (Plaso)

Create timeline:
```bash
log2timeline.py timeline.plaso evidence.dd
```

Convert timeline:
```bash
psort.py -o L2tcsv timeline.plaso > timeline.csv
```

Correlate:
- File access times  
- USB insertion times  
- Command execution  
- File deletion  

---

### 🔹 Step 5: Evidence Correlation

| Artifact | Finding |
|--------|--------|
| USB History | USB connected at 10:43 PM |
| File Access | Sensitive file opened |
| Memory Dump | Copy command executed |
| Deletion | File deleted after transfer |

**Conclusion:** Data exfiltration via USB device.

---

## 🧾 Final Findings Summary

- USB device connected during off-hours  
- Sensitive files accessed and copied  
- Evidence of file deletion attempts  
- Suspicious command execution found in memory  
- Timeline confirms insider activity  

---

## 📌 Final Conclusion

This investigation confirms insider data exfiltration using removable media. Disk artifacts, memory analysis, and timeline correlation collectively validate malicious activity and attempted evidence cleanup.
