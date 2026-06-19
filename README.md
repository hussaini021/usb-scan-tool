<div align="center">

<img src="logo.png" width="140" alt="ShieldScan Pro Logo"/>

# ShieldScan Pro — USB Guardian

**Advanced USB Threat Detection for Windows**

[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-0078d4?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/hussaini021/usb-scan-tool/releases)
[![Python](https://img.shields.io/badge/Python-3.11%2B-3572A5?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![PySide6](https://img.shields.io/badge/UI-PySide6-41cd52?style=for-the-badge&logo=qt&logoColor=white)](https://doc.qt.io/qtforpython/)
[![Version](https://img.shields.io/badge/Version-5.0.0-4cde9a?style=for-the-badge)](https://github.com/hussaini021/usb-scan-tool/releases/tag/v2.0.0)
[![License](https://img.shields.io/badge/License-Expires%20Oct%202026-f5d142?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-4cde9a?style=for-the-badge)](https://github.com/hussaini021/usb-scan-tool)

**Plug in. Block. Scan. Stay safe.**

*The moment a USB device connects — ShieldScan Pro blocks it, scans every file across 9 detection engines, and only grants access if the device is clean.*

[⬇ Download v5.0.0](https://github.com/hussaini021/usb-scan-tool/releases/tag/v5.0.0) · [⬇ Download v1.0.0](https://github.com/hussaini021/usb-scan-tool/releases/tag/v1.0.0) · [Report Bug](https://github.com/hussaini021/usb-scan-tool/issues)

</div>

---

## 📋 Table of Contents

- [What's New in v5](#-whats-new-in-v2)
- [Overview](#-overview)
- [Features](#-features)
- [Detection Engines](#-detection-engines)
- [Threat Levels](#-threat-levels)
- [Installation](#-installation)
- [How It Works](#-how-it-works)
- [Requirements](#-requirements)
- [Building from Source](#-building-from-source)
- [Release History](#-release-history)
- [Author](#-author)

---

## 🚀 What's New in v5

Version 5.0.0 is a complete rewrite from the ground up. Every weakness in v1.0 was identified, documented, and fixed.

| Area | v1.0 (Old) | v5.0.0 (Current) |
|------|-----------|-----------------|
| **UI Framework** | Tkinter (legacy) | PySide6 — Windows 11 Fluent dark theme |
| **Navigation** | Single window | Sidebar: Dashboard · Scan · USB · Reports · Settings |
| **Scan Engine** | Single-threaded | `ThreadPoolExecutor` — up to 8 parallel workers |
| **USB Monitor** | Blocking WMI `NextEvent()` — hangs in EXE | `ctypes.GetLogicalDrives()` polling — reliable everywhere |
| **Startup** | 7-second fake splash | 2-second real splash |
| **GUI Thread** | Blocking operations on UI | All workers on `QThread` — zero UI freezing |
| **Crash Logging** | None — silent failures | `sys.excepthook` → rotating log at `%APPDATA%\ShieldScanPro\logs\` |
| **Progress Updates** | Every single file → queue flood | Throttled to max once per 150 ms |
| **Report Generation** | On GUI thread → freeze | `ReportWorker(QThread)` → background |
| **SHA-256** | Every file, always | Only for flagged files → 2× faster on clean drives |
| **Signatures** | 34 patterns | 47 patterns + 13 new threat types |
| **Installer** | Manual | NSIS wizard with auto-start, uninstaller, Add/Remove Programs |

> **14 documented bugs fixed.** Full audit notes are in the source file header.

---

## 🧭 Overview

**ShieldScan Pro** is a Windows security application that acts as an active gate for every USB storage device. Unlike passive antivirus software, it intercepts devices **before** access is granted and runs a full multi-engine scan — signatures, entropy, EXIF metadata, PE injection detection, LNK analysis, Office macros, PDF payloads, and more.

The application runs silently at Windows startup, uses minimal CPU/RAM when idle, and only springs into action the moment a removable device is detected.

> *Built entirely by one student developer. No team. No funding. Every line written from scratch.*

---

## ✨ Features

### Core Protection
- 🔌 **Real-time USB Interception** — device blocked the instant it connects
- 🛡 **Windows Defender Integration** — triggers a live custom Defender scan in parallel
- 🔬 **9-Engine Deep Scan** — runs all detection modules on every file
- ⚡ **Parallel Scanning** — up to 8 simultaneous file scan threads
- 🚀 **Auto-Start at Boot** — always running, invisible until needed

### Modern Interface
- 🌙 **Windows 11 Dark Theme** — PySide6 with custom QSS styling
- 🗂 **Sidebar Navigation** — Dashboard, Scan, USB, Reports, Settings
- 📊 **Live Stat Cards** — files scanned, threats found, critical count, speed
- 🔔 **Toast Notifications** — slide-in alerts for USB events and scan results
- 📈 **Real-time Progress** — ETA, files/sec, per-category threat counters

### Reporting & Diagnostics
- 📑 **Professional HTML Reports** — auto-generated after each scan, opens in browser
- 📋 **Security Score** — 0–100 score based on threat severity and count
- 🗃 **Reports History** — browse, open, manage all past scan reports
- 📝 **Rotating Log File** — crash-safe diagnostics at `%APPDATA%\ShieldScanPro\logs\`
- 💥 **Crash Handler** — unhandled exceptions written to disk, dialog shown to user

---

## 🔬 Detection Engines

ShieldScan Pro runs **9 independent engines** on every file:

| Engine | What It Finds |
|--------|--------------|
| **Signature Scanner** | 47 malicious patterns — PowerShell, C2, ransomware, keyloggers, injectors |
| **PE Injection Detector** | Windows executables hidden inside image/video/document files |
| **Shannon Entropy Analyzer** | Encrypted or packed payloads disguised as normal files |
| **EXIF Metadata Inspector** | JavaScript, PowerShell, or C2 URLs injected into image EXIF fields |
| **VBA Macro Detector** | AutoOpen/AutoClose macros in Word, Excel, PowerPoint |
| **LNK Shortcut Analyzer** | `.lnk` files that silently execute dropper commands on click |
| **Polyglot File Detector** | Files valid in two formats simultaneously (ZIP inside JPG, etc.) |
| **PDF Payload Scanner** | `/JavaScript`, `/Launch`, and embedded EXE inside PDF containers |
| **Video Metadata Scanner** | Scripts injected into MP4/MOV container atoms |

**+ Name Spoofing Checks:**
- Double extension (`photo.jpg.exe`)
- Unicode Right-to-Left Override (hides true extension)
- Known autorun trigger filenames (`autorun.inf`, `desktop.ini`)

### Signature Coverage (47 Patterns)

```
PowerShell abuse      invoke-expression, encodedcommand, bypass, iex(, downloadstring
Remote execution      mshta, certutil, rundll32, regsvr32, installutil, regasm, wmic
Keyloggers            GetAsyncKeyState, SetWindowsHookEx
Credential theft      LoginData, sqlite, credential, mimikatz, lsass
Reverse shells        nc -e, reverse shell
C2 infrastructure     discord.com/api, pastebin.com, .onion
Ransomware            encryptfile, vssadmin delete, bcdedit /set, ransom note text
Persistence           autorun.inf, registry Run key, schtasks
Process injection     CreateRemoteThread, VirtualAlloc, WriteProcessMemory
Privilege escalation  whoami /priv, net user /add, netsh firewall
```

---

## 🚦 Threat Levels

| Level | Color | Meaning | Action |
|-------|-------|---------|--------|
| `CLEAN` | 🟢 Green | No threats found | Device access granted |
| `LOW` | 🔵 Blue | Minor concern | Warning logged |
| `MEDIUM` | 🟡 Yellow | Moderate risk | User prompted |
| `HIGH` | 🟠 Orange | Serious threat | Strong warning |
| `CRITICAL` | 🔴 Red | Active threat | Device blocked, immediate alert |

---

## 💾 Installation

### Option A — Setup Wizard (Recommended)

1. Go to [**Releases**](https://github.com/hussaini021/usb-scan-tool/releases)
2. Download `ShieldScanPro_Setup_v5.0.0.exe`
3. **Run as Administrator**
4. Accept the trilingual license (English / دری / پښتو)
5. Click Install — done ✅

The installer will:
- Install to `C:\Program Files\ShieldScan Pro\`
- Create Desktop + Start Menu shortcuts
- Register auto-start at Windows login
- Add entry to Add/Remove Programs

> ⚠️ **Administrator privileges are required** for USB monitoring and Windows Defender integration.

### Option B — Run from Source

```bash
pip install PySide6 Pillow
python shieldscan_pro_v5.py
```

---

## ⚙️ How It Works

```
USB Device Connected
        │
        ▼
┌───────────────────┐
│  ctypes polling   │  ← GetLogicalDrives() every 1.5 s
│  detects new drive│
└────────┬──────────┘
         │
         ▼
┌───────────────────┐
│  ACCESS BLOCKED   │  ← User alerted via dialog + toast
└────────┬──────────┘
         │  (User approves scan)
         ▼
┌────────────────────────────────────┐
│         SCAN PIPELINE              │
│                                    │
│  ┌─────────────────────────────┐   │
│  │  Windows Defender (thread)  │   │  ← runs in parallel
│  └─────────────────────────────┘   │
│                                    │
│  ┌─────────────────────────────┐   │
│  │  File Enumeration           │   │
│  └────────────┬────────────────┘   │
│               │                    │
│  ┌────────────▼────────────────┐   │
│  │  ThreadPoolExecutor         │   │
│  │  (up to 8 parallel workers) │   │
│  │  × 9 Detection Engines each │   │
│  └────────────────────────────┘   │
└───────────────┬────────────────────┘
                │
       ┌────────┴────────┐
       │                 │
  CLEAN ✅          THREAT ⚠️ / 🚨
       │                 │
  Access granted    Device blocked
                   Report generated
                   Toast + dialog shown
```

---

## 🖥️ Requirements

| Requirement | Details |
|------------|---------|
| **OS** | Windows 10 / 11 (64-bit) |
| **Python** | 3.11 or higher (source build only) |
| **Privileges** | Administrator — required |
| **PySide6** | `pip install PySide6` |
| **Pillow** | `pip install Pillow` — optional, enables EXIF scanning |
| **Windows Defender** | Must be enabled for Defender integration |

---

## 🔨 Building from Source

```bash
# 1. Clone the repository
git clone https://github.com/hussaini021/usb-scan-tool.git
cd usb-scan-tool

# 2. Install dependencies
pip install PySide6 Pillow pyinstaller

# 3. Run the build script (as Administrator)
build.bat

# 4. Build the installer (requires NSIS installed)
makensis shieldscan_installer.nsi
```

Output: `ShieldScanPro_Setup_v5.0.0.exe`

The build script automatically:
- Checks for Python and dependencies
- Generates the trilingual license RTF
- Compiles the executable with PyInstaller
- Copies resources into the dist folder

---

## 📦 Release History

| Version | Date | Highlights |
|---------|------|-----------|
| [**v2.0.0**](https://github.com/hussaini021/usb-scan-tool/releases/tag/v2.0.0) | 2026 | Full rewrite — PySide6, parallel scanning, 14 bugs fixed, NSIS installer |
| [**v1.0.0**](https://github.com/hussaini021/usb-scan-tool/releases/tag/v1.0.0) | 2026 | Initial release — Tkinter UI, WMI USB monitor, 9-engine scan |

---

## 👤 Author

<div align="center">

**Murtaza Hussaini**
*Student Developer · Security Tools*

[![GitHub](https://img.shields.io/badge/GitHub-hussaini021-181717?style=for-the-badge&logo=github)](https://github.com/hussaini021)

*Built with no team, no funding, and no shortcuts.*
*Every line written in stolen hours, fueled by determination.*

---

**ShieldScan Pro** — *Because every USB deserves to be questioned.*

⭐ If this project helped you, a star on GitHub means a lot.

</div>
