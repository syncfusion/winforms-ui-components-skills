# System Requirements for Syncfusion WinForms Controls

This reference covers the minimum and recommended system requirements for developing and running applications with Syncfusion® Windows Forms controls.

---

## Operating Systems

Syncfusion WinForms controls are supported on the following Windows versions:

- Windows 2000
- Windows XP
- Windows Vista
- Windows 7
- Windows 8
- Windows 10
- Windows 11
- Windows Server 2003 and later

---

## Hardware Requirements

| Component | Minimum | Recommended |
|---|---|---|
| Processor | x86 or x64 | x64 |
| RAM | 512 MB | 1 GB or more |
| Hard Disk | 400 MB free on boot drive | 4 GB (for full installation with samples) |

> Even when installing to a non-boot drive, 400 MB of free space is required on the boot drive.

---

## Development Environment

### Visual Studio

Syncfusion WinForms controls are supported in:
- Visual Studio 2015
- Visual Studio 2017
- Visual Studio 2019
- Visual Studio 2022

### .NET Framework

- **.NET Framework 4.6.2** is the baseline supported version.
- Assemblies compiled for .NET 4.6.2 are compatible with applications targeting .NET 4.7, 4.7.1, 4.7.2, and 4.8.
- **Example:** You can reference a `Syncfusion` assembly built for .NET 4.6.2 in a project targeting .NET 4.8.

### .NET (Modern)

Syncfusion WinForms also supports modern .NET versions (see `.NET Compatibility` reference for the full version matrix):
- .NET 8.0
- .NET 9.0

---

## Gotchas

- **Boot drive space:** The installer always writes some files to the boot drive regardless of the chosen install path. Ensure at least 400 MB free on `C:\` even when installing elsewhere.
- **Visual Studio version:** Syncfusion VS Extensions (toolbox configuration, project templates, migration wizard) require the matching VS version. Using an older VS with newer Syncfusion versions may limit extension features.
- **Classic controls:** Controls labeled `classic` in the Syncfusion documentation do not support .NET Core / modern .NET. Use the non-classic equivalents for .NET 6+ projects.
