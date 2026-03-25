# Upgrading Syncfusion WinForms Components

Syncfusion releases new versions every three months (Volume releases) plus Service Pack releases for major bug fixes. You can upgrade from any installed version to the latest.

> See the [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/winforms-ui-controls) for breaking changes, bug fixes, new features, and known issues between versions.

---

## Option 1: Upgrade via Installer (Website / Control Panel)

Use this approach when you installed Syncfusion via the offline installer.

### From Control Panel

1. Open the **Syncfusion Control Panel**.
2. Click the **"Latest Version: {Version}"** link at the top.
3. Download and install the latest version.

### From Website

1. Log in to your Syncfusion account.
2. Go to [License & Downloads](https://www.syncfusion.com/account/downloads).
3. Download the latest installer for Windows Forms.
4. Run the installer — existing versions do not need to be uninstalled first.

> Service Pack releases are independent of Volume releases. You can skip a Volume release and install a Service Pack directly.

---

## Option 2: Upgrade via Visual Studio Extensions

Use this approach to update the Syncfusion VS Extension itself (project templates, toolbox, migration wizard).

1. In Visual Studio, go to **Extensions > Manage Extensions > Updates**.
   - *VS 2017 or lower:* go to **Tools > Extensions and Updates**.
2. Find the **Syncfusion WinForms** extension in the list.
3. Click **Update**.
4. Close Visual Studio.
5. In the VSIX Installer dialog that appears, click **Modify** to complete the update.

---

## Option 3: Upgrade NuGet Packages

Use this approach when your project uses Syncfusion WinForms NuGet packages from nuget.org.

### Via Package Manager UI

1. Right-click the project or solution in Solution Explorer → **Manage NuGet Packages...**.
2. Go to the **Updates** tab.
3. Search for `Syncfusion` to find all installed Syncfusion packages.
4. Select the package(s) to update. Choose a specific version if needed.
5. Click **Update** and accept the license terms.

To update multiple packages at once, select the checkboxes next to each package, then click **Update**.

### Via Package Manager Console

```powershell
# Update a specific package to the latest version
Update-Package Syncfusion.Grid.Windows

# Update to a specific version
Update-Package Syncfusion.Grid.Windows -Version 19.2.0.59

# Update a package in a specific project only
Update-Package Syncfusion.Grid.Windows -ProjectName SyncfusionWinformsApp
```

### Via .NET CLI

The .NET CLI always installs the latest version unless you specify a version:

```bash
# Update to latest (re-adds package with latest version)
dotnet add package Syncfusion.Grid.Windows

# Update to a specific version
dotnet add package Syncfusion.Grid.Windows -v 19.2.0.59
```

---

## Upgrading from Trial to Licensed

**Option A — Reinstall:**
1. Uninstall the trial installer.
2. Download and install the licensed build from [License & Downloads](https://www.syncfusion.com/account/downloads).

**Option B — Replace license key (NuGet users):**
1. Generate a paid license key from [License & Downloads](https://www.syncfusion.com/account/downloads).
2. Replace the trial key in `Program.cs` with the paid key.

> License registration is **not required** when referencing assemblies from the Licensed installer — only from trial installers and NuGet packages.

---

## Gotchas

- **No need to uninstall first:** Installing a newer version alongside an existing one is supported. The installer handles coexistence.
- **Keep all packages the same version:** After upgrading, ensure every Syncfusion NuGet package in the project is updated to the same version. Mixed versions cause runtime assembly conflicts.
- **Check breaking changes:** Always review the Upgrade Guide before upgrading major or minor versions — some releases include API changes or removed members that may require code updates.
- **VS Extension update applies to toolbox only:** Updating the VS Extension does not upgrade the NuGet packages or assemblies in your project. Do that separately via NuGet.
