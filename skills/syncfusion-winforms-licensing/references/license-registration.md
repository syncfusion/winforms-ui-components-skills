# Registering Syncfusion License Keys in Windows Forms Applications

This reference provides comprehensive guidance on registering Syncfusion license keys in Windows Forms applications using both C# and VB.NET.

## Overview

The generated license key is a string that must be registered **before any Syncfusion control is initiated** in your application. Registration is done using the `RegisterLicense` method from `Syncfusion.Licensing.SyncfusionLicenseProvider`.

## Basic Registration Syntax

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

**Key Points:**
- Place the license key between double quotes
- Ensure `Syncfusion.Licensing.dll` is referenced in your project
- Register before initializing any Syncfusion control
- Registration happens at application entry point

## Prerequisites

### 1. Reference Syncfusion.Licensing.dll

Before registering, ensure your project references the `Syncfusion.Licensing` assembly:

**In Visual Studio:**
1. Right-click on project → Add → Reference
2. Browse to Syncfusion installation folder or use NuGet
3. Add reference to `Syncfusion.Licensing.dll`

**Via NuGet:**
```
Install-Package Syncfusion.Licensing
```

### 2. Verify Copy Local Setting

Ensure `Copy Local` is set to `True` for `Syncfusion.Licensing.dll`:

1. In Solution Explorer, expand References
2. Right-click `Syncfusion.Licensing` → Properties
3. Set `Copy Local` = `True`

This ensures the DLL is copied to the output folder during build.

## Registration in C# Windows Forms

### Method 1: Registration in Main() Method (Recommended)

The most common and recommended approach is to register the license key in the `Main()` method of `Program.cs`, **before** calling `Application.Run()`.

**File:** `Program.cs`

```csharp
using System;
using System.Windows.Forms;

namespace MyWindowsFormsApp
{
    static class Program
    {
        /// <summary>
        /// The main entry point for the application.
        /// </summary>
        [STAThread]
        static void Main()
        {
            // IMPORTANT: Register Syncfusion license BEFORE initializing any Syncfusion control
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

**Why This Works:**
- Executes before any form is loaded
- Executes before any Syncfusion control is initialized
- Clean separation of licensing concern
- Easy to locate and update

### Method 2: Registration in App.config (Not Recommended)

While you can store the key in `App.config`, you still need to call `RegisterLicense()` in code:

**App.config:**
```xml
<configuration>
  <appSettings>
    <add key="SyncfusionLicenseKey" value="YOUR_LICENSE_KEY_HERE"/>
  </appSettings>
</configuration>
```

**Program.cs:**
```csharp
using System;
using System.Configuration;
using System.Windows.Forms;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Read license key from config
        string licenseKey = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];
        
        // Register license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

### Complete C# Example with Error Handling

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Licensing;

namespace SyncfusionWindowsFormsApp
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            try
            {
                // Register Syncfusion license key
                string licenseKey = "Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p/VWJYdVt5flBPcDwsT3RfQF5jS39TdkNnWHxedXRTRA==";
                SyncfusionLicenseProvider.RegisterLicense(licenseKey);
                
                Application.EnableVisualStyles();
                Application.SetCompatibleTextRenderingDefault(false);
                Application.Run(new MainForm());
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Application startup error: {ex.Message}", "Error", 
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}
```

## Registration in VB.NET Windows Forms

VB.NET Windows Forms applications have different entry point configurations. The registration location depends on your project structure.

### Method 1: Application.Designer.vb (Recommended for VB with Application Framework)

If your VB.NET project uses the Application Framework, register the license key in the `Application.Designer.vb` file constructor.

**File:** `Application.Designer.vb` (in `My Project` folder)

```vb
Namespace My
    
    'NOTE: This file is auto-generated; do not modify it directly. To make changes,
    'or if you encounter build errors in this file, go to the Project Designer
    '(go to Project Properties or double-click the My Project node in
    'Solution Explorer), and make changes on the Application tab.
    
    Partial Friend Class MyApplication
        
        <Global.System.Diagnostics.DebuggerStepThroughAttribute()>  _
        Public Sub New()
            MyBase.New(Global.Microsoft.VisualBasic.ApplicationServices.AuthenticationMode.Windows)
            
            'Register Syncfusion License
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE")
            
            Me.IsSingleInstance = False
            Me.EnableVisualStyles = True
            Me.SaveMySettingsOnExit = True
            Me.ShutdownStyle = Global.Microsoft.VisualBasic.ApplicationServices.ShutdownMode.AfterMainFormCloses
        End Sub
        
        <Global.System.Diagnostics.DebuggerStepThroughAttribute()>  _
        Protected Overrides Sub OnCreateMainForm()
            Me.MainForm = Global.MyWindowsFormsApp.Form1
        End Sub
    End Class
End Namespace
```

**Important Notes:**
- If `Application.Designer.vb` doesn't exist, it will be generated in the `My Project` folder
- Register the key in the `New()` constructor
- Place registration before setting other properties

### Method 2: Program.vb (For Projects Without Application Framework)

If you convert a C# project to VB.NET or disable the Application Framework, you may have a `Program.vb` file with a `Main()` method.

**File:** `Program.vb`

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Licensing

Module Program
    
    <STAThread()>
    Sub Main()
        'Register Syncfusion License key
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE")
        
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
    
End Module
```

**When to Use This:**
- Project uses Program.vb as entry point
- Application Framework is disabled
- Converted from C# to VB.NET

### Complete VB.NET Example

**Application.Designer.vb:**

