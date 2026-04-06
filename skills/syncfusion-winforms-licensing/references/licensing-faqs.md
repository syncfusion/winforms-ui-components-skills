# Syncfusion Licensing FAQs and CI/CD Integration

## Table of Contents
- [CI/CD License Validation](#cicd-license-validation)
  - [Overview](#overview)
  - [LicenseKeyValidator Utility Setup](#licensekeyvalidator-utility-setup)
  - [Azure Pipelines (YAML)](#azure-pipelines-yaml)
  - [Azure Pipelines (Classic)](#azure-pipelines-classic)
  - [GitHub Actions](#github-actions)
  - [Jenkins](#jenkins)
  - [ValidateLicense Method](#validatelicense-method)
  - [Unit Test Project Validation](#unit-test-project-validation)
- [Where Can I Get a License Key?](#where-can-i-get-a-license-key)
- [Is Internet Connection Required?](#is-internet-connection-required)
- [How to Upgrade from Trial Version?](#how-to-upgrade-from-trial-version)
- [Registering Syncfusion Account for NuGet.org Users](#registering-syncfusion-account-for-nugetorg-users)
- [License Key Specificity](#license-key-specificity)

## CI/CD License Validation

### Overview

Syncfusion license key validation in CI/CD services ensures that components are properly licensed during continuous integration processes. Validating the license key at the CI level prevents licensing errors during deployment.

**Benefits of CI License Validation:**
- Catch licensing issues early in the build pipeline
- Prevent deployment of applications with invalid licenses
- Validate license keys before reaching production
- Ensure all build servers use correct license keys

**Supported CI/CD Platforms:**
- Azure Pipelines (YAML and Classic)
- GitHub Actions
- Jenkins
- Any platform supporting PowerShell scripts

**Minimum Version:** This feature is supported from Essential Studio version 16.2.0.41 and later.

### LicenseKeyValidator Utility Setup

The LicenseKeyValidator utility validates Syncfusion license keys outside of your application, making it ideal for CI/CD integration.

#### Recommended Approach: Use ValidateLicense Method

**⚠️ SECURITY RECOMMENDATION:** Instead of downloading and executing external binaries, use the built-in `ValidateLicense()` method from the Syncfusion.Licensing assembly already referenced in your project. This approach is more secure and doesn't require external dependencies.

See the [ValidateLicense Method](#validatelicense-method) section below for implementation details.

#### Alternative: LicenseKeyValidator Utility (Advanced Users Only)

**⚠️ SECURITY WARNING:** The following approach downloads and executes code from an external source. Only use this if:
- You cannot use the ValidateLicense method approach
- You fully understand the security implications
- You have verified the source authenticity
- Your organization's security policy permits external executable downloads

**Security Best Practices:**
1. Download the utility once and commit it to your source repository after verification
2. Verify file integrity using checksums before execution
3. Scan the downloaded files with antivirus software
4. Review your organization's security policies before proceeding

##### Manual Download Steps

1. **Verify Source:** Ensure you're downloading from the official Syncfusion domain
2. **Download Location:** The utility is available from Syncfusion's official distribution:
   - Contact Syncfusion support for the official download link
   - Or check your Syncfusion account downloads section
3. **Verify Integrity:** After download, verify file checksums if provided
4. **Scan Files:** Run antivirus/malware scan before extraction
5. **Extract** the ZIP file to a known location (e.g., `D:\LicenseKeyValidator\` or `/home/user/LicenseKeyValidator/`)

##### Contents

The extracted folder typically contains:
- `LicenseKeyValidatorConsole.exe` - Console application for validation
- `LicenseKeyValidation.ps1` - PowerShell script template
- Supporting DLLs and configuration files

#### Configure PowerShell Script

**Secure PowerShell Script Template:**

```powershell
# SECURITY BEST PRACTICE: Read license key from environment variable
$licenseKey = $env:SYNCFUSION_LICENSE_KEY

if ([string]::IsNullOrEmpty($licenseKey)) {
    Write-Error "SYNCFUSION_LICENSE_KEY environment variable is not set"
    exit 1
}

# Configure platform and version (these can be in source control)
$platform = "WindowsForms"
$version = "26.2.4"

# Execute validation
$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" /platform:$platform /version:$version /licensekey:$licenseKey

Write-Host $result

# Exit with appropriate code
if ($result -match "valid") {
    exit 0
} else {
    exit 1
}
```

**Parameters:**

| Parameter | Description | Example | Can Commit? |
|-----------|-------------|---------|-------------|
| `/platform:` | Target platform | `"WindowsForms"` | ✅ Yes |
| `/version:` | Syncfusion version | `"26.2.4"` | ✅ Yes |
| `/licensekey:` | License key from environment | `$env:SYNCFUSION_LICENSE_KEY` | ⚠️ Use env var only |

**How to Set Environment Variable:**

```powershell
# Windows PowerShell (current session)
$env:SYNCFUSION_LICENSE_KEY = "your-license-key"

# Windows (permanent - user level)
[System.Environment]::SetEnvironmentVariable("SYNCFUSION_LICENSE_KEY", "your-license-key", "User")
```

### Azure Pipelines (YAML)

Integrate license validation in Azure Pipelines using YAML configuration.

#### Setup

**Step 1: Create User-Defined Variable**

1. In Azure DevOps, navigate to your pipeline
2. Click **Variables**
3. Add new variable:
   - **Name:** `LICENSE_VALIDATION`
   - **Value:** Path to LicenseKeyValidation.ps1 script (e.g., `D:\LicenseKeyValidator\LicenseKeyValidation.ps1`)

**Step 2: Add PowerShell Task to Pipeline**

**azure-pipelines.yml:**
```yaml
pool:
  vmImage: 'windows-latest'

steps:

- task: PowerShell@2
  inputs:
    targetType: filePath
    filePath: $(LICENSE_VALIDATION) # Or the actual path to the LicenseKeyValidation.ps1 script
  
  displayName: Syncfusion License Validation
```

#### Complete Example with Build

```yaml
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'

steps:

# Validate Syncfusion license before building
- task: PowerShell@2
  inputs:
    targetType: filePath
    filePath: $(LICENSE_VALIDATION)
  displayName: 'Syncfusion License Validation'

# Restore NuGet packages
- task: NuGetToolInstaller@1

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

# Build solution
- task: VSBuild@1
  inputs:
    solution: '$(solution)'
    platform: '$(buildPlatform)'
    configuration: '$(buildConfiguration)'
```

**How it Works:**
1. Pipeline runs on Windows agent
2. PowerShell task executes LicenseKeyValidation.ps1
3. Script validates license key
4. If validation fails, pipeline stops
5. If validation succeeds, build continues

### Azure Pipelines (Classic)

Integrate license validation using Azure Pipelines Classic Editor.

#### Setup Steps

**Step 1: Create User-Defined Variable**

1. Edit your pipeline in Classic Editor
2. Go to **Variables** tab
3. Add variable:
   - **Name:** `LICENSE_VALIDATION`
   - **Value:** `D:\LicenseKeyValidator\LicenseKeyValidation.ps1`

**Step 2: Add PowerShell Task**

1. Click **"+"** to add a task
2. Search for **"PowerShell"**
3. Add **PowerShell** task
4. Configure:
   - **Type:** File Path
   - **Script Path:** `$(LICENSE_VALIDATION)`
   - **Display name:** Syncfusion License Validation

**Step 3: Position Task**

Place the PowerShell task **before** your build tasks to validate license before building.

#### Task Configuration

**PowerShell Task Settings:**
- **Display name:** Syncfusion License Validation
- **Type:** File Path
- **Script Path:** `$(LICENSE_VALIDATION)`
- **Arguments:** (leave empty, parameters are in script)
- **Working Directory:** (optional)

### GitHub Actions

Integrate license validation in GitHub Actions workflows.

#### Setup

**Step 1: Add Script to Repository**

Option A: Add LicenseKeyValidator folder to repository:
```
YourRepo/
├── .github/
│   └── workflows/
│       └── build.yml
├── LicenseKeyValidator/
│   ├── LicenseKeyValidatorConsole.exe
│   └── LicenseKeyValidation.ps1
└── src/
```

Option B: Download during workflow (see example below)

**Step 2: Update Workflow YAML**

**.github/workflows/build.yml:**
```yaml
name: Build and Validate

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: windows-latest

    steps:
    - uses: actions/checkout@v3
    
    # Validate Syncfusion License
    - name: Syncfusion License Validation
      shell: pwsh
      run: |
        ./LicenseKeyValidator/LicenseKeyValidation.ps1
    
    # Setup .NET
    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x
    
    # Restore dependencies
    - name: Restore dependencies
      run: dotnet restore
    
    # Build
    - name: Build
      run: dotnet build --no-restore --configuration Release
```

#### Using GitHub Secrets for License Keys

**Better approach:** Use GitHub Secrets to store license key:

**Step 1: Add Secret**
1. Go to repository Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `SYNCFUSION_LICENSE_KEY`
4. Value: Your license key
5. Save

**Step 2: Update Script to Use Secret**

**Modified LicenseKeyValidation.ps1:**
```powershell
# Get license key from environment variable (set by GitHub Actions)
$licenseKey = $env:SYNCFUSION_LICENSE_KEY

if ([string]::IsNullOrEmpty($licenseKey)) {
    Write-Error "License key not found in environment"
    exit 1
}

$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" /platform:"WindowsForms" /version:"26.2.4" /licensekey:$licenseKey

Write-Host $result
```

**Step 3: Pass Secret to Workflow**

```yaml
- name: Syncfusion License Validation
  shell: pwsh
  env:
    SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
  run: |
    ./LicenseKeyValidator/LicenseKeyValidation.ps1
```

### Jenkins

Integrate license validation in Jenkins pipelines.

#### Setup

**Step 1: Create Environment Variable**

1. In Jenkins, go to pipeline configuration
2. Add environment variable:
   - **Name:** `LICENSE_VALIDATION`
   - **Value:** Path to script (e.g., `D:\LicenseKeyValidator\LicenseKeyValidation.ps1` or `/var/jenkins/LicenseKeyValidator/LicenseKeyValidation.ps1`)

**Step 2: Add Stage to Jenkinsfile**

**Jenkinsfile (Declarative Pipeline):**
```groovy
pipeline {
    agent any
    
    environment {
        LICENSE_VALIDATION = 'D:\\LicenseKeyValidator\\LicenseKeyValidation.ps1'
    }
    
    stages {
        stage('Syncfusion License Validation') {
            steps {
                // For Windows agents
                bat "powershell -File ${LICENSE_VALIDATION}"
                
                // For Linux/Mac agents with PowerShell Core
                // sh "pwsh ${LICENSE_VALIDATION}"
            }
        }
        
        stage('Build') {
            steps {
                // Your build steps
                bat 'dotnet build'
            }
        }
        
        stage('Test') {
            steps {
                // Your test steps
                bat 'dotnet test'
            }
        }
    }
}
```

#### Using Jenkins Credentials

**Better approach:** Store license key in Jenkins credentials:

**Step 1: Add Credential**
1. Jenkins → Manage Jenkins → Manage Credentials
2. Add Secret Text credential
3. ID: `syncfusion-license-key`
4. Secret: Your license key

**Step 2: Use in Pipeline**

```groovy
pipeline {
    agent any
    
    stages {
        stage('Syncfusion License Validation') {
            steps {
                withCredentials([string(credentialsId: 'syncfusion-license-key', variable: 'LICENSE_KEY')]) {
                    powershell """
                        \$result = & "D:\\LicenseKeyValidator\\LicenseKeyValidatorConsole.exe" /platform:"WindowsForms" /version:"26.2.4" /licensekey:\$env:LICENSE_KEY
                        Write-Host \$result
                    """
                }
            }
        }
    }
}
```

### ValidateLicense Method

**✅ RECOMMENDED APPROACH:** For programmatic validation within your application, use the `ValidateLicense()` method. This is the most secure method as it uses assemblies already in your project without external dependencies.

#### Basic Usage (Secure)

```csharp
using System;
using Syncfusion.Licensing;

// SECURITY BEST PRACTICE: Read license key from environment variable
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");

if (string.IsNullOrEmpty(licenseKey))
{
    Console.WriteLine("License key not found in environment variables");
    return;
}

// Register license key
SyncfusionLicenseProvider.RegisterLicense(licenseKey);

// Validate the registered license key
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WindowsForms);

if (isValid)
{
    Console.WriteLine("License is valid");
}
else
{
    Console.WriteLine("License is invalid");
    // Handle invalid license
}
```

#### With Validation Message (Secure)

```csharp
using System;
using Syncfusion.Licensing;

// SECURITY BEST PRACTICE: Read from environment variable
string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");

if (string.IsNullOrEmpty(licenseKey))
{
    throw new InvalidOperationException("SYNCFUSION_LICENSE_KEY environment variable not set");
}

// Register license key
SyncfusionLicenseProvider.RegisterLicense(licenseKey);

// Validate with detailed message
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WindowsForms, out string validationMessage);

if (isValid)
{
    Console.WriteLine($"License is valid: {validationMessage}");
}
else
{
    Console.WriteLine($"License validation failed: {validationMessage}");
    throw new Exception($"Invalid license: {validationMessage}");
}
```

#### Use in Application Startup (Secure)

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Licensing;

static void Main()
{
    // SECURITY BEST PRACTICE: Read from environment variable
    string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    
    if (string.IsNullOrEmpty(licenseKey))
    {
        MessageBox.Show("License key not configured. Set SYNCFUSION_LICENSE_KEY environment variable.", 
            "Configuration Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
        return;
    }
    
    // Register license
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    
    // Validate before proceeding
    bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WindowsForms, out string message);
    
    if (!isValid)
    {
        MessageBox.Show($"License validation failed: {message}", "Licensing Error", 
            MessageBoxButtons.OK, MessageBoxIcon.Error);
        return; // Exit application
    }
    
    // Continue with normal application flow
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);
    Application.Run(new Form1());
}
```

### Unit Test Project Validation

Create unit tests to validate license keys, useful for CI/CD integration.

#### Create Unit Test Project

**In Visual Studio:**
1. File → New → Project
2. Search for "Test"
3. Select test framework (MSTest, NUnit, or xUnit)
4. Name: `LicensingTests`
5. Create

#### Example: MSTest (Secure)

```csharp
using System;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Syncfusion.Licensing;

namespace LicensingTests
{
    [TestClass]
    public class SyncfusionLicenseTests
    {
        [TestMethod]
        public void TestSyncfusionWindowsFormsLicense()
        {
            // Arrange
            var platform = Platform.WindowsForms;
            
            // SECURITY BEST PRACTICE: Read from environment variable
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            
            Assert.IsNotNull(licenseKey, "SYNCFUSION_LICENSE_KEY environment variable not set");
            Assert.IsFalse(string.IsNullOrWhiteSpace(licenseKey), "SYNCFUSION_LICENSE_KEY is empty");
            
            // Act
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(platform, out string validationMessage);
            
            // Assert
            Assert.IsTrue(isValidLicense, $"Validation failed for {platform}. Message: {validationMessage}");
            
            // Log success
            TestContext.WriteLine($"Platform {platform} is correctly licensed");
        }
        
        public TestContext TestContext { get; set; }
    }
}
```

#### Example: NUnit

```csharp
using NUnit.Framework;
using Syncfusion.Licensing;

namespace LicensingTests
{
    [TestFixture]
    public class SyncfusionLicenseTests
    {
        [Test]
        public void TestSyncfusionWindowsFormsLicense()
        {
            // Arrange
            var platform = Platform.WindowsForms;
            string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            
            Assert.That(licenseKey, Is.Not.Null.And.Not.Empty, "License key not found in environment");
            
            // Act
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(platform, out string validationMessage);
            
            // Assert
            Assert.That(isValidLicense, Is.True, $"Validation failed for {platform}. Message: {validationMessage}");
            
            // Log success
            TestContext.Out.WriteLine($"Platform {platform} is correctly licensed for version {typeof(SyncfusionLicenseProvider).Assembly.GetName().Version}");
        }
    }
}
```

#### Run in CI/CD

**Azure Pipelines:**
```yaml
- task: VSTest@2
  inputs:
    testSelector: 'testAssemblies'
    testAssemblyVer2: '**/LicensingTests.dll'
    searchFolder: '$(System.DefaultWorkingDirectory)'
```

**GitHub Actions:**
```yaml
- name: Run License Validation Tests
  run: dotnet test LicensingTests/LicensingTests.csproj --logger "console;verbosity=detailed"
```

#### Expected Output

**Success:**
```
Test Passed - TestSyncfusionWindowsFormsLicense
Platform WindowsForms is correctly licensed for version 26.2.4.0
```

**Failure:**
```
Test Failed - TestSyncfusionWindowsFormsLicense
Validation failed for WindowsForms. Message: Invalid license key
```

## Where Can I Get a License Key?

License keys can be generated from your Syncfusion account in two locations:

**For Paid Licenses:**
- **[License & Downloads](https://syncfusion.com/account/downloads)** section

**For Trial Licenses:**
- **[Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)** section

### Important Notes

⚠️ **Version and Platform Specificity:**
- License keys are **version-specific** - a key for v26.2.4 won't work for v26.1.35
- License keys are **platform-specific** - Windows Forms keys only work for Windows Forms

📚 **Additional Resources:**
- [How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products)
- [Which version license key should I use?](https://support.syncfusion.com/kb/article/7865/which-version-syncfusion-license-key-should-i-use-in-my-application)

## Is Internet Connection Required?

**No, internet connection is NOT required for Syncfusion license validation.**

### Key Points

✅ **Offline Validation:**
- Syncfusion license validation is done **offline** during application execution
- No internet access required at runtime
- Validation happens locally using embedded assembly metadata

✅ **Deployment:**
- Apps registered with a Syncfusion license key can be deployed on any system
- Systems do not need internet connectivity
- License validation works in air-gapped environments

✅ **When Internet IS Needed:**
- Generating license keys (requires access to Syncfusion website)
- Downloading Syncfusion installers or NuGet packages
- Accessing documentation and support

### Example Scenarios

**Scenario 1: Offline Development Machine**
```
✅ Can develop with Syncfusion if license key already obtained
✅ License validation works without internet
❌ Cannot generate new license keys without internet
❌ Cannot download new Syncfusion packages without internet
```

**Scenario 2: Production Server Without Internet**
```
✅ Application with registered license key works fine
✅ No licensing errors or popups
✅ Full Syncfusion functionality available
```

## How to Upgrade from Trial Version?

After purchasing a license, there are two ways to upgrade from trial version:

### Option 1: Reinstall with Licensed Installer (Recommended)

**Steps:**
1. Uninstall the trial version
2. Log in to Syncfusion account
3. Go to **[License & Downloads](https://www.syncfusion.com/account/downloads)**
4. Download licensed installer
5. Install licensed version
6. No license key registration needed (assemblies are pre-licensed)

**Pros:**
- No code changes required
- Assemblies are fully licensed
- No license registration needed

**Cons:**
- Requires uninstall/reinstall process
- More steps involved

### Option 2: Replace License Key (For NuGet Users)

**Steps:**
1. Keep existing trial installation or NuGet packages
2. Generate paid license key from **[License & Downloads](https://www.syncfusion.com/account/downloads)**
3. Update the license key in your secure configuration (environment variable or config file)
4. Rebuild and test application

**Example (Secure Approach):**
```csharp
static void Main()
{
    // SECURITY BEST PRACTICE: License key should be stored in environment variable
    // Update SYNCFUSION_LICENSE_KEY environment variable with your new paid license key
    
    string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    
    if (!string.IsNullOrEmpty(licenseKey))
    {
        SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    }
    
    Application.Run(new Form1());
}
```

**How to Update:**
```powershell
# Update environment variable with new paid license key
[System.Environment]::SetEnvironmentVariable("SYNCFUSION_LICENSE_KEY", "your-new-paid-license-key", "User")
```

**Pros:**
- Simple configuration change
- No reinstallation required
- Works well with NuGet workflow
- Secure key management

**Cons:**
- Still requires license registration in code

### Important Note

⚠️ **License registration is NOT required** if you reference Syncfusion assemblies from the **licensed installer**.

License registration is only required when:
- Using NuGet packages from nuget.org
- Using evaluation/trial installer

## Registering Syncfusion Account for NuGet.org Users

If you obtained Syncfusion assemblies directly from NuGet.org without a Syncfusion account, you need to register for an account to get a license key.

### Why This Is Needed

- Syncfusion packages on NuGet.org require license keys
- License keys are generated from Syncfusion accounts
- Even NuGet.org users need Syncfusion accounts for licensing

### Steps to Register and Get License Key

**Step 1: Register for Free Syncfusion Account**
1. Go to https://www.syncfusion.com/account/register
2. Fill out registration form
3. Verify email address
4. Account created

**Step 2: Start a Trial**
1. Log in to your new Syncfusion account
2. Go to **[Start Trials](https://syncfusion.com/account/manage-trials/start-trials)** page
3. Select "Essential Studio for Windows Forms"
4. Start 30-day trial

**Step 3: Generate License Key**
1. Go to **[Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)** section
2. Select Windows Forms platform
3. Select version matching your NuGet packages
4. Generate and copy license key

**Step 4: Register License Key in Application (Secure)**
```csharp
static void Main()
{
    // SECURITY BEST PRACTICE: Store key in environment variable
    // Set SYNCFUSION_LICENSE_KEY environment variable with your trial license key
    
    string licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    
    if (string.IsNullOrEmpty(licenseKey))
    {
        MessageBox.Show("Please set SYNCFUSION_LICENSE_KEY environment variable", 
            "Configuration Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
        return;
    }
    
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    Application.Run(new Form1());
}
```

### Trial Period

- **Duration:** 30 days from trial start
- **Features:** Full access to all Syncfusion components
- **After expiration:** Trial message appears, must purchase or uninstall

### Purchase After Trial

To continue using after trial:
1. Purchase license from https://www.syncfusion.com/sales/products
2. Generate paid license key from License & Downloads
3. Replace trial key with paid key in code

## License Key Specificity

Understanding license key specificity is crucial for avoiding licensing errors.

### Version-Specific

**Rule:** License key version must EXACTLY match assembly version.

❌ **Wrong:**
```csharp
// Using v26.2.4 license key with v26.1.35 assemblies
// (Note: License keys should be stored in environment variables, not hardcoded)
string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY"); // v26.2.4 key
SyncfusionLicenseProvider.RegisterLicense(key);
// But project references: Syncfusion.Grid.Windows v26.1.35
// Result: Version mismatch error
```

✅ **Correct:**
```csharp
// License key version matches assembly version
// Generate v26.1.35 key and store in SYNCFUSION_LICENSE_KEY environment variable
string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY"); // v26.1.35 key
SyncfusionLicenseProvider.RegisterLicense(key);
// Project references: Syncfusion.Grid.Windows v26.1.35
// Result: Works perfectly
```

### Platform-Specific

**Rule:** License key platform must match application platform.

❌ **Wrong:**
```csharp
// Using WPF license key in Windows Forms application
// (Assuming wrong platform key is stored in environment variable)
string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY"); // WPF key
SyncfusionLicenseProvider.RegisterLicense(key);
// Result: Platform mismatch error
```

✅ **Correct:**
```csharp
// Using Windows Forms license key in Windows Forms application
// Generate Windows Forms platform key and store in environment variable
string key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY"); // Windows Forms key
SyncfusionLicenseProvider.RegisterLicense(key);
// Result: Works perfectly
```

### Best Practices

1. **Document version-key mappings** for your projects
2. **Update license keys** when upgrading Syncfusion versions
3. **Verify platform** when generating keys (select "Windows Forms")
4. **Check assembly versions** in references before generating keys
5. **Test after changes** to ensure licensing works

### Helpful Knowledge Base Articles

- [How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products)
- [Which version license key should I use?](https://support.syncfusion.com/kb/article/7865/which-version-syncfusion-license-key-should-i-use-in-my-application)
- [Difference between unlock key and license key](https://support.syncfusion.com/kb/article/7863/difference-between-the-unlock-key-and-licensing-key)

## Summary

**CI/CD License Validation:**
- Use LicenseKeyValidator utility for automated validation
- Integrate in Azure Pipelines, GitHub Actions, or Jenkins
- Use ValidateLicense() method for programmatic validation
- Create unit tests for license validation

**Common Questions:**
- Generate license keys from License & Downloads or Trial & Downloads
- Internet NOT required for validation (only for key generation)
- Upgrade by reinstalling or replacing license key
- NuGet.org users need Syncfusion account for licensing
- License keys are version and platform specific

**Best Practices:**
- Store license keys in CI/CD secrets, not source control
- Validate licenses early in build pipeline
- Keep license keys and assembly versions in sync
- Document version-key mappings for team reference

## Next Steps

- For license generation, read [license-generation.md](license-generation.md)
- For license registration, read [license-registration.md](license-registration.md)
- For troubleshooting errors, read [licensing-errors.md](licensing-errors.md)
