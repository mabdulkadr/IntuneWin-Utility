<div align="center">

# 📦 IntuneWin Utility

**Intune Win32 packaging, point and click**

Turn EXE, MSI, PS1, or BAT installers into `.intunewin` packages with Microsoft's IntuneWinAppUtil. You pick the folder and the tool handles the command line.

[![Mode](https://img.shields.io/badge/Mode-CLI-334155?style=for-the-badge)](#-usage)
[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-5391FE?style=for-the-badge&logo=powershell&logoColor=white)](https://learn.microsoft.com/en-us/powershell/)
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-0F172A?style=for-the-badge)](#%EF%B8%8F-requirements)
[![UI](https://img.shields.io/badge/UI-WPF%20Tailwind%20Slate-3B82F6?style=for-the-badge&logo=windows&logoColor=white)](#%EF%B8%8F-usage)
[![Intune](https://img.shields.io/badge/Intune-Win32%20Apps-0078D4?style=for-the-badge)](#-overview)
[![License](https://img.shields.io/badge/License-MIT-F59E0B?style=for-the-badge)](#-license)
[![Version](https://img.shields.io/badge/Version-1.1-334155?style=for-the-badge)](#-overview)

[Overview](#-overview) • [Features](#-core-features) • [Usage](#-usage) • [After Packaging](#-after-packaging--upload-to-intune) • [Requirements](#%EF%B8%8F-requirements) • [FAQ](#-faq) • [License](#-license)

</div>

---

# 📖 Overview

IntuneWin Utility creates Intune Win32 packages (`.intunewin`) from EXE, MSI, PS1, and BAT installers. It calls Microsoft's own IntuneWinAppUtil.exe, finds the installer for you, and logs each step in the window.

You do not have to remember `IntuneWinAppUtil.exe -c <folder> -s <setup> -o <output>`. Choose a source folder, check the installer it found, and click once.

---

# ✨ Core Features

### 🔹 Core Packaging
* Converts EXE, MSI, PS1, and BAT installers to `.intunewin`
* Uses Microsoft's official IntuneWinAppUtil
* Packaging runs in the background, so the window stays usable
* The Message Center shows progress as it happens
* The output name and size are shown when the job finishes

### 🔹 Smart User Experience
* Finds the installer inside the source folder
* If several installers are present, it picks the first and shows a green SUCCESS message
* It shows the source file name rather than the full path, along with the package name and size
* Only one packaging job can run at a time

### 🔹 Validation Engine
* The source folder must exist
* The installer must live inside that folder
* Only `.exe`, `.msi`, `.ps1`, and `.bat` files are accepted
* The output folder is created if it is missing
* A missing `IntuneWinAppUtil.exe` is reported

### 🔹 Logging
Each run writes a log under `C:\IntuneWinUtility\`.

---

# 📂 Project Structure

```text
IntuneWin-Utility/
│
├── IntuneWin Utility.ps1      # WPF application source
├── IntuneWinAppUtil.exe       # Microsoft's packaging utility
├── Screenshot.png             # Documentation screenshot
└── README.md
```

---

# 🚀 Usage

### Run

Double-click:

```text
IntuneWinAppUtil.exe
```

That is Microsoft's packaging utility. The WPF front end is `IntuneWin Utility.ps1`; the `.exe` wrapper is not shipped under the name shown in older docs.

### Steps

1. Select the source folder
2. The tool finds the EXE, MSI, PS1, or BAT installer
3. Choose where to write the package: the same folder or a custom one
4. Click **Create Package**

### Auto-Detection Behavior

After you pick a source folder, the tool searches for `.exe`, `.msi`, `.ps1`, and `.bat` files. It selects the one it finds and logs a green SUCCESS message. If there are several, it picks the first and logs how many it found.

### Output Result

When packaging finishes, the tool shows the output name, its size, a success message, and a button to open the folder.

```text
AppName.intunewin (45.3 MB)
```

---

# 📁 Recommended Source Folder Layout

Keep one installer per source folder so detection has no ambiguity.

```text
Source\
├── AppSetup.exe            # the installer (auto-detected)
├── config.mst              # transforms / config files
├── logo.png                # app icon for Intune
└── readme.txt              # any extra content to ship
```

Everything in the source folder ends up in the `.intunewin`, so include only what the installation needs.

---

# 📤 After Packaging: Upload to Intune

The `.intunewin` file is only the package. To deploy it:

1. In the Intune admin center, go to **Devices** > **Windows** > **Apps** > **Add**
2. Choose **Windows app (Win32)** and select the `.intunewin` file
3. Fill in the name, description, and publisher
4. Enter install and uninstall commands, for example:

| Field | Example (EXE installer) |
|---|---|
| **Install command** | `AppSetup.exe /S` |
| **Uninstall command** | `AppSetup.exe /S /uninstall` (use the vendor's switches) |
| **Install behavior** | System or User, to match the installer |

5. Set the requirements and detection rules, such as a file version or MSI product code
6. Assign to a pilot group first, then widen the assignment

The exact install and uninstall switches come from the installer vendor. Test them by hand before assigning the app to users.

---

# ⚙️ Requirements

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10 / 11 |
| **PowerShell** | 5.1 or later; the packaged EXE needs no console |
| **IntuneWinAppUtil.exe** | Microsoft's official tool must be available to the utility |
| **Internet** | Not needed for packaging. Needed later to upload to Intune |

### Working Structure

Created on first run:

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
| "IntuneWinAppUtil.exe not found" | Microsoft's tool is missing | Put it where the utility expects it |
| Installer not detected | Unsupported extension or wrong folder | Put an `.exe`, `.msi`, `.ps1`, or `.bat` in the source folder |
| Source folder validation fails | Missing path or installer outside the folder | Keep the installer inside the selected source folder |
| Create Package is disabled | Another job is running | Wait for the current job to finish |
| Output file is too large | Extra files in the source folder | Remove content the installation does not need |
| Package fails to upload in Intune | Empty or corrupt source at package time | Re-package with the installer present and try again |

---

# ❓ FAQ

**What is a `.intunewin` file?**
Microsoft's encrypted package format for Win32 apps in Intune. It bundles the installer and its content so Intune can download and run it on managed devices.

**Where do I get IntuneWinAppUtil.exe?**
It is part of Microsoft's [Microsoft-Win32-Content-Prep-Tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) repository on GitHub.

**Can I package an MSI with a transform (.mst)?**
Yes. Put the `.msi` and `.mst` in the same source folder. The transform is applied through the install command in Intune, for example `msiexec /i App.msi TRANSFORMS=config.mst /qn`.

**Does the tool upload anything to Intune?**
No. It only creates the `.intunewin` locally. Uploading happens in the Endpoint Manager portal.

**Do PS1 scripts need a particular execution policy?**
Intune runs Win32 app commands in its own context, which bypasses the local execution policy for the script it invokes.

---

# 🛡 Operational Notes

* Packaging uses Microsoft's own IntuneWinAppUtil. No third-party wrapper touches your installers
* Upload the package as a pilot assignment first and check install behavior on a test device
* Keep one installer per source folder so detection stays clear
* Silent-install flags such as `/S`, `/qn`, and `/VERYSILENT` come from the vendor. Verify them in a manual run
* The daily log under `C:\IntuneWinUtility\` records every packaging run

---

## 👤 Author

**Mohammad Abdelkader Omar**  
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
