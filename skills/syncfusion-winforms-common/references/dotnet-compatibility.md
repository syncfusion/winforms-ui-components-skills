# .NET Compatibility for Syncfusion WinForms Controls

Syncfusion® Windows Forms controls support both the classic .NET Framework and modern .NET (formerly .NET Core). This reference covers how to create WinForms apps targeting modern .NET and shows the version compatibility matrix.

---

## .NET Framework and .NET Core Compatibility Matrix

The table below shows which Syncfusion Essential Studio versions support each .NET target:

| Syncfusion Version | .NET 2.0 | .NET 3.5 | .NET 4.0 | .NET 4.5 | .NET 4.6.2+ | .NET 6.0 | .NET 7.0 | .NET 8.0 | .NET 9.0 | .NET 10.0 |
|---|---|---|---|---|---|---|---|---|---|---|
| From 31.2 (2025 Vol3 SP) | No | No | No | No | Yes | No | No | Yes | Yes | Yes |
| From 29.1 (2025 Vol1) | No | No | No | No | Yes | No | No | Yes | Yes | No |
| From 28.1 (2024 Vol4) | No | No | No | No | Yes | Yes | Yes | Yes | Yes | No |
| From 27.2 (2024 Vol3 SP) | No | No | Yes | No | Yes | Yes | Yes | Yes | Yes | No |
| From 23.2 (2023 Vol3 SP) | No | No | Yes | Yes | Yes | Yes | Yes | Yes | No | No |
| From 21.1 (2023 Vol1) | No | No | Yes | Yes | Yes | Yes | Yes | No | No | No |
| From 20.4 (2022 Vol4) | Yes | Yes | Yes | Yes | Yes | Yes | Yes | No | No | No |
| From 19.4 (2021 Vol4) | Yes | Yes | Yes | Yes | Yes | Yes | No | No | No | No |
| From 18.4 (2020 Vol4) | Yes | Yes | Yes | Yes | Yes | No | No | No | No | No |
| From 17.2 (2019 Vol2) | Yes | Yes | Yes | Yes | Yes | No | No | No | No | No |
| Earlier versions | Yes | Yes | Yes | No | No | No | No | No | No | No |

---

## Creating a WinForms App with .NET Core / Modern .NET

### Step 1: Create the Project

1. Open Visual Studio → **File > New > Project**.
2. Select **Windows Forms App (.NET Core)** (or **Windows Forms App** for .NET 5+) → click **Next**.
3. Fill in project name and location → click **Next**.
4. In **Additional Information**, select the .NET version (e.g., .NET 6.0 LTS or .NET 8.0) → click **Create**.

### Step 2: Add Syncfusion Controls

**Option A — Assembly Deployment:**

1. In Solution Explorer, right-click **Dependencies** → **Add Reference**.
2. In Reference Manager, click **Browse**.
3. Navigate to the Syncfusion `.NET Core` assemblies folder:
   ```
   C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\net6.0\
   ```
4. Select the required assemblies and click **Add** → **OK**.

**Option B — NuGet Package:**

```powershell
# Package Manager Console
Install-Package Syncfusion.Shared.Base
```

```bash
# .NET CLI
dotnet add package Syncfusion.Shared.Base
```

### Step 3: Add a Control (Example — ButtonAdv)

```csharp
// Required assembly: Syncfusion.Shared.Base
using Syncfusion.Windows.Forms;

ButtonAdv button = new ButtonAdv();
button.Text = "ButtonAdv";
this.Controls.Add(button);
```

> Configure toolbox for .NET 6.0/8.0 projects from NuGet packages via the Syncfusion VS Extension toolbox configurator.

---

## Key Notes

- **Classic controls are not supported on .NET Core / modern .NET.** Controls labeled `classic` in Syncfusion documentation only work with .NET Framework. Use non-classic equivalents (e.g., `SfDataGrid` instead of the classic `GridDataBoundGrid`).
- **Assembly path for .NET Core:** Assemblies are located in the `net6.0` (or `net8.0`) subfolder under the precompiled assemblies path, not in the standard framework path.
- **NuGet is the recommended approach** for .NET 6+ projects — it handles transitive dependencies and version management automatically.
- **Version alignment:** All Syncfusion NuGet packages in a project must be the same version. Mixing versions causes runtime conflicts.
- **Toolbox configuration:** In .NET 5/6/7/8 projects, Syncfusion controls do not appear in the Toolbox automatically. Use the Syncfusion VS Extension to configure the toolbox from installed NuGet packages.
