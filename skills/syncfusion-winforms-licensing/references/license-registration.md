# Registering Syncfusion License Keys in Windows Forms Applications

This reference provides comprehensive guidance on registering Syncfusion license keys in Windows Forms applications using both C# and VB.NET.

## Overview

The generated license key is a string that must be registered **before any Syncfusion control is initiated** in your application. Registration is done using the `RegisterLicense` method from `Syncfusion.Licensing.SyncfusionLicenseProvider`.

## Basic Registration Syntax

```csharp
// SECURITY BEST PRACTICE: Read from environment variable
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

**Key Points:**
- **NEVER** hardcode license keys directly in source code
- Store keys in environment variables or secure configuration
- Ensure `Syncfusion.Licensing.dll` is referenced in your project
- Register before initializing any Syncfusion control
- Registration happens at application entry point
- Never commit license keys to version control

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

### Method 1: Registration in Main() Method with Environment Variable (Recommended)

The most secure and recommended approach is to register the license key in the `Main()` method of `Program.cs`, reading the key from an environment variable.

**File:** `Program.cs`

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Licensing;

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
            // SECURITY BEST PRACTICE: Read license key from environment variable
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            
            if (string.IsNullOrEmpty(licenseKey))
            {
                MessageBox.Show(
                    "Syncfusion license key not found. Please set SYNCFUSION_LICENSE_KEY environment variable.",
                    "Configuration Error",
                    MessageBoxButtons.OK,
                    MessageBoxIcon.Error);
                return;
            }
            
            // Register Syncfusion license BEFORE initializing any Syncfusion control
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

**Setting the Environment Variable:**

```powershell
# PowerShell - For current session
$env:SYNCFUSION_LICENSE_KEY = "your-license-key-value"

# PowerShell - Permanent (User level)
[System.Environment]::SetEnvironmentVariable("SYNCFUSION_LICENSE_KEY", "your-license-key-value", "User")
```

**Why This Approach Is Best:**
- Keeps sensitive data out of source code
- Prevents accidental exposure in version control
- Executes before any form is loaded
- Executes before any Syncfusion control is initialized
- Clean separation of licensing concern
- Easy to update without code changes
- Supports different keys per environment (dev/test/prod)

### Method 2: Registration with App.config (Alternative - Requires Proper Security)

**App.config (DO NOT commit to source control):**
```xml
<configuration>
  <appSettings>
    <!-- WARNING: This file should NOT be committed to source control -->
    <!-- Use environment-specific config files or User Secrets -->
    <add key="SyncfusionLicenseKey" value=""/>
  </appSettings>
</configuration>
```

**Program.cs:**
```csharp
using System;
using System.Configuration;
using System.Windows.Forms;
using Syncfusion.Licensing;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Read license key from config
        string licenseKey = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];
        
        if (string.IsNullOrEmpty(licenseKey))
        {
            MessageBox.Show(
                "Syncfusion license key not configured in App.config",
                "Configuration Error",
                MessageBoxButtons.OK,
                MessageBoxIcon.Error);
            return;
        }
        
        // Register license
        SyncfusionLicenseProvider.RegisterLicense(licenseKey);
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

**.gitignore entry (if using this method):**
```
# Exclude App.config with sensitive data
App.config
```

**Better:** Create App.config.template with empty values, commit that instead.

### Complete C# Example with Error Handling (Secure)

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
                // SECURITY BEST PRACTICE: Read from environment variable
                string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
                
                if (string.IsNullOrEmpty(licenseKey))
                {
                    MessageBox.Show(
                        "SYNCFUSION_LICENSE_KEY environment variable is not set.\n\n" +
                        "Please configure it before running the application.",
                        "Configuration Error",
                        MessageBoxButtons.OK,
                        MessageBoxIcon.Error);
                    return;
                }
                
                // Register Syncfusion license key
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

### Method 1: Application.Designer.vb with Environment Variable (Recommended)

If your VB.NET project uses the Application Framework, register the license key in the `Application.Designer.vb` file constructor, reading from an environment variable.

**File:** `Application.Designer.vb` (in `My Project` folder)

```vb
Imports System
Imports Syncfusion.Licensing

Namespace My
    
    'NOTE: This file is auto-generated; do not modify it directly. To make changes,
    'or if you encounter build errors in this file, go to the Project Designer
    '(go to Project Properties or double-click the My Project node in
    'Solution Explorer), and make changes on the Application tab.
    
    Partial Friend Class MyApplication
        
        <Global.System.Diagnostics.DebuggerStepThroughAttribute()>  _
        Public Sub New()
            MyBase.New(Global.Microsoft.VisualBasic.ApplicationServices.AuthenticationMode.Windows)
            
            ' SECURITY BEST PRACTICE: Read license key from environment variable
            Dim licenseKey As String = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY")
            
            If String.IsNullOrEmpty(licenseKey) Then
                MessageBox.Show( _
                    "SYNCFUSION_LICENSE_KEY environment variable is not set." & vbCrLf & vbCrLf & _
                    "Please configure it before running the application.", _
                    "Configuration Error", _
                    MessageBoxButtons.OK, _
                    MessageBoxIcon.Error)
                ' Note: Cannot easily exit here, validation will fail later
            Else
                ' Register Syncfusion License
                SyncfusionLicenseProvider.RegisterLicense(licenseKey)
            End If
            
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

**Setting the Environment Variable:**

```powershell
# PowerShell - For current session
$env:SYNCFUSION_LICENSE_KEY = "your-license-key-value"

