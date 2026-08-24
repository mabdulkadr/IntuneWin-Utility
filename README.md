<div align="center">

# 📦 IntuneWin Utility

**Intune Win32 packaging, point and click**

Convert EXE / MSI / PS1 / BAT installers into `.intunewin` packages using Microsoft's official IntuneWinAppUtil — with a clean WPF interface and zero command lines.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![PowerShell](https://img.shields.io/badge/powershell-5.1%2B-blue.svg)
![Platform](https://img.shields.io/badge/Windows-10%2F11-blue.svg)
![UI](https://img.shields.io/badge/UI-WPF%20GUI-blue.svg)
![Intune](https://img.shields.io/badge/Intune-Win32%20Apps-0078D4.svg)
![Version](https://img.shields.io/badge/version-1.1-green.svg)

[Features](#-core-features) • [Usage](#-usage) • [Requirements](#️-requirements) • [Troubleshooting](#-troubleshooting)

</div>

---

# 📖 Overview

**IntuneWin Utility** is a modern GUI tool that creates **Intune Win32 packages (`.intunewin`)** from **EXE / MSI / PS1 / BAT** installers using Microsoft's official **IntuneWinAppUtil.exe** — with a clean UI, smart installer detection, and real-time logging.

---

# ✨ Core Features

### 🔹 Core Packaging
* Convert EXE / MSI / PS1 / BAT → `.intunewin`
* Uses the official Microsoft **IntuneWinAppUtil**
* Async packaging — the UI never freezes
* Real-time Message Center logs
* Accurate output file name + size display

### 🔹 Smart User Experience
* Auto-detects the installer inside the Source folder
* Multiple installers? Selects the first automatically and shows a green SUCCESS message
* Displays source file name (no full path), output package name, and output size
* Prevents running multiple packaging jobs simultaneously

### 🔹 Validation Engine
* Source folder must exist
* Installer must be inside the source folder
* Only `.exe`, `.msi`, `.ps1`, or `.bat` supported
* Output folder auto-created if missing
* Detects a missing `IntuneWinAppUtil.exe`

### 🔹 Logging
Log file stored automatically under `C:\IntuneWinUtility\`.

---

# 🚀 Usage

### Run

Just double-click:

```text
IntuneWin App Utility.exe
```

No PowerShell window required.

### Steps

1. Select the **Source Folder**
2. The tool auto-detects the EXE / MSI / PS1 / BAT installer
3. Choose the output mode — **Same as source** or a **custom output folder**
4. Click **Create Package**

### Auto-Detection Behavior

When a Source Folder is selected:

* The tool searches for `.exe`, `.msi`, `.ps1`, or `.bat` files
* If found — the installer is auto-selected with a green SUCCESS message
* If multiple exist — the first is selected automatically and the detected count is logged

### Output Result

After packaging, the tool shows the output file name, its size, a success message, and can open the output folder:

```text
AppName.intunewin (45.3 MB)
```

---

# ⚙️ Requirements

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10 / 11 |
| **PowerShell** | 5.1+ (packaged EXE needs no console) |
| **IntuneWinAppUtil.exe** | Microsoft's official tool — must be available to the utility |

### Working Structure

Automatically created on first run:

```text
C:\IntuneWinUtility\
├── Source\                              # place your installer + content here
├── Output\                              # .intunewin packages land here
└── IntuneWinUtility-YYYYMMDD.log        # daily log
```

---

# 🔍 Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| "IntuneWinAppUtil.exe not found" | Microsoft's tool missing | Place it where the utility expects it |
| Installer not detected | Unsupported extension or wrong folder | Put an `.exe`/`.msi`/`.ps1`/`.bat` inside the Source folder |
| Source folder validation fails | Path missing or installer outside it | Keep the installer inside the selected Source folder |
| Create Package disabled | Another packaging job is running | Wait for the current job to finish |

---

# 🛡 Operational Notes

* **Official tooling only** — packaging goes through Microsoft's own IntuneWinAppUtil; no third-party wrappers touch your installers
* **Test before assigning** — upload the `.intunewin` to Intune as a pilot assignment first; verify install behavior on a test device
* **Keep sources clean** — one installer per Source folder avoids ambiguity in auto-detection
* **Audit trail** — the daily log under `C:\IntuneWinUtility\` records every packaging run

---

## 👤 Author

**Mohammad Abdulkader Omar**  
GitHub: [@mabdulkadr](https://github.com/mabdulkadr)  
Website: [momar.tech](https://momar.tech)  

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

## ⚠ Disclaimer

This skill and every script it generates are provided as-is with no warranty
of any kind. Test generated tools in a staging environment before deploying to
production. The authors assume no liability for any damage or data loss
resulting from their use.

---

<div align="center">

⭐ **If this tool saves you time, star the repo — it helps others find it.**

[Report an Issue](../../issues) · [momar.tech](https://momar.tech)

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/mabdulkadrx)

Built with [**PowerShell Enterprise Admin**](https://github.com/mabdulkadr/powershell-enterprise-admin-skill)

</div>
