<div align="center">

<img src="https://img.shields.io/badge/Platform-Windows%2010%2F11-0078d4?style=for-the-badge&logo=windows&logoColor=white"/>
<img src="https://img.shields.io/badge/Python-3.11+-3572A5?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Version-1.0.0-4cde9a?style=for-the-badge"/>
<img src="https://img.shields.io/badge/License-Expires%20Oct%202026-f5d142?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Status-Active-4cde9a?style=for-the-badge"/>

<br/><br/>

```
███╗   ███╗███╗   ██╗    ███████╗ ██████╗ █████╗ ███╗   ██╗    ████████╗ ██████╗  ██████╗ ██╗
████╗ ████║████╗  ██║    ██╔════╝██╔════╝██╔══██╗████╗  ██║    ╚══██╔══╝██╔═══██╗██╔═══██╗██║
██╔████╔██║██╔██╗ ██║    ███████╗██║     ███████║██╔██╗ ██║       ██║   ██║   ██║██║   ██║██║
██║╚██╔╝██║██║╚██╗██║    ╚════██║██║     ██╔══██║██║╚██╗██║       ██║   ██║   ██║██║   ██║██║
██║ ╚═╝ ██║██║ ╚████║    ███████║╚██████╗██║  ██║██║ ╚████║       ██║   ╚██████╔╝╚██████╔╝███████╗
╚═╝     ╚═╝╚═╝  ╚═══╝    ╚══════╝ ╚═════╝╚═╝  ╚═╝╚═╝  ╚═══╝       ╚═╝    ╚═════╝  ╚═════╝ ╚══════╝
```

# 🔍 MN Scan Tool
### USB Device Security Scanner for Windows

**Plug in. Scan. Stay safe.**  
A lightweight yet powerful security tool that intercepts USB devices the moment they connect and scans every file before granting access — protecting your system from malware, ransomware, keyloggers, and hidden payloads.

<br/>

> *Built from scratch by one developer, with no team, no funding, and no shortcuts.*  
> *Every line of code was written in stolen hours, fueled by determination.*

