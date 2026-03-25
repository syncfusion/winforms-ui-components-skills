# Generating Syncfusion Windows Forms License Keys

This reference explains how to generate Syncfusion license keys for Windows Forms applications, including different generation methods and license scenarios.

## Overview

License keys for Windows Forms can be generated from your Syncfusion account in two primary locations:

1. **[License & Downloads](https://syncfusion.com/account/downloads)** - For paid customers with active licenses
2. **[Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)** - For trial users

## Important Notes

⚠️ **Version and Platform Specificity:**
- Syncfusion license keys are **version-specific** (e.g., 26.2.4 vs 26.1.35)
- License keys are **platform-specific** (Windows Forms vs WPF vs Blazor)
- You must generate the license key for your exact version and platform

⚠️ **Which Version to Use:**
- Use the license key that matches your referenced assembly version
- If you upgrade Syncfusion components, regenerate license key for the new version

## Method 1: Generating from License & Downloads

For customers with active paid licenses:

### Steps

1. Log in to your Syncfusion account at https://www.syncfusion.com/
2. Navigate to **License & Downloads** section
3. Select **Essential Studio for Windows Forms**
4. Choose the specific version you need (e.g., v26.2.4)
5. Click **"Generate License"** or **"Get License Key"**
6. Copy the generated license key

### Example

```
1. Go to: https://syncfusion.com/account/downloads
2. Select: Essential Studio for Windows Forms
3. Select version: 26.2.4
4. Click: Generate License
5. Copy: Mgo+DSMBaFt/QHRqVVhkVFpFdEBBXHxAd1p/VWJYdVt5flBPcDwsT3RfQF5jS39T...
```

## Method 2: Generating from Trial & Downloads

For trial users:

### Steps

1. Log in to your Syncfusion account at https://www.syncfusion.com/
2. Navigate to **Trial & Downloads** section (or **Manage Trials**)
3. Select **Essential Studio for Windows Forms**
4. Choose the trial version you're using
5. Click **"Get License Key"** or **"Generate License"**
6. Copy the trial license key

### Trial License Characteristics

- Valid for **30 days** from generation date
- Can be used for development and testing
- Shows trial message after expiration
- Can be upgraded to paid license without code changes

## Method 3: Claim License Key

The **Claim License Key** feature provides a quick way to generate license keys based on your account status.

### Accessing Claim License Key

You can access this feature:

1. From the licensing error dialog (if you see a license error in your app)
2. Click **"Claim License"** button in the error message
3. This opens a webpage where you can claim a license key

### License Scenarios

#### Scenario 1: Active License

**Situation:** You have a Syncfusion account with a valid, active license.

**Result:**
- License key will be generated immediately
- No expiration date
- Full access to all features
- Valid for the purchased version and platform

**Action:**
1. Click "Claim License Key"
2. Select Windows Forms platform
3. Select version
4. Copy generated license key

#### Scenario 2: Active Trial

**Situation:** You have an active trial license (within 30 days of starting trial).

**Result:**
- Trial license key will be generated
- Includes expiration date (30 days from trial start)
- Full access during trial period
- Trial message appears after expiration

**Action:**
1. Click "Claim License Key"
2. Select Windows Forms platform
3. Select version
4. Copy trial license key
5. Note the expiration date

**Example:**
```
Trial License: Valid until April 24, 2026
Platform: Windows Forms
Version: 26.2.4
```

#### Scenario 3: Expired License

**Situation:** Your license subscription has expired.

**Result:**
- Cannot generate license key for latest version
- Must renew subscription to get valid key for new versions
- Temporary license key available (5-day validity) as interim solution
- Previous version keys may still work for old versions you own

**Action:**
1. Click "Claim License Key"
2. System shows expired status
3. Option to renew subscription
4. Or receive 5-day temporary license key for evaluation

**Important:** 
- Expired license keys won't work with newer versions
- You can still use keys for versions within your license period
- Renew subscription to continue receiving updates

#### Scenario 4: No License or Expired Trial

**Situation:** 
- You don't have an active license or trial
- Your trial has expired (more than 30 days ago)
- You're a new user

**Result:**
- No immediate license key available
- Must start a new trial or purchase a license
- Options presented to claim trial or purchase

**Actions Available:**

**Option A: Start a New Trial**
1. Click "Start Trial"
2. Fill out trial form if needed
3. Trial activated immediately
4. Generate trial license key (30-day validity)

**Option B: Purchase a License**
1. Click "Purchase" or "View Plans"
2. Select appropriate license
3. Complete purchase
4. Generate license key from License & Downloads

**Option C: Contact Sales**
- Request quote
- Enterprise licensing
- Volume licensing

## Generating License Keys for Multiple Versions

If you work with multiple Syncfusion versions:

### Best Practice

1. Document which license key corresponds to which version
2. Generate separate keys for each version you use
3. Store keys securely (e.g., environment variables, configuration files)
4. Use conditional registration based on assembly version if needed

### Example Organization

```
Version 26.2.4: Mgo+DSMBaFt/QHRq...
Version 26.1.35: Nho+FTNCbGu/RISr...
Version 25.2.7:  Pjp+GTODcHw/SJTt...
```

## Generating Keys for Different Platforms

If you use multiple Syncfusion platforms (Windows Forms, WPF, etc.):

### Important

- Each platform requires a separate license key
- Generate keys for each platform separately
- Keys cannot be shared across platforms

### Example

```csharp
// Windows Forms application
SyncfusionLicenseProvider.RegisterLicense("windowsforms-license-key");

// This would NOT work (WPF key in Windows Forms app):
// SyncfusionLicenseProvider.RegisterLicense("wpf-license-key"); ❌
```

## License Key Best Practices

### Security

1. **Don't hardcode in source code:** Use configuration files or environment variables
2. **Exclude from source control:** Add to .gitignore if stored in config files
3. **Use build-time injection:** Inject during CI/CD pipeline
4. **Encrypt if stored:** Consider encrypting license keys in configuration

### Organization

1. **Document version-key mappings:** Keep a record of which key is for which version
2. **Centralize storage:** Store in one location for team access
3. **Update regularly:** Regenerate keys when upgrading Syncfusion versions
4. **Test after generation:** Verify key works before deploying

## Troubleshooting License Generation

### Issue: Can't Find Generate License Button

**Solution:**
- Ensure you're logged into your Syncfusion account
- Check that your license/trial is active
- Navigate to correct section (License & Downloads or Trial & Downloads)
- Contact support if button doesn't appear

### Issue: Generated Key Shows Wrong Version

**Solution:**
- Verify you selected correct version from dropdown
- Check your referenced assembly version in project
- Regenerate key with correct version selected

### Issue: Generated Key Shows Wrong Platform

**Solution:**
- Confirm you selected "Windows Forms" as platform
- Don't use "Essential Studio" generic key
- Regenerate key with Windows Forms specifically selected

## Summary

**License Key Generation Options:**

1. **License & Downloads:** For paid license holders
2. **Trial & Downloads:** For trial users
3. **Claim License Key:** Quick generation based on account status

**Key Reminders:**

- License keys are version and platform specific
- Match license key version to your assembly version
- Trial keys expire after 30 days
- Expired licenses need renewal for latest versions
- Keep keys secure and organized

## Next Steps

- To register the generated license key, read [license-registration.md](license-registration.md)
- For troubleshooting license errors, read [licensing-errors.md](licensing-errors.md)
- For FAQ about licensing, read [licensing-faqs.md](licensing-faqs.md)
