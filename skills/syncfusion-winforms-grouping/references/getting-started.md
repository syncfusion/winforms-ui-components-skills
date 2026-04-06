# Getting Started with Syncfusion Grouping Engine

This guide covers installation, deployment, and initial setup of the Syncfusion Grouping Engine for Windows Forms applications.

## Installation Overview

Essential Grouping is part of Syncfusion Essential Studio. After installation, assemblies are located at:

**Windows Forms:**
```
[Install Location]\Syncfusion\Essential Studio\[Version Number]\Windows\Grouping.Windows\
```

**Samples:**
- Windows Forms: `[Install Location]\...\Windows\Grouping.Windows\Samples\2.0` 

## Creating Windows Forms Applications

### Windows Forms Application

**Step 1: Create New Project**

1. Open Visual Studio
2. Go to File → New → Project
3. Select **Windows Forms Application** template
4. Name the project and click OK

A new Windows Forms application is created with a default form.

**Step 2: Add Grouping References**

Add the required Syncfusion assemblies to your project:

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to the Syncfusion installation folder
4. Add the following assemblies: 
   - `Syncfusion.Grouping.Base.dll`
   - `Syncfusion.Grouping.Windows.dll`
   - `Syncfusion.Shared.Base.dll`
   - `Syncfusion.Shared.Windows.dll`
   
## Deployment Configuration

### Setting Copy Local Property

To deploy applications with Syncfusion assemblies, configure Copy Local settings:

**Step 1: Select Syncfusion Assemblies**

1. In Solution Explorer, expand **References** folder
2. Select all Syncfusion assemblies (Ctrl+Click to multi-select)
3. Right-click and select **Properties**

**Step 2: Enable Copy Local**

1. In Properties window, find **Copy Local** property
2. Set to **True**
3. Compile the project

This ensures Syncfusion assemblies are copied to the output directory (bin/debug or bin/release) alongside your executable.

### Licensing Configuration

**Check licenses.licx File**

1. Locate `licenses.licx` file in your project
2. Right-click → Properties
3. Set **Build Action** to **Embedded Resource**

This embeds licensing information in your application assembly.

## Deployment Scenarios

### Windows Forms Desktop Deployment

**Required Files for Deployment:**
- Your application executable (.exe)
- All Syncfusion DLLs referenced in your project 
- `Syncfusion.Grouping.Base.dll`
- `Syncfusion.Grouping.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`

**Deployment Steps:**

1. Build your application in Release mode
2. Navigate to `bin\Release\` folder
3. Copy all files to target machine
4. Ensure Syncfusion DLLs are in the same folder as the .exe

**Alternative: GAC Deployment**

For Windows Forms, you can install Syncfusion assemblies in the GAC on the target machine:

```bash 
gacutil /i Syncfusion.Grouping.Base.dll
gacutil /i Syncfusion.Grouping.Windows.dll
gacutil /i Syncfusion.Shared.Base.dll
gacutil /i Syncfusion.Shared.Windows.dll
```

With GAC deployment, assemblies don't need to be in the application folder.

## Basic Engine Setup

Once assemblies are referenced, set up the Grouping Engine:

```csharp
using Syncfusion.Grouping;
using System.Collections;

// Create Engine instance
Engine groupingEngine = new Engine();

// Create datasource (IList object)
ArrayList dataList = new ArrayList();
// ... populate dataList with objects

// Set datasource
groupingEngine.SetSourceList(dataList);

// Engine is now ready for grouping, sorting, filtering operations
```

## Viewing Sample Applications

**Using Dashboard:**

1. Go to Start → All Programs → Syncfusion → Essential Studio → [Version] → Dashboard
2. Select **Reporting Edition**
3. Click **Run Samples** for Windows Forms
4. Navigate to **Grouping** section
5. Browse and run sample applications

**Direct Sample Location:**

Navigate to sample installation folders and open solution files directly:

- Windows Forms: `[Install Location]\Windows\Grouping.Windows\Samples\2.0\` 

## Source Code Access

**Windows Forms Source:**
```
[System Drive]:\Program Files\Syncfusion\Essential Studio\[Version]\Windows\Grouping.Windows\Src
``` 
Source code is available for studying internal implementations and customization.

## Common Setup Issues

### Assembly Not Found Errors

**Symptom:** Runtime error about missing Syncfusion assemblies

**Solution:**
1. Verify Copy Local = True for all Syncfusion references
2. Check assemblies are in bin folder after build
3. Ensure version numbers match in references and physical DLLs

### Licensing Errors

**Symptom:** License validation errors at runtime

**Solution:**
1. Verify licenses.licx exists in project
2. Check Build Action = Embedded Resource
3. Rebuild project to embed license information

### GAC Deployment Issues

**Symptom:** Application can't find assemblies even when in GAC

**Solution:**
1. Verify assemblies are properly installed in GAC using `gacutil /l`
2. Check version numbers match between reference and GAC assembly
3. Ensure strong name signature is intact 

## Assembly Version Management

When upgrading Syncfusion versions:

1. Remove old version references from project
2. Add new version references
3. Rebuild and test thoroughly
4. Update deployment packages with new DLLs

**Version Consistency:** Always use the same version for all Syncfusion assemblies in a project. Mixing versions can cause compatibility issues.

## Next Steps

1. Review [data-binding.md](data-binding.md) to learn about creating datasources
2. Explore grouping, sorting, and filtering capabilities in respective reference files
3. Examine sample applications for implementation patterns
4. Test deployment in your target environment before production release
