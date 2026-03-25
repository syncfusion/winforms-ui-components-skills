# Text Customization in FolderBrowser

## Overview

Text customization allows you to set user-friendly messages in the FolderBrowser dialog to guide users through the folder selection process.

## Description Property

The `Description` property sets the main text displayed in the folder browser dialog to explain what the user should do.

### Basic Usage

```csharp
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.Description = "Select a folder to back up your files";
folderBrowser.ShowDialog();
```

### Purpose of Description

The description appears at the top of the dialog and should clearly explain:
- What the user is selecting
- Why they're selecting it
- Any constraints or requirements

### Setting Descriptive Text

```csharp
// Example 1: For backup destination
folderBrowser.Description = "Select a destination folder for your backup";

// Example 2: For project selection
folderBrowser.Description = "Select your project folder";

// Example 3: For document browsing
folderBrowser.Description = "Browse and select a document location";

// Example 4: For installation path
folderBrowser.Description = "Choose where to install the application";
```

## StatusText Property

When using the `StatusText` style flag, you can display dynamic status messages during folder browsing. This is handled through the callback event.

### StatusText with Style Flag

```csharp
FolderBrowser folderBrowser = new FolderBrowser();

// Enable status text in dialog
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                     FolderBrowserStyles.StatusText;

folderBrowser.Description = "Select a folder";

// Handle status updates in callback
folderBrowser.FolderBrowserCallback += (sender, e) =>
{
    if (e.FolderBrowserMessage == FolderBrowserMessage.SelChanged)
    {
        e.BrowseCallbackText = $"Current selection: {e.Path}";
    }
};

folderBrowser.ShowDialog();
```

**Note:** `StatusText` style doesn't apply when using `NewDialogStyle`.

## BrowseCallbackText in Callbacks

The `BrowseCallbackText` property allows you to set dynamic messages through the callback event:

```csharp
private void folderBrowser_FolderBrowserCallback(object sender, 
    Syncfusion.Windows.Forms.FolderBrowserCallbackEventArgs e)
{
    // Set the callback text based on the event
    switch (e.FolderBrowserMessage)
    {
        case FolderBrowserMessage.SelChanged:
            e.BrowseCallbackText = $"Selected: {e.Path}";
            break;
            
        case FolderBrowserMessage.ValidateFailed:
            e.BrowseCallbackText = "Invalid folder - please try again";
            break;
    }
}
```

## Complete Text Customization Examples

### Example 1: Simple Description

```csharp
private void BrowseSimple()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.Description = "Select your work folder";
    folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;
    
    folderBrowser.ShowDialog();
}
```

### Example 2: Context-Specific Description

```csharp
private string SelectBackupDestination()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Choose a folder where your backup will be saved. " + 
                               "Make sure you have enough disk space.";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        return folderBrowser.DirectoryPath;
    }
    
    return null;
}
```

### Example 3: With Dynamic Status Messages

```csharp
private void BrowseWithStatus()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select a project folder";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.StatusText;
    
    // Handle status updates
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        switch (e.FolderBrowserMessage)
        {
            case FolderBrowserMessage.Initialized:
                e.BrowseCallbackText = "Ready to browse";
                break;
                
            case FolderBrowserMessage.SelChanged:
                e.BrowseCallbackText = $"Browsing: {System.IO.Path.GetFileName(e.Path)}";
                break;
                
            case FolderBrowserMessage.ValidateFailed:
                e.BrowseCallbackText = "Invalid path - select a valid folder";
                break;
        }
    };
    
    folderBrowser.ShowDialog();
}
```

### Example 4: Installation Wizard

```csharp
private string SelectInstallationPath()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select Installation Folder\n\n" +
                               "Choose a destination folder where the application " +
                               "will be installed. You need at least 500 MB of disk space.";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle | 
                         FolderBrowserStyles.ShowTextBox;
    
    folderBrowser.StartLocation = FolderBrowserFolder.CustomStartLocation;
    folderBrowser.CustomStartLocation = "C:\\Program Files";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        return folderBrowser.DirectoryPath;
    }
    
    return null;
}
```

### Example 5: With Validation Messages

```csharp
private void BrowseWithValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select a configuration folder";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            // Provide helpful error message
            if (string.IsNullOrWhiteSpace(e.Path))
            {
                e.BrowseCallbackText = "Please enter a valid folder path";
            }
            else
            {
                e.BrowseCallbackText = $"'{e.Path}' is not a valid folder";
            }
            
            // Keep dialog open for retry
            e.Dismiss = false;
        }
    };
    
    folderBrowser.ShowDialog();
}
```

### Example 6: Multi-Line Description

```csharp
private void BrowseWithDetailedDescription()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    // Use newline characters for multi-line descriptions
    folderBrowser.Description = "Select Data Export Location\n\n" +
                               "This folder will contain exported data files.\n" +
                               "Make sure you have write permissions.";
    
    folderBrowser.StartLocation = FolderBrowserFolder.MyDocuments;
    
    folderBrowser.ShowDialog();
}
```

## Best Practices for Text Customization

1. **Be Clear and Concise**
   - Use simple language
   - Explain the purpose of selection
   - Keep descriptions under 2-3 lines

2. **Provide Context**
   - Specify constraints (disk space, permissions)
   - Explain why this folder is needed
   - Guide users to appropriate locations

3. **Use Multi-Line When Needed**
   - Use `\n` for line breaks in Description
   - Improve readability for longer texts

4. **Dynamic Status Messages**
   - Update status during browsing
   - Validate and provide feedback
   - Help users understand what's happening

5. **Error Messages**
   - Be specific about validation failures
   - Suggest corrective actions
   - Keep tone professional and helpful

## Common Customization Patterns

### Pattern 1: Simple Clear Description
```csharp
folderBrowser.Description = "Select a folder";
```

### Pattern 2: Descriptive with Constraints
```csharp
folderBrowser.Description = "Select a backup destination. " +
                           "Ensure you have at least 5 GB available space.";
```

### Pattern 3: Multi-Line with Context
```csharp
folderBrowser.Description = "Select Project Location\n\n" +
                           "Choose where to create your project folder.";
```

### Pattern 4: With Dynamic Status
```csharp
folderBrowser.FolderBrowserCallback += (sender, e) =>
{
    if (e.FolderBrowserMessage == FolderBrowserMessage.SelChanged)
    {
        e.BrowseCallbackText = $"Selected: {System.IO.Path.GetFileName(e.Path)}";
    }
};
```

## Localization Considerations

For multi-language applications, consider:
- Loading descriptions from resource files
- Using language-specific configurations
- Maintaining consistent terminology

```csharp
// Example with resource file
folderBrowser.Description = Properties.Resources.FolderBrowserDescription;
```

## Next Steps

- Add validation with [Callback Events](callback-events.md)
- Combine with [Style Options](style-options.md)
