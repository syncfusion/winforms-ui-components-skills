# Common Patterns for FolderBrowser

## Workflow Overview

A typical FolderBrowser usage follows this workflow:

1. Create FolderBrowser instance
2. Configure location settings
3. Set style options
4. Add description/text
5. (Optional) Subscribe to callback events
6. Call ShowDialog()
7. Check DialogResult
8. Retrieve DirectoryPath if OK

## Pattern 1: Basic Folder Selection

The simplest use case - let user select any folder.

```csharp
private void BasicFolderSelection()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string selectedFolder = folderBrowser.DirectoryPath;
        ProcessFolder(selectedFolder);
    }
}
```

## Pattern 2: Starting from Specific Location

Pre-set the browsing location to guide users.

```csharp
private void SelectFromDocuments()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;
    folderBrowser.Description = "Select a document folder";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        LoadDocumentsFromFolder(folderBrowser.DirectoryPath);
    }
}
```

## Pattern 3: With Default Pre-Selection

Start at a location and pre-highlight a default folder.

```csharp
private void SelectWithDefault()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\";
    folderBrowser.SelectLocation = "C:\\Users\\Documents\\Projects";
    
    folderBrowser.Description = "Select your project folder";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string projectPath = folderBrowser.DirectoryPath;
    }
}
```

## Pattern 4: Text Input with Auto-Complete

Allow users to manually type paths with suggestions.

```csharp
private void SelectWithTextInput()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.Description = "Enter or browse a folder path";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string userFolder = folderBrowser.DirectoryPath;
    }
}
```

## Pattern 5: With Input Validation

Enable validation and handle invalid paths.

```csharp
private void SelectWithValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            if (Directory.Exists(e.Path))
            {
                e.BrowseCallbackText = "Valid folder";
                e.Dismiss = true;
            }
            else
            {
                e.BrowseCallbackText = "Folder not found";
                e.Dismiss = false;
            }
        }
    };
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string validFolder = folderBrowser.DirectoryPath;
    }
}
```

## Pattern 6: Network Folder Selection

Select shared folders on the network.

```csharp
private void SelectNetworkFolder()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.StartLocation = FolderBrowserFolder.MyNetwork;
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowShares | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.Description = "Select a network shared folder";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string networkPath = folderBrowser.DirectoryPath;
        // Example: \\ServerName\ShareName
    }
}
```

## Pattern 7: Computer Selection

Select a computer from the network.

```csharp
private void SelectNetworkComputer()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.BrowseForComputer | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.Description = "Select a computer on the network";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string computerName = folderBrowser.DirectoryPath;
    }
}
```

## Pattern 8: Backup Destination Selection

Full-featured backup folder selection with validation.

```csharp
private string SelectBackupDestination()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select Backup Destination\n\n" +
                               "Choose where to save your backup files. " +
                               "Ensure you have sufficient disk space.";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.SelChanged)
        {
            // Check disk space
            var drive = new DriveInfo(Path.GetPathRoot(e.Path));
            long spaceGB = drive.AvailableFreeSpace / (1024 * 1024 * 1024);
            e.BrowseCallbackText = $"Available: {spaceGB} GB";
        }
        else if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            if (!Directory.Exists(e.Path))
            {
                e.BrowseCallbackText = "Path does not exist";
                e.Dismiss = false;
            }
        }
    };
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        return folderBrowser.DirectoryPath;
    }
    
    return null;
}
```

## Pattern 9: Installation Path Selection

Installation wizard style folder selection.

```csharp
private string SelectInstallationPath(string appName)
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = $"Select Installation Location for {appName}\n\n" +
                               "Default location: C:\\Program Files";
    
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\Program Files";
    folderBrowser.SelectLocation = $"C:\\Program Files\\{appName}";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle | 
                         FolderBrowserStyles.ShowTextBox;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        return folderBrowser.DirectoryPath;
    }
    
    return "C:\\Program Files";
}
```

## Pattern 10: Save Location Selection

Select where to save files.

```csharp
private string SelectSaveLocation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select where to save your files";
    
    // Start from Documents
    folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string savePath = folderBrowser.DirectoryPath;
        
        // Verify we can write to location
        try
        {
            string testFile = Path.Combine(savePath, ".test");
            using (File.Create(testFile))
            { }
            File.Delete(testFile);
            return savePath;
        }
        catch
        {
            MessageBox.Show("Cannot write to selected location");
            return null;
        }
    }
    
    return null;
}
```

