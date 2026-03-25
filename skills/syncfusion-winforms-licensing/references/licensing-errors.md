# Syncfusion Licensing Errors and Solutions

## Table of Contents
- [Overview](#overview)
- [Common Licensing Errors (v20.4.0.38+)](#common-licensing-errors-v2040038)
  - [License Key Not Registered / Trial Expired](#license-key-not-registered--trial-expired)
  - [Invalid Key](#invalid-key)
- [Legacy Licensing Errors (v16.2.0 - v20.3.0)](#legacy-licensing-errors-v1620---v2030)
  - [License Key Not Registered](#license-key-not-registered-legacy)
  - [Invalid Key](#invalid-key-legacy)
  - [Trial Expired](#trial-expired-legacy)
  - [Platform Mismatch](#platform-mismatch-legacy)
  - [Version Mismatch](#version-mismatch-legacy)
- [Assembly Loading Errors](#assembly-loading-errors)
- [Troubleshooting Guide](#troubleshooting-guide)

## Overview

Syncfusion displays licensing error popups under various circumstances. This reference provides comprehensive solutions for all common licensing errors in Windows Forms applications.

**Error categories:**
- License not registered or trial expired
- Invalid license keys
- Platform and version mismatches
- Assembly loading issues

## Common Licensing Errors (v20.4.0.38+)

These errors apply to Syncfusion Essential Studio version 20.4.0.38 and later.

### License Key Not Registered / Trial Expired

#### Error Message

```
This application was built using a trial version of Syncfusion Essential Studio. 
You should include the valid license key to remove the license validation message permanently.
```

#### When This Occurs

- Syncfusion license key has not been registered in your application
- Trial key has expired after 30 days
- License key registration code is not executing
- License key is registered after control initialization

#### Error Dialog

The error appears as a popup dialog with:
- Error message explaining the issue
- **"Claim License"** button to generate a license key
- Option to continue using trial version

#### Solutions

**Solution 1: Generate and Register License Key**

1. **Generate a valid license key:**
   - Licensed users: Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Trial users: Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
   - Or click **"Claim License"** button in the error dialog

2. **Register the license key in your application:**

   **C#:**
   ```csharp
   static void Main()
   {
       // Register license key before Application.Run()
       Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
       
       Application.EnableVisualStyles();
       Application.SetCompatibleTextRenderingDefault(false);
       Application.Run(new Form1());
   }
   ```

   **VB.NET (Application.Designer.vb):**
   ```vb
   Public Sub New()
       MyBase.New(Global.Microsoft.VisualBasic.ApplicationServices.AuthenticationMode.Windows)
       
       'Register Syncfusion License
       Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY")
       
       Me.IsSingleInstance = False
       Me.EnableVisualStyles = True
   End Sub
   ```

**Solution 2: Verify Registration Timing**

Ensure license registration happens **before** any Syncfusion control initialization:

```csharp
// ✅ CORRECT: Registration before Application.Run()
static void Main()
{
    SyncfusionLicenseProvider.RegisterLicense("LICENSE_KEY"); // ← Register here
    Application.Run(new Form1());
}

// ❌ WRONG: Registration in form constructor (too late)
public Form1()
{
    InitializeComponent(); // Controls initialized here
    SyncfusionLicenseProvider.RegisterLicense("LICENSE_KEY"); // ← Too late!
}
```

**Solution 3: Check License Key Validity**

- Verify license key is complete (not truncated)
- Ensure license key is between double quotes
- Check that no extra spaces or line breaks are in the key

### Invalid Key

#### Error Message

```
The included Syncfusion license key is invalid.
```

#### When This Occurs

- License key is for a different version of Syncfusion
- License key is for a different platform (e.g., WPF key in Windows Forms app)
- License key is corrupted or incomplete
- License key format is incorrect

#### Error Dialog

The error appears as a popup with:
- "Invalid key" message
- **"Claim License"** button to generate correct key
- Continue option

#### Solutions

**Solution 1: Verify Version Match**

Ensure license key version matches your referenced assembly version.

```csharp
// Check your assembly version
// If you're using Syncfusion v26.2.4, generate a v26.2.4 license key
```

**Steps:**
1. Check NuGet package version or assembly version in References
2. Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
3. Generate license key for exact version
4. Update registration code with new key

**Solution 2: Verify Platform Match**

Ensure license key is for Windows Forms platform.

❌ **Wrong platform:**
```csharp
// Using WPF or Blazor key in Windows Forms app - will fail!
SyncfusionLicenseProvider.RegisterLicense("wpf-platform-license-key");
```

✅ **Correct platform:**
```csharp
// Using Windows Forms license key
SyncfusionLicenseProvider.RegisterLicense("windowsforms-platform-license-key");
```

**Steps:**
1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Select **"Essential Studio for Windows Forms"** (not WPF, Blazor, etc.)
3. Generate key for Windows Forms platform
4. Update registration code

**Solution 3: Verify Key Integrity**

Check that license key is complete and properly formatted:

```csharp
// ❌ WRONG: Key split across lines or truncated
SyncfusionLicenseProvider.RegisterLicense("Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p
/VWJYdVt5flBPcDwsT3RfQF5j...");

// ✅ CORRECT: Key on single line, complete
SyncfusionLicenseProvider.RegisterLicense("Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p/VWJYdVt5flBPcDwsT3RfQF5jS39TdkNnWHxedXRTRA==");
```

## Legacy Licensing Errors (v16.2.0 - v20.3.0)

These errors apply to Syncfusion versions 16.2.0.* through 20.3.0.*.

### License Key Not Registered (Legacy)

#### Error Message

```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this message 
during the evaluation period. Please refer to this help topic for more information.
```

#### Solutions

Same solutions as modern version:
1. Generate valid license key
2. Register in application before control initialization
3. Verify registration timing

### Invalid Key (Legacy)

#### Error Message

```
The included Syncfusion license is invalid. Please refer to this help topic for more information.
```

#### Solutions

1. Generate license key for correct version and platform
2. Verify key integrity
3. Update registration code

### Trial Expired (Legacy)

#### Error Message

```
Your Syncfusion trial license has expired. Please refer to this help topic for more information.
```

#### When This Occurs

- Trial license key has expired (30 days after generation)
- Continuing to use trial after expiration period

#### Solutions

**Solution: Purchase License**

1. Purchase a license from [Syncfusion Sales](https://www.syncfusion.com/sales/teamlicense)
2. Generate a paid license key from [License & Downloads](https://www.syncfusion.com/account/downloads)
3. Replace trial key with paid key in your application
4. Rebuild and test

```csharp
// Replace trial key with paid license key
// SyncfusionLicenseProvider.RegisterLicense("TRIAL_KEY"); // ← Old trial key
SyncfusionLicenseProvider.RegisterLicense("PAID_LICENSE_KEY"); // ← New paid key
```

### Platform Mismatch (Legacy)

#### Error Message

```
The included Syncfusion license is invalid (Platform mismatch). 
Please refer to this help topic for more information.
```

#### When This Occurs

- License key is for a different platform (WPF, Blazor, ASP.NET, etc.)
- Using generic Essential Studio key instead of Windows Forms key

#### Solutions

**Solution: Generate Windows Forms License Key**

1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads) or [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
2. Select **"Essential Studio for Windows Forms"** specifically
3. Generate key for Windows Forms platform
4. Update application with correct key

```csharp
// Generate and use Windows Forms specific license key
SyncfusionLicenseProvider.RegisterLicense("WINDOWS_FORMS_LICENSE_KEY");
```

### Version Mismatch (Legacy)

#### Error Message

```
The included Syncfusion license ({Registered Version}) is invalid for version {Required version}. 
Please refer to this help topic for more information.
```

Example:
```
The included Syncfusion license (26.1.35) is invalid for version 26.2.4.
```

#### When This Occurs

- License key version doesn't match referenced assembly version
- Upgraded Syncfusion components but didn't update license key
- Using old license key with newer assemblies

#### Solutions

**Solution 1: Generate License Key for Current Version**

1. Check your assembly/NuGet package version
2. Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
3. Generate license key for matching version
4. Update registration code

**Example:**

If using Syncfusion v26.2.4:
```csharp
// ❌ WRONG: Using v26.1.35 key with v26.2.4 assemblies
SyncfusionLicenseProvider.RegisterLicense("v26.1.35-license-key");

// ✅ CORRECT: Using v26.2.4 key with v26.2.4 assemblies
SyncfusionLicenseProvider.RegisterLicense("v26.2.4-license-key");
```

**Solution 2: Downgrade Assemblies to Match License**

If you need to use an older version:

1. Uninstall current NuGet packages
2. Install NuGet packages matching your license version
3. Rebuild application

## Assembly Loading Errors

### Error: "Could not load Syncfusion.Licensing.dll assembly version..."

#### When This Occurs

- `Syncfusion.Licensing.dll` is not referenced properly
- `Syncfusion.Licensing.dll` is not copied to output directory
- Assembly version mismatch
- NuGet packages not restored properly

#### Solutions

**Solution 1: Verify Copy Local Setting**

1. In Solution Explorer, expand References
2. Find `Syncfusion.Licensing`
3. Right-click → Properties
4. Set **Copy Local = True**

**Why this matters:**
- Copy Local determines if reference is copied to output path
- Must be True for Syncfusion.Licensing.dll to be available at runtime

**Solution 2: Verify DLL in Output Folder**

Check that `Syncfusion.Licensing.dll` exists in your output folder:

```
YourProject/bin/Debug/Syncfusion.Licensing.dll
YourProject/bin/Release/Syncfusion.Licensing.dll
```

If missing:
1. Set Copy Local = True
2. Clean solution
3. Rebuild solution
4. Verify DLL appears in output folder

**Solution 3: Ensure All Syncfusion Packages Are Same Version**

Check that all Syncfusion NuGet packages are the same version:

```
❌ WRONG: Mixed versions
Syncfusion.Grid.Windows v26.2.4
Syncfusion.Licensing v26.1.35  ← Different version!
Syncfusion.Shared.Base v26.2.4

✅ CORRECT: All same version
Syncfusion.Grid.Windows v26.2.4
Syncfusion.Licensing v26.2.4  ← Same version
Syncfusion.Shared.Base v26.2.4
```

**To fix:**
1. In Solution Explorer, right-click solution → Manage NuGet Packages for Solution
2. Select Updates tab
3. Update all Syncfusion packages to same version
4. Rebuild

**Solution 4: Clean and Rebuild**

1. Close Visual Studio
2. Delete `bin` and `obj` folders
3. Reopen Visual Studio
4. Restore NuGet packages
5. Rebuild solution

**Solution 5: Verify csproj and packages.config References**

Check that references in project files are correct:

**In .csproj:**
```xml
<Reference Include="Syncfusion.Licensing, Version=26.2.4.0, Culture=neutral, PublicKeyToken=632609b4d040f6b4">
  <HintPath>..\packages\Syncfusion.Licensing.26.2.4\lib\net46\Syncfusion.Licensing.dll</HintPath>
  <Private>True</Private> <!-- Copy Local = True -->
</Reference>
```

**In packages.config:**
```xml
<package id="Syncfusion.Licensing" version="26.2.4" targetFramework="net48" />
```

Ensure versions match across all files.

## Troubleshooting Guide

### Step-by-Step Diagnostic Process

When encountering licensing errors, follow these steps:

#### Step 1: Identify the Specific Error

Read the error message carefully to determine:
- Is it "not registered" or "invalid key"?
- Is it platform or version mismatch?
- Is it an assembly loading issue?

#### Step 2: Verify License Key Basics

```csharp
// Check these basics:
// 1. Is key between double quotes?
SyncfusionLicenseProvider.RegisterLicense("key"); // ✅

// 2. Is key complete (not truncated)?
// 3. Are there extra spaces or line breaks? Remove them
// 4. Is registration before Application.Run()? Must be
```

#### Step 3: Verify Version Match

1. Check your Syncfusion assembly version:
   - Solution Explorer → References → Right-click Syncfusion.Grid.Windows → Properties → Version
   
2. Check your license key version:
   - Should match assembly version exactly

3. If mismatch:
   - Generate new license key for correct version
   - Or downgrade/upgrade assemblies to match key

#### Step 4: Verify Platform Match

1. Confirm you're using Windows Forms license key
2. License key should be generated from "Essential Studio for Windows Forms"
3. Not WPF, Blazor, ASP.NET, or other platforms

#### Step 5: Verify Registration Timing

```csharp
// ✅ CORRECT order:
static void Main()
{
    // 1. Register license FIRST
    SyncfusionLicenseProvider.RegisterLicense("key");
    
    // 2. Then Application.Run
    Application.Run(new Form1());
}
```

#### Step 6: Verify Syncfusion.Licensing.dll

1. Is it referenced in project?
2. Is Copy Local = True?
3. Is it in bin/Debug or bin/Release folder?
4. Is version consistent with other Syncfusion packages?

#### Step 7: Test with Fresh License Key

1. Go to License & Downloads
2. Generate brand new license key
3. Copy entire key carefully
4. Replace old key in code
5. Rebuild and test

### Common Mistakes Checklist

- [ ] Forgot to register license key
- [ ] Registered key in wrong location (form constructor instead of Main)
- [ ] Used license key for wrong version
- [ ] Used license key for wrong platform
- [ ] License key truncated or has line breaks
- [ ] Syncfusion.Licensing.dll not referenced
- [ ] Copy Local = False for Syncfusion.Licensing.dll
- [ ] Mixed Syncfusion package versions
- [ ] Trial license expired

### Quick Fixes

**Quick Fix 1: Complete Re-registration**
```csharp
// Generate fresh license key, then:
static void Main()
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("FRESH_LICENSE_KEY");
    Application.Run(new Form1());
}
```

**Quick Fix 2: Clean Rebuild**
1. Close Visual Studio
2. Delete bin and obj folders
3. Reopen, restore NuGet packages
4. Rebuild

**Quick Fix 3: Update All Syncfusion Packages**
1. Manage NuGet Packages for Solution
2. Update all Syncfusion packages to latest/same version
3. Generate new license key for that version
4. Register new key

## Summary

**Key Troubleshooting Steps:**

1. **Identify error type** (not registered, invalid, platform/version mismatch, assembly error)
2. **Verify license key** (correct version, platform, complete, no truncation)
3. **Verify registration location** (in Main, before Application.Run)
4. **Check dependencies** (Syncfusion.Licensing.dll referenced, Copy Local = True)
5. **Ensure version consistency** (all Syncfusion packages same version)
6. **Test with fresh key** (generate new key if unsure)

**Most Common Solutions:**

- Generate correct license key for version and platform
- Register in Main() before Application.Run()
- Set Copy Local = True for Syncfusion.Licensing.dll
- Update all Syncfusion packages to same version

## Next Steps

- For license key generation, read [license-generation.md](license-generation.md)
- For license registration guidance, read [license-registration.md](license-registration.md)
- For CI/CD validation, read [licensing-faqs.md](licensing-faqs.md)