# PowerShell - Permanent (User level)
[System.Environment]::SetEnvironmentVariable("SYNCFUSION_LICENSE_KEY", "your-license-key-value", "User")
```

**Important Notes:**
- If `Application.Designer.vb` doesn't exist, it will be generated in the `My Project` folder
- Register the key in the `New()` constructor
- Place registration before setting other properties
- Never hardcode license keys in source code

### Method 2: Program.vb with Environment Variable (For Projects Without Application Framework)

If you convert a C# project to VB.NET or disable the Application Framework, you may have a `Program.vb` file with a `Main()` method.

**File:** `Program.vb`

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Licensing

Module Program
    
    <STAThread()>
    Sub Main()
        ' SECURITY BEST PRACTICE: Read license key from environment variable
        Dim licenseKey As String = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY")
        
        If String.IsNullOrEmpty(licenseKey) Then
            MessageBox.Show( _
                "SYNCFUSION_LICENSE_KEY environment variable is not set." & vbCrLf & vbCrLf & _
                "Please configure it before running the application.", _
                "Configuration Error", _
                MessageBoxButtons.OK, _
                MessageBoxIcon.Error)
            Return
        End If
        
        ' Register Syncfusion License key
        SyncfusionLicenseProvider.RegisterLicense(licenseKey)
        
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

**Security Benefits:**
- Keeps license keys out of source code
- Prevents accidental exposure in version control

### Complete VB.NET Example (Secure)

**Application.Designer.vb:**

```vb
Imports System
Imports Syncfusion.Licensing

Namespace My

    Partial Friend Class MyApplication
        
        Public Sub New()
            MyBase.New(Global.Microsoft.VisualBasic.ApplicationServices.AuthenticationMode.Windows)
            
            ' SECURITY BEST PRACTICE: Read from environment variable
            Dim licenseKey As String = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY")
            
            If Not String.IsNullOrEmpty(licenseKey) Then
                ' Register Syncfusion License
                SyncfusionLicenseProvider.RegisterLicense(licenseKey)
            Else
                MessageBox.Show( _
                    "Syncfusion license key not configured in environment variables.", _
                    "Configuration Error", _
                    MessageBoxButtons.OK, _
                    MessageBoxIcon.Warning)
            End If
            
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
    // Read from environment variable (secure)
    string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    
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
        string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
        SyncfusionLicenseProvider.RegisterLicense(key);
    }
}
```

```csharp
// ❌ WRONG: Registering in Form_Load event (too late)
private void Form1_Load(object sender, EventArgs e)
{
    // Too late - form and controls already loaded!
    string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    SyncfusionLicenseProvider.RegisterLicense(key);
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

### 1. Security Considerations - CRITICAL

**🔒 NEVER hardcode license keys in source code or commit them to version control!**

```csharp
// ❌ DANGEROUS: Hardcoded license key - NEVER DO THIS
// This exposes your license key in source control and to anyone with code access
SyncfusionLicenseProvider.RegisterLicense("Mgo+DSMBaFt...");
```

**✅ Secure Approaches:**

**Option A: Environment Variable (RECOMMENDED)**
```csharp
// Read from environment variable - keeps keys out of source code
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");

if (string.IsNullOrEmpty(licenseKey))
{
    throw new InvalidOperationException("SYNCFUSION_LICENSE_KEY environment variable not set");
}

SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

**Option B: Configuration File (with proper .gitignore)**
```csharp
// Only if App.config is excluded from source control
string licenseKey = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];

if (string.IsNullOrEmpty(licenseKey))
{
    throw new InvalidOperationException("License key not configured in App.config");
}

SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

**Option C: Azure Key Vault or Secret Management**
```csharp
// For production applications, use proper secret management
// Example with Azure Key Vault, AWS Secrets Manager, etc.
```

**Add to .gitignore:**
```
# Never commit these files with real license keys
App.config
appsettings.json
*.user
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

**Secure Templates to Use:**

**C# (Environment Variable - RECOMMENDED):**
```csharp
// Read from environment variable - SECURITY BEST PRACTICE
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
if (!string.IsNullOrEmpty(licenseKey))
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey);
}
```

**VB.NET (Environment Variable - RECOMMENDED):**
```vb
' Read from environment variable - SECURITY BEST PRACTICE
Dim licenseKey As String = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY")
If Not String.IsNullOrEmpty(licenseKey) Then
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(licenseKey)
End If
```

**Setting Environment Variable:**
```powershell
# PowerShell
[System.Environment]::SetEnvironmentVariable("SYNCFUSION_LICENSE_KEY", "your-key-here", "User")
```

## Next Steps

- For troubleshooting license errors, read [licensing-errors.md](licensing-errors.md)
- For CI/CD license validation, read [licensing-faqs.md](licensing-faqs.md)
- For generating license keys, read [license-generation.md](license-generation.md)