## Pattern 11: Project Folder Selection

Select a valid project folder.

```csharp
private string SelectProjectFolder()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select Project Folder\n\n" +
                               "Choose a folder containing a valid project.";
    
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\Projects";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            // Check for project files
            if (IsValidProjectFolder(e.Path))
            {
                e.BrowseCallbackText = "Valid project folder";
                e.Dismiss = true;
            }
            else
            {
                e.BrowseCallbackText = "Not a valid project folder";
                e.Dismiss = false;
            }
        }
    };
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        return folderBrowser.DirectoryPath;
    }
    
    return null;
}

private bool IsValidProjectFolder(string path)
{
    // Check for common project files
    return File.Exists(Path.Combine(path, ".csproj")) ||
           File.Exists(Path.Combine(path, ".sln")) ||
           File.Exists(Path.Combine(path, "package.json"));
}
```

## Pattern 12: Error Handling with Retry

Handle errors gracefully and allow retry.

```csharp
private string SelectFolderWithRetry()
{
    const int maxRetries = 3;
    int attempts = 0;
    
    while (attempts < maxRetries)
    {
        FolderBrowser folderBrowser = new FolderBrowser();
        
        folderBrowser.Description = "Select a folder";
        folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                             FolderBrowserStyles.ShowTextBox | 
                             FolderBrowserStyles.Validate;
        
        folderBrowser.FolderBrowserCallback += (sender, e) =>
        {
            if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
            {
                try
                {
                    if (Directory.Exists(e.Path))
                    {
                        e.BrowseCallbackText = "Valid";
                        e.Dismiss = true;
                    }
                    else
                    {
                        e.BrowseCallbackText = $"Not found (Attempt {attempts + 1}/{maxRetries})";
                        e.Dismiss = false;
                    }
                }
                catch (Exception ex)
                {
                    e.BrowseCallbackText = $"Error: {ex.Message}";
                    e.Dismiss = false;
                }
            }
        };
        
        if (folderBrowser.ShowDialog() == DialogResult.OK)
        {
            return folderBrowser.DirectoryPath;
        }
        
        attempts++;
        
        if (attempts < maxRetries)
        {
            if (MessageBox.Show("Try again?", "Invalid Selection", 
                MessageBoxButtons.YesNo) != DialogResult.Yes)
            {
                break;
            }
        }
    }
    
    return null;
}
```

## Integration with File Operations

### Reading from Selected Folder

```csharp
private void ProcessSelectedFolder()
{
    FolderBrowser fb = new FolderBrowser();
    
    if (fb.ShowDialog() == DialogResult.OK)
    {
        string folder = fb.DirectoryPath;
        
        // Get all files
        string[] files = Directory.GetFiles(folder);
        
        // Process each file
        foreach (string file in files)
        {
            ProcessFile(file);
        }
    }
}
```

### Saving to Selected Folder

```csharp
private void SaveToSelectedFolder(string fileContent)
{
    FolderBrowser fb = new FolderBrowser();
    fb.Description = "Select save location";
    
    if (fb.ShowDialog() == DialogResult.OK)
    {
        string filePath = Path.Combine(fb.DirectoryPath, "output.txt");
        File.WriteAllText(filePath, fileContent);
        MessageBox.Show($"Saved to: {filePath}");
    }
}
```

## Best Practices Summary

1. **Always check DialogResult** - Don't assume OK was clicked
2. **Set meaningful descriptions** - Guide users about what to select
3. **Use appropriate StartLocation** - Help users navigate efficiently
4. **Provide validation** - Ensure selected paths are usable
5. **Handle exceptions** - Wrap file operations in try-catch
6. **Use NewDialogStyle** - Modern appearance and better UX
7. **Test edge cases** - Network paths, permissions, non-existent folders
8. **Document requirements** - Explain folder requirements to users

## Next Steps

- Explore [Style Options](style-options.md) for advanced features
- Learn [Callback Events](callback-events.md) for validation
- Set up [Location Settings](location-settings.md) for specific scenarios