<br/>

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Detection Engines](#-detection-engines)
- [Threat Levels](#-threat-levels)
- [Installation](#-installation)
- [How It Works](#-how-it-works)
- [Screenshots](#-screenshots)
- [Requirements](#-requirements)
- [Building from Source](#-building-from-source)
- [Author](#-author)

---

## 🧭 Overview

**MN Scan Tool** is a Windows desktop application that acts as a security gate for every USB device connected to your computer. The moment a flash drive, external hard disk, or any storage device is plugged in — **MN Scan Tool blocks access**, performs a deep multi-engine scan, and only releases the device if it passes all security checks.

Unlike traditional antivirus software that monitors the whole system passively, MN Scan Tool takes an **active, intercept-first** approach: **the device is blocked until proven clean.**

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔌 **Auto USB Intercept** | Detects and blocks any new USB device the instant it connects via WMI event monitoring |
| 🛡️ **Windows Defender Integration** | Triggers a live Defender custom scan on the USB drive and reports the result |
| 🔬 **Deep Multi-Engine Scan** | Runs 9 independent detection engines simultaneously on every file |
| 📊 **Shannon Entropy Analysis** | Detects encrypted/packed payloads by measuring file randomness |
| 🖼️ **EXIF Metadata Inspection** | Scans image EXIF fields for injected scripts and malicious keywords |
| 🧬 **PE Injection Detection** | Finds Windows executables hidden inside image, video, and document files |
| 📎 **LNK Shortcut Analysis** | Detects malicious `.lnk` files that silently execute dropper commands |
| 📄 **Office Macro Detection** | Identifies VBA macros in Word, Excel, and PowerPoint documents |
| 🎭 **Polyglot File Detection** | Exposes files that are simultaneously valid in two formats to bypass filters |
| 📹 **Video Metadata Scanning** | Scans MP4/MOV container atoms for injected script payloads |
| 🔡 **Extension Spoofing Detection** | Catches double-extension and Unicode RLO name tricks |
| #️⃣ **SHA-256 Hashing** | Computes cryptographic hash for every scanned file |
| ⚡ **Fast Mode / Deep Mode** | Choose between quick scan (signatures only) or full deep scan with entropy + EXIF |
| 🚀 **Auto-Start at Boot** | Registers itself in Windows startup — always running silently in the background |
| 📑 **HTML Security Report** | Generates a detailed, styled HTML report with all findings, file paths, and hashes |
| 🌙 **Silent Background Mode** | Runs invisibly in system tray with `/background` flag, pops up only when a threat is found |

---

## 🔬 Detection Engines

MN Scan Tool uses **9 independent detection modules** that work in parallel:

```
┌─────────────────────────────────────────────────────────────┐
│                    SCAN ENGINE PIPELINE                     │
├──────────────────┬──────────────────────────────────────────┤
│  1. Signatures   │  50+ known malicious byte patterns       │
│                  │  (PowerShell, IEX, reverse shells, C2s)  │
├──────────────────┼──────────────────────────────────────────┤
│  2. PE Injection │  Finds EXE headers hidden in media files  │
├──────────────────┼──────────────────────────────────────────┤
│  3. Entropy      │  Shannon entropy > 7.5 → packed payload  │
├──────────────────┼──────────────────────────────────────────┤
│  4. EXIF         │  Scripts/keywords in image metadata       │
├──────────────────┼──────────────────────────────────────────┤
│  5. LNK          │  Malicious Windows shortcut analysis      │
├──────────────────┼──────────────────────────────────────────┤
│  6. OLE Macros   │  VBA AutoOpen/Shell in Office documents   │
├──────────────────┼──────────────────────────────────────────┤
│  7. Polyglot     │  ZIP-in-image, JS-in-PDF detection        │
├──────────────────┼──────────────────────────────────────────┤
│  8. Video Meta   │  Injected payloads in MP4/MOV atoms       │
├──────────────────┼──────────────────────────────────────────┤
│  9. Name Spoof   │  Double extension, Unicode RLO, autorun   │
└──────────────────┴──────────────────────────────────────────┘
```

### Signature Coverage (50+ Patterns)
The signature engine detects threats including:
- PowerShell abuse (`invoke-expression`, `encodedcommand`, `bypass`)
- Remote code execution (`mshta`, `certutil`, `rundll32`, `regsvr32`)
- Keyloggers (`GetAsyncKeyState`, `SetWindowsHookEx`)
- Browser credential theft (`LoginData`, SQLite access)
- Reverse shells (`nc -e`, reverse shell markers)
- C2 infrastructure (`discord.com/api`, `pastebin.com`, `.onion`)
- Ransomware payloads (`encryptfile`, ransom note text)
- Persistence mechanisms (`autorun.inf`, registry `Run` key writes)
- Process injection (`CreateRemoteThread`, `VirtualAlloc`, `WriteProcessMemory`)

---

## 🚦 Threat Levels

```
  ● CLEAN      No threats detected — device is safe
  ● LOW        Minor concern — suspicious but low risk
  ● MEDIUM     Moderate risk — review recommended
  ● HIGH       Serious threat — do not use this file
  ● CRITICAL   Active threat — device is blocked
```

Threat levels are color-coded throughout the UI and HTML report:

| Level | Color | Action |
|---|---|---|
| CLEAN | 🟢 Green | Device access granted |
| LOW | 🔵 Blue | Warning issued |
| MEDIUM | 🟡 Yellow | User prompted to confirm |
| HIGH | 🟠 Orange | Strong warning, blocked by default |
| CRITICAL | 🔴 Red | Device permanently blocked |

---

## 💾 Installation

### Option A — Install via Setup Wizard (Recommended)

1. Download `MNScanTool_Setup.exe` from the [Releases](https://github.com/hussaini021/usb-scan-tool/releases) page
2. Run the installer as Administrator
3. Follow the setup wizard — accept the license, choose install folder
4. MN Scan Tool will install, create shortcuts, and register for Windows auto-start
5. Done — it's running silently in the background, ready to intercept any USB

### ## 💾 Installation

### ⬇️ Quick Download

**[Download MNScanTool_Setup.exe v1.0.0](https://github.com/hussaini021/usb-scan-tool/releases/download/v1.0.0/MNScanTool_Setup.exe)** 
*(Windows 10/11, 64-bit)*

Or view all releases: [github.com/hussaini021/usb-scan-tool/releases](https://github.com/hussaini021/usb-scan-tool/releases)

---

### Option A — Install via Setup Wizard (Recommended)

1. Download the `.exe` file using the link above
2. Run as Administrator
3. Follow the setup wizard
4. Done ✅

> ⚠️ **Administrator privileges are required** for USB monitoring and Windows Defender integration.

---

## ⚙️ How It Works

```
  USB Device Connected
         │
         ▼
  ┌─────────────────┐
  │  WMI Event Fire │  ← Win32_LogicalDisk creation event
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │  ACCESS BLOCKED │  ← User is alerted, device locked
  └────────┬────────┘
           │
           ▼
  ┌─────────────────┐
  │ User Permission │  ← Scan now / Deny access
  └────────┬────────┘
           │  (Scan approved)
           ▼
  ┌──────────────────────────────┐
  │     SCAN PIPELINE            │
  │  ┌────────────────────────┐  │
  │  │ Windows Defender Scan  │  │
  │  └────────────────────────┘  │
  │  ┌────────────────────────┐  │
  │  │ 9-Engine File Analysis │  │
  │  └────────────────────────┘  │
  └──────────────┬───────────────┘
                 │
         ┌───────┴───────┐
         │               │
    CLEAN ✅         THREAT ⚠️
         │               │
    Access           Device still
    granted          blocked +
                  Report generated
```

---

## 📷 Screenshots

> *Screenshots will be added after the first public release.*

The UI uses a dark SSMS-inspired blue theme:
- **Splash screen** — animated loading on startup
- **Main window** — real-time scan log, progress bar, USB status
- **Results window** — threat summary cards + detailed findings log
- **HTML report** — exportable, styled security report

---

## 🖥️ Requirements

| Requirement | Details |
|---|---|
| OS | Windows 10 / Windows 11 (64-bit) |
| Python | 3.11 or higher (for source builds) |
| Privileges | Administrator (required for USB monitoring + Defender) |
| Dependencies | `tkinter` (built-in), `pillow` (optional), `pywin32` (optional) |
| Windows Defender | Must be enabled for Defender integration |

**Optional dependencies enhance detection:**
- `pillow` — enables EXIF metadata scanning of images
- `pywin32` — enables real-time USB auto-detection via WMI

Without these, MN Scan Tool still works — it falls back to manual folder scan mode.

---

## 🔨 Building from Source

To build a standalone `.exe` using PyInstaller:

```bash
pip install pyinstaller pillow pywin32

pyinstaller --onefile --windowed --icon=logo.ico \
  --name MN_Scan_Tool mn_scan_tool.py
```

The output will be in `dist/MN_Scan_Tool.exe`.

Then use the provided `installer.nsi` with [NSIS](https://nsis.sourceforge.io/) to build the setup wizard:

```bash
makensis installer.nsi
# Output: MNScanTool_Setup.exe
```

---

## 👤 Author

**Murtaza Hussaini**  
Self-taught developer | Security tools enthusiast

- 🐙 GitHub: [github.com/hussaini021](https://github.com/hussaini021)

---

## 📜 License

This software is licensed until **October 31, 2026**.  
After the expiry date, the program will display a renewal notice and stop functioning.  
For license renewal or commercial inquiries, contact via GitHub.

---

## 🤝 Contributing

This project was built independently with no funding or team support.  
If you find it useful, a ⭐ star on GitHub goes a long way.  

Bug reports, feature suggestions, and pull requests are welcome.

---

<div align="center">

**MN Scan Tool** — *Because every USB deserves to be questioned.*

Made with persistence, not resources.

</div>
