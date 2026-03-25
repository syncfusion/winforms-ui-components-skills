# Installing Syncfusion WinForms Controls

## Table of Contents
- [Downloading the Installer](#downloading-the-installer)
- [Installing with the UI Wizard](#installing-with-the-ui-wizard)
- [Silent Command-Line Installation](#silent-command-line-installation)
- [Installing via NuGet Package Manager UI](#installing-via-nuget-package-manager-ui)
- [Installing via .NET CLI](#installing-via-net-cli)
- [Installing via Package Manager Console](#installing-via-package-manager-console)
- [Common Installation Errors](#common-installation-errors)

---

## Downloading the Installer

### Trial Version

**Option A — Download Free Trial Setup:**
1. Visit [syncfusion.com/downloads](https://www.syncfusion.com/downloads) and select Windows Forms.
2. Log in or complete the registration form.
3. Download the Windows Forms trial installer from the confirmation page.
4. Only the latest version's trial installer is available for download.

**Option B — Start Trial via NuGet (no download):**
1. Go to [Start Trials](https://www.syncfusion.com/account/manage-trials/start-trials) in your Syncfusion account.
2. Select the Windows Forms product to begin the 30-day trial.
3. Generate a license key from [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads).

### Licensed Version

1. Log in to your Syncfusion account.
2. Go to [License & Downloads](https://www.syncfusion.com/account/downloads).
3. Click **Download** next to the Windows Forms product.
4. For older versions, click **Downloads Older Versions**.
5. Both EXE and ZIP formats are available for Windows.

---

## Installing with the UI Wizard

1. Double-click the downloaded installer `.exe`. The wizard opens and extracts the package.

2. **Unlock the installer** using one of two methods:

   **Login to Install:**  
   Enter your Syncfusion email and password. Click **Next**.

   **Use Unlock Key:**  
   Enter your trial or licensed unlock key. Trial keys are valid for 30 days.

3. Read and accept the **License Terms and Privacy Policy** checkbox → click **Next**.

4. Set install location and sample location. Configure **Additional Settings**:
   - **Install Demos** — install Syncfusion sample projects
   - **Register Syncfusion Assemblies in GAC** — add assemblies to Global Assembly Cache
   - **Configure Syncfusion controls in Visual Studio** — add controls to VS Toolbox *(requires GAC registration to also be checked)*
   - **Configure Syncfusion Extensions in Visual Studio** — add Syncfusion VS Extensions
   - **Create Desktop Shortcut** / **Create Start Menu Shortcut**

5. If a previous version is installed, the **Uninstall Previous Version(s)** dialog appears. Check the versions to uninstall → click **Proceed**.

6. The installer shows progress and completes. Click **Launch Control Panel** to open the Syncfusion Control Panel, then **Finish**.

---

## Silent Command-Line Installation

### Silent Install

```console
"D:\Temp\syncfusionessentialwindowsforms_x.x.x.x.exe" /Install silent ^
  /UNLOCKKEY:"your-unlock-key" ^
  /log "C:\Temp\install.log" ^
  /InstallPath:C:\Syncfusion\x.x.x.x ^
  /InstallSamples:true ^
  /InstallAssemblies:true ^
  /UninstallExistAssemblies:true ^
  /InstallToolbox:true
```

**Parameters:**

| Parameter | Description |
|---|---|
| `/Install silent` | Run installation without UI |
| `/UNLOCKKEY:` | Trial or licensed unlock key |
| `/log` | (Optional) Path for installation log |
| `/InstallPath:` | (Optional) Custom install directory |
| `/InstallSamples:` | `true`/`false` — install sample projects |
| `/InstallAssemblies:` | `true`/`false` — register assemblies in GAC |
| `/UninstallExistAssemblies:` | `true`/`false` — remove previous version assemblies |
| `/InstallToolbox:` | `true`/`false` — configure VS Toolbox |

> Replace `x.x.x.x` with the actual version number.

### Silent Uninstall

```console
"D:\Temp\syncfusionessentialwindowsforms_x.x.x.x.exe" /uninstall silent
```

---

## Installing via NuGet Package Manager UI

Syncfusion WinForms NuGet packages are published at [nuget.org](https://www.nuget.org/packages?q=Tags%3A%22Winforms%22+syncfusion). Available from v16.2.0.46 onward.

1. Right-click the WinForms project in Solution Explorer → **Manage NuGet Packages...**  
   *(or **Tools > NuGet Package Manager > Manage NuGet Packages for Solution...**)*

2. In the **Browse** tab, search for `Syncfusion.WinForms` or the specific control package (e.g., `Syncfusion.Grid.Windows`).

3. Select the desired package. The right panel shows package details.

4. Choose the required version (latest selected by default).

5. Click **Install** and accept the license terms.

---

## Installing via .NET CLI

```bash
# Install the latest version
dotnet add package Syncfusion.Grid.Windows

# Install a specific version
dotnet add package Syncfusion.Grid.Windows -v 19.2.0.57
```

After running the command, run `dotnet restore` if needed (automatic with `dotnet build` in .NET Core 2.0+).

---

## Installing via Package Manager Console

Open **Tools > NuGet Package Manager > Package Manager Console** in Visual Studio.

```powershell
# Install in the default project
Install-Package Syncfusion.Grid.Windows

# Install in a specific project
Install-Package Syncfusion.Grid.Windows -ProjectName SyncfusionWinformsApp

# Install a specific version
Install-Package Syncfusion.Grid.Windows -Version 19.2.0.59
```

---

## Common Installation Errors

### "Trial unlock key cannot unlock the licensed installer"

**Cause:** You used a trial unlock key on a licensed installer.  
**Solution:** Use a licensed unlock key. Generate one from your account's KB article on unlock key generation.

### "License has expired"

**Cause:** Your Syncfusion subscription has expired.  
**Solution:** Renew at [My Renewals](https://www.syncfusion.com/account/my-renewals), purchase a new license, or contact sales@syncfusion.com.

### "Unable to find a valid license or trial"

**Cause:** Account has no active license or trial, trial has expired, or you are not the license holder.  
**Solution:** Purchase a new license, contact your account administrator, or email clientrelations@syncfusion.com.

### "Another installation is in progress"

**Cause:** Another MSI-based installer is running in the background.  
**Solution:**
1. Open **Task Manager > Details** tab.
2. Find and end the `msiexec.exe` process.
3. Retry the Syncfusion installer. If the issue persists, restart the computer first.

### "Unable to install due to controlled folder access"

**Cause:** Windows Controlled Folder Access is enabled, blocking installation to protected directories (e.g., Documents).  
**Solution:**
- **Option A:** Temporarily disable Controlled Folder Access in Windows Security settings, install, then re-enable.
- **Option B:** Choose an alternative (unprotected) install directory instead of the default Documents path.