```vb
Namespace My

    Partial Friend Class MyApplication
        
        Public Sub New()
            MyBase.New(Global.Microsoft.VisualBasic.ApplicationServices.AuthenticationMode.Windows)
            
            'Register Syncfusion License - Replace with your actual license key
            Dim licenseKey As String = "Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p/VWJYdVt5flBPcDwsT3RfQF5jS39TdkNnWHxedXRTRA=="
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey)
            
            Me.IsSingleInstance = False
            Me.EnableVisualStyles = True
            Me.SaveMySettingsOnExit = True
            Me.ShutdownStyle = Global.Microsoft.VisualBasic.ApplicationServices.ShutdownMode.AfterMainFormCloses
        End Sub
        
        Protected Overrides Sub OnCreateMainForm()
            Me.MainForm = Global.MyApp.MainForm
        End Sub
        
    End Class
    
End Namespace
```

## Registration Timing

### ✅ Correct Timing

```csharp
static void Main()
{
    // ✅ CORRECT: Register BEFORE Application.Run()
    SyncfusionLicenseProvider.RegisterLicense("LICENSE_KEY");
    
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);
    Application.Run(new Form1());
}
```

### ❌ Incorrect Timing

```csharp
// ❌ WRONG: Registering in Form constructor (too late)
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent(); // Syncfusion controls may be initialized here
        
        // Too late - controls already initialized!
        SyncfusionLicenseProvider.RegisterLicense("LICENSE_KEY");
    }
}
```

```csharp
// ❌ WRONG: Registering in Form_Load event (too late)
private void Form1_Load(object sender, EventArgs e)
{
    // Too late - form and controls already loaded!
    SyncfusionLicenseProvider.RegisterLicense("LICENSE_KEY");
}
```

**Why Timing Matters:**
- License must be registered before any Syncfusion control initializes
- Controls in designer are initialized in `InitializeComponent()`
- `InitializeComponent()` is called in form constructor
- Therefore, registration must happen before form instantiation

## Offline Validation

**Important:** Syncfusion license validation is done **offline** during application execution.

Key characteristics:
- **No internet required:** Applications do not need internet access for license validation
- **Runtime validation:** Validation happens when application starts
- **Deployment friendly:** Apps can be deployed to systems without internet connectivity
- **Local validation:** License is validated against embedded metadata in assemblies

```csharp
// This works offline - no internet connection needed
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

## Multiple Syncfusion Components

If your application uses multiple Syncfusion components (Grid, Chart, PDF, etc.):

### Single Registration Covers All

**Good news:** You only need to register the license key **once** in your application, regardless of how many Syncfusion components you use.

```csharp
static void Main()
{
    // Register once for all Syncfusion components
    SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    
    // Now all Syncfusion components are licensed:
    // - DataGrid
    // - Chart
    // - PDF
    // - Excel
    // - etc.
    
    Application.Run(new Form1());
}
```

## Best Practices

### 1. Security Considerations

**Don't hardcode license keys in source code:**

```csharp
// ❌ Bad: Hardcoded license key
SyncfusionLicenseProvider.RegisterLicense("Mgo+DSMBaFt...");
```

**Better approaches:**

**Option A: Environment Variable**
```csharp
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

**Option B: Configuration File**
```csharp
string licenseKey = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

**Option C: Secure Configuration**
```csharp
// Encrypt sensitive sections in app.config
// Read from encrypted configuration
```

### 2. Version Management

When upgrading Syncfusion versions:

1. Generate new license key for new version
2. Update the registered license key in code
3. Test application to verify licensing works
4. Deploy with new key

### 3. Error Handling

Add validation to ensure license key exists:

```csharp
static void Main()
{
    string licenseKey = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];
    
    if (string.IsNullOrEmpty(licenseKey))
    {
        MessageBox.Show("Syncfusion license key not configured!", "Configuration Error");
        return;
    }
    
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    
    Application.Run(new Form1());
}
```

### 4. Team Development

For team environments:

1. Store license key in shared configuration (not source control)
2. Document where team members can get the key
3. Use environment-specific keys for dev/test/prod
4. Include licensing setup in onboarding documentation

## Troubleshooting Registration Issues

### Issue: "Syncfusion.Licensing.dll not found"

**Solution:**
1. Verify `Syncfusion.Licensing.dll` is referenced
2. Set `Copy Local = True`
3. Rebuild project
4. Check bin/Debug or bin/Release folder for DLL

### Issue: License error still appears after registration

**Solution:**
1. Verify license key is for correct version
2. Verify license key is for Windows Forms platform
3. Ensure registration happens before control initialization
4. Check that license key is complete (not truncated)

### Issue: Registration code not executing

**Solution:**
1. Set breakpoint on RegisterLicense line
2. Verify Main() method is the entry point
3. Check project properties → Startup object
4. Ensure no exceptions are swallowed

## Summary

**Key Registration Rules:**

1. **Timing:** Register before any Syncfusion control initialization
2. **Location:** 
   - C#: In `Main()` method before `Application.Run()`
   - VB.NET: In `Application.Designer.vb` constructor or `Program.vb`
3. **Frequency:** Register once per application run
4. **Scope:** One registration covers all Syncfusion components
5. **Dependencies:** Ensure `Syncfusion.Licensing.dll` is referenced
6. **Validation:** Happens offline, no internet required

**Template to Copy:**

**C#:**
```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

**VB.NET:**
```vb
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY")
```

## Next Steps

- For troubleshooting license errors, read [licensing-errors.md](licensing-errors.md)
- For CI/CD license validation, read [licensing-faqs.md](licensing-faqs.md)
- For generating license keys, read [license-generation.md](license-generation.md)
