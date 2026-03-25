# Location Settings in FolderBrowser

## Overview

Location settings control where the folder browser dialog starts browsing and which folders are displayed to the user. The FolderBrowser provides properties to set the initial browse location and automatically highlight specific paths.

## Table of Contents

- [StartLocation Property](#startlocation-property)
- [CustomStartLocation Property](#customstartlocation-property)
- [SelectLocation Property](#selectlocation-property)
- [DirectoryPath Property](#directorypath-property)
- [Code Examples](#code-examples)

## StartLocation Property

The `StartLocation` property determines the root folder where browsing begins. This uses the `FolderBrowserFolder` enumeration.

### Available StartLocation Options

| Option | Description |
|--------|-------------|
| `MyComputer` | Starts at "My Computer" root |
| `MyDocuments` | Starts at the user's My Documents folder |
| `MyNetwork` | Starts at "My Network Places" |
| `RecycleBin` | Starts at the Recycle Bin |
| `CustomStartLocation` | Starts at a custom folder path (requires CustomStartLocation property) |

### Basic Usage

```csharp
FolderBrowser folderBrowser = new FolderBrowser();

// Start from My Computer
folderBrowser.StartLocation = FolderBrowserFolder.MyComputer;

// Start from My Documents
folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;

// Start from My Network Places
folderBrowser.StartLocation = FolderBrowserFolder.MyNetwork;
```

## CustomStartLocation Property

When you want to start browsing from a specific directory (not a predefined location), use `CustomStartLocation`.

### Important Note

The `SelectLocation` property only takes effect when `StartLocation` is set to `CustomStartLocation`.

### Using CustomStartLocation

```csharp
FolderBrowser folderBrowser = new FolderBrowser();

// Set to use custom location
folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;

// Specify the custom starting path
folderBrowser.CustomStartLocation = "C:\\Program Files";

folderBrowser.ShowDialog();
```

### Multiple Custom Locations

```csharp
// Start from Documents folder
folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
folderBrowser.CustomStartLocation = Environment.GetFolderPath(
    Environment.SpecialFolder.MyDocuments);

// Or start from a network path
folderBrowser.CustomStartLocation = "\\\\NetworkComputer\\SharedFolder";
```

## SelectLocation Property

The `SelectLocation` property automatically scrolls the folder tree and highlights a specific folder when the dialog opens. This is useful for pre-selecting a default path.

### Prerequisites for SelectLocation

1. `StartLocation` must be set to `CustomStartLocation`
2. The path must be valid and accessible
3. The path must be within the browsing scope

### Using SelectLocation

```csharp
FolderBrowser folderBrowser = new FolderBrowser();

// Configure to use custom location
folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
folderBrowser.CustomStartLocation = "C:\\";

// Pre-select a specific path
folderBrowser.SelectLocation = "C:\\Program Files\\Syncfusion\\Essential Studio";

folderBrowser.ShowDialog();
```

### Real-World Example

```csharp
// Start from C: drive but pre-select a common folder
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
folderBrowser.CustomStartLocation = "C:\\";
folderBrowser.SelectLocation = "C:\\Program Files";
folderBrowser.Description = "Select an installation folder";

if (folderBrowser.ShowDialog() == DialogResult.OK)
{
    string selectedPath = folderBrowser.DirectoryPath;
    // Use selectedPath
}
```

## DirectoryPath Property

After the user selects a folder, use the `DirectoryPath` property to retrieve the selected path.

### Getting the Selected Path

```csharp
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.ShowDialog();

string selectedFolder = folderBrowser.DirectoryPath;
```

### With Error Checking

```csharp
if (folderBrowser.ShowDialog() == DialogResult.OK)
{
    string path = folderBrowser.DirectoryPath;
    
    if (!string.IsNullOrEmpty(path))
    {
        // Use the path
        System.IO.DirectoryInfo dirInfo = new System.IO.DirectoryInfo(path);
        MessageBox.Show($"Selected: {dirInfo.FullName}");
    }
}
```

## Code Examples

### Example 1: Simple Location Setup

```csharp
private void BrowseDocuments()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string path = folderBrowser.DirectoryPath;
        Console.WriteLine($"Selected: {path}");
    }
}
```

### Example 2: Custom Location with Pre-Selection

```csharp
private void BrowseProgramFiles()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    // Start from C: drive
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\";
    
    // Pre-select Program Files
    folderBrowser.SelectLocation = "C:\\Program Files";
    
    folderBrowser.ShowDialog();
}
```

### Example 3: Using Environment Variables

```csharp
private void BrowseUserFolder()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    // Use environment variable for user's home directory
    string userHome = Environment.GetFolderPath(
        Environment.SpecialFolder.UserProfile);
    
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = userHome;
    folderBrowser.SelectLocation = userHome;
    
    folderBrowser.ShowDialog();
}
```

### Example 4: Browsing Network Locations

```csharp
private void BrowseNetwork()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.StartLocation = FolderBrowserFolder.MyNetwork;
    folderBrowser.Description = "Select a network folder";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string networkPath = folderBrowser.DirectoryPath;
        // Network path like: \\ServerName\ShareName
    }
}
```

### Example 5: Complete Workflow

```csharp
public string SelectProjectFolder()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    // Set starting location
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\Projects";
    
    // Pre-select recent project
    folderBrowser.SelectLocation = "C:\\Projects\\ActiveProject";
    
    // Add description
    folderBrowser.Description = "Select project folder";
    
    // Show dialog
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string projectPath = folderBrowser.DirectoryPath;
        
        // Validate path
        if (System.IO.Directory.Exists(projectPath))
        {
            return projectPath;
        }
    }
    
    return null;
}
```

## Best Practices

1. **Always Use CustomStartLocation** when starting from a specific path
2. **Validate Paths** before setting StartLocation or SelectLocation
3. **Use Environment Variables** for standard folders (My Documents, Desktop, etc.)
4. **Check DialogResult** before accessing DirectoryPath
5. **Handle Network Paths** with appropriate error handling

## Common Issues

**Issue:** SelectLocation doesn't highlight the folder
**Solution:** Ensure StartLocation is set to CustomStartLocation first

**Issue:** DirectoryPath is empty after ShowDialog()
**Solution:** Check that DialogResult.OK was returned before accessing DirectoryPath

**Issue:** Custom path not found
**Solution:** Verify the path exists and is accessible before setting it

## Next Steps

- Add validation with [Callback Events](callback-events.md)
- Combine with [Style Options](style-options.md) for advanced configurations
