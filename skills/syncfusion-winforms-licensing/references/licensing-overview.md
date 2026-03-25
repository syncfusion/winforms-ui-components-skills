# Syncfusion Licensing Overview

This reference provides a comprehensive overview of the Syncfusion licensing system for Windows Forms applications, including when license registration is required and how it differs from unlock keys.

## Introduction to Syncfusion Licensing

Starting with version **16.2.0.x** of Essential Studio, Syncfusion introduced a new licensing system. These changes apply to:

- All evaluators using trial installers
- Paid customers who use NuGet packages from [nuget.org](https://www.nuget.org/)

If you use the evaluation installer or NuGet feed to reference Syncfusion assemblies, you must include the corresponding **platform and version-specific license key** in your projects.

## Difference Between Unlock Key and License Key

**Important:** The license key is different from the installer unlock key.

- **Unlock Key:** Used to unlock and install Syncfusion software on your development machine
- **License Key:** A string that must be registered in your application code to enable Syncfusion components

The license key must be separately generated from the Syncfusion website and registered in your application before any Syncfusion control is initialized.

## When License Registration is Required

License registration requirements depend on how you obtained Syncfusion assemblies:

### Scenarios Requiring License Registration

1. **NuGet Packages from nuget.org**
   - If you reference Syncfusion components via NuGet packages from nuget.org
   - You must register a license key in your application
   - No need to install Syncfusion software

2. **Trial Installer**
   - If you installed Syncfusion using the trial installer
   - You must register a trial license key
   - Trial keys expire after 30 days

### Scenarios NOT Requiring License Registration

1. **Licensed Installer**
   - If you installed Syncfusion using the licensed installer (not trial)
   - License key registration is NOT required
   - Components are pre-licensed during installation

## License Validation Error Message

If you don't register a license key when required, you'll see this error:

```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license key to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this message 
during the evaluation period.
```

This message appears when:
- License key is not registered in your application
- Trial key has expired after 30 days
- License key is invalid or for wrong version/platform

## Build Server and CI/CD Licensing

Understanding license requirements for build servers and continuous integration is critical:

### Build Server Scenarios

| Assembly Source | License Registration Required? | Details |
|----------------|-------------------------------|---------|
| **NuGet packages from nuget.org** | ✅ **Yes** | No Syncfusion installer needed on build server. Download NuGet packages directly. Register license key in application. Use any developer license to generate keys for build environments. |
| **Trial Installer** | ✅ **Yes** | If using assemblies from trial installer on build server, register license key for corresponding version and platform to avoid trial warning. Use any developer trial license to generate keys. |
| **Licensed Installer** | ❌ **No** | If using assemblies from licensed installer on build server, no license key registration needed. Download and install licensed version on build server. |

### Key Points for Build Servers

1. **NuGet-based builds:** Most common approach, requires license registration
2. **Developer licenses work for CI:** Use any developer license to generate keys for automated builds
3. **Internet not required:** License validation happens offline during build
4. **Version matching critical:** License key version must match package/assembly version

## Version and Platform Specificity

**Critical Rule:** Syncfusion license keys are:

1. **Version-specific:** License key for version 26.2.4 will NOT work for version 26.1.35
2. **Platform-specific:** Windows Forms license key will NOT work for WPF, Blazor, etc.

### Examples

❌ **Incorrect:**
```csharp
// Using v26.2.4 license key with v26.1.35 assemblies - will fail
SyncfusionLicenseProvider.RegisterLicense("v26.2.4-license-key");
// But project references Syncfusion.Grid.Windows v26.1.35
```

✅ **Correct:**
```csharp
// License key version matches assembly version
SyncfusionLicenseProvider.RegisterLicense("v26.1.35-license-key");
// Project references Syncfusion.Grid.Windows v26.1.35
```

❌ **Incorrect:**
```csharp
// Using WPF license key in Windows Forms application - will fail
SyncfusionLicenseProvider.RegisterLicense("wpf-platform-license-key");
```

✅ **Correct:**
```csharp
// Using Windows Forms license key in Windows Forms application
SyncfusionLicenseProvider.RegisterLicense("windowsforms-platform-license-key");
```

## Offline License Validation

**Important:** Syncfusion license validation happens **offline** during application execution.

Key characteristics:
- **No internet required:** Applications do not need internet access for license validation
- **Deployment friendly:** Apps registered with license keys can be deployed to systems without internet
- **Runtime validation:** Validation occurs when application starts, not during compilation
- **One-time registration:** Register license key once at application entry point

## License Key Format

License keys are long alphanumeric strings that look like this:

```
Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p/VWJYdVt5flBPcDwsT3RfQF5jS39TdkNnWHxedXRTRA==
```

The license key:
- Is a base64-encoded string
- Contains version and platform information
- Is generated from your Syncfusion account
- Should be kept between double quotes when registered

## Summary

**Key Takeaways:**

1. License registration is required when using:
   - NuGet packages from nuget.org
   - Trial installers

2. License registration is NOT required when using:
   - Licensed installers

3. License keys are:
   - Version-specific
   - Platform-specific
   - Different from unlock keys
   - Validated offline (no internet needed)

4. Build servers and CI/CD:
   - Follow same rules as development machines
   - Use developer license to generate keys for builds
   - Consider using NuGet packages for easier CI integration

5. Always register license key:
   - Before any Syncfusion control initialization
   - At application entry point
   - With matching version and platform

## Next Steps

- To generate a license key, read [license-generation.md](license-generation.md)
- To register a license key in your application, read [license-registration.md](license-registration.md)
- For troubleshooting license errors, read [licensing-errors.md](licensing-errors.md)
