# Callback Events in FolderBrowser

## Table of Contents

- [FolderBrowserCallback Event Overview](#folderbrowsercallback-event-overview)
- [FolderBrowserCallbackEventArgs](#folderbrowsercallbackeventargs)
- [FolderBrowserMessage Types](#folderbrowsermessage-types)
- [Key Event Properties](#key-event-properties)
- [Event Handling Patterns](#event-handling-patterns)
- [Code Examples](#code-examples)

## FolderBrowserCallback Event Overview

The `FolderBrowserCallback` event occurs when an event within the folder browser dialog triggers validation or when state changes happen. This event handler receives `FolderBrowserCallbackEventArgs` which provides information about the event.

### Event Declaration

```csharp
folderBrowser.FolderBrowserCallback += new EventHandler<FolderBrowserCallbackEventArgs>(
    folderBrowser_FolderBrowserCallback);

private void folderBrowser_FolderBrowserCallback(object sender, 
    FolderBrowserCallbackEventArgs e)
{
    // Handle event
}
```

### Lambda Expression

```csharp
folderBrowser.FolderBrowserCallback += (sender, e) =>
{
    // Handle event
};
```

## FolderBrowserCallbackEventArgs

The event arguments provide detailed information about the callback event.

### Available Members

| Member | Type | Description |
|--------|------|-------------|
| `Dismiss` | bool | Gets/sets whether the dialog should be dismissed |
| `FolderBrowserCallbackSetState` | FolderBrowserCallbackSetState | Gets/sets dialog state |
| `BrowseCallbackText` | string | Gets/sets contextual string for status display |
| `FolderBrowserMessage` | FolderBrowserMessage | Type of event that triggered callback |
| `Path` | string | Folder name (valid or invalid) being browsed |
| `Window` | IntPtr | Window handle of browser dialog box |

## FolderBrowserMessage Types

The `FolderBrowserMessage` property indicates which event triggered the callback:

| Message Type | Description | When It Occurs |
|--------------|-------------|----------------|
| `Initialized` | Dialog initialized | Dialog window is created |
| `SelChanged` | Selection changed | User selects a different folder |
| `ValidateFailed` | Validation failed | Invalid path entered in textbox |

### Initialized Message

Fires when the dialog is first opened and initialized.

```csharp
if (e.FolderBrowserMessage == FolderBrowserMessage.Initialized)
{
    e.BrowseCallbackText = "Ready to browse folders";
}
```

### SelChanged Message

Fires when the user navigates to or selects a different folder.

```csharp
if (e.FolderBrowserMessage == FolderBrowserMessage.SelChanged)
{
    string currentPath = e.Path;
    e.BrowseCallbackText = $"Current folder: {currentPath}";
}
```

### ValidateFailed Message

Fires when the user enters an invalid path in the textbox (only with `Validate` style flag).

```csharp
if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
{
    e.BrowseCallbackText = "Invalid path - please check and try again";
    e.Dismiss = false;  // Keep dialog open
}
```

## Key Event Properties

### Dismiss Property

Controls whether the dialog should close or stay open.

```csharp
// Close the dialog
e.Dismiss = true;

// Keep dialog open (for retry)
e.Dismiss = false;
```

### BrowseCallbackText Property

Sets the status or feedback text displayed in the dialog during browsing.

```csharp
// Set status message
e.BrowseCallbackText = "Processing your selection...";
```

### Path Property

Provides the current folder path being browsed or validated.

```csharp
string currentPath = e.Path;

// Check if path is valid
if (Directory.Exists(currentPath))
{
    // Path is valid
}
```

### FolderBrowserCallbackSetState Property

Gets/sets the state of the dialog.

```csharp
var state = e.FolderBrowserCallbackSetState;
```

## Event Handling Patterns

### Pattern 1: Basic Event Subscription

```csharp
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.FolderBrowserCallback += FolderBrowser_FolderBrowserCallback;
folderBrowser.ShowDialog();

private void FolderBrowser_FolderBrowserCallback(object sender, 
    FolderBrowserCallbackEventArgs e)
{
    // Handle event based on message type
    switch (e.FolderBrowserMessage)
    {
        case FolderBrowserMessage.Initialized:
            // Dialog opened
            break;
        case FolderBrowserMessage.SelChanged:
            // User selected a folder
            break;
        case FolderBrowserMessage.ValidateFailed:
            // User entered invalid path
            break;
    }
}
```

### Pattern 2: Lambda Expression

```csharp
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.FolderBrowserCallback += (sender, e) =>
{
    switch (e.FolderBrowserMessage)
    {
        case FolderBrowserMessage.SelChanged:
            e.BrowseCallbackText = e.Path;
            break;
    }
};
folderBrowser.ShowDialog();
```

### Pattern 3: Validation with Custom Logic

```csharp
FolderBrowser folderBrowser = new FolderBrowser();
folderBrowser.Style = FolderBrowserStyles.ShowTextBox | 
                     FolderBrowserStyles.Validate;

folderBrowser.FolderBrowserCallback += (sender, e) =>
{
    if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
    {
        bool isValid = ValidateFolderPath(e.Path);
        
        if (isValid)
        {
            e.BrowseCallbackText = "Path is valid";
            e.Dismiss = true;
        }
        else
        {
            e.BrowseCallbackText = "Invalid path - retry";
            e.Dismiss = false;
        }
    }
};

folderBrowser.ShowDialog();
```

## Code Examples

### Example 1: Simple Status Tracking

```csharp
private void BrowseWithTracking()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    folderBrowser.Description = "Select a folder";
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        switch (e.FolderBrowserMessage)
        {
            case FolderBrowserMessage.Initialized:
                Console.WriteLine("Dialog opened");
                break;
                
            case FolderBrowserMessage.SelChanged:
                Console.WriteLine($"User browsing: {e.Path}");
                break;
        }
    };
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        Console.WriteLine($"Selected: {folderBrowser.DirectoryPath}");
    }
}
```

### Example 2: Path Validation

```csharp
private void BrowseWithValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            // Check if path exists
            if (!Directory.Exists(e.Path))
            {
                e.BrowseCallbackText = $"Path not found: {e.Path}";
                e.Dismiss = false;
            }
            else
            {
                e.BrowseCallbackText = "Valid folder selected";
                e.Dismiss = true;
            }
        }
    };
    
    folderBrowser.ShowDialog();
}
```

### Example 3: Dynamic Status Updates

```csharp
private void BrowseWithStatusUpdates()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.StatusText;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        switch (e.FolderBrowserMessage)
        {
            case FolderBrowserMessage.Initialized:
                e.BrowseCallbackText = "Ready to browse";
                break;
                
            case FolderBrowserMessage.SelChanged:
                // Get folder size for status display
                string folderName = Path.GetFileName(e.Path);
                e.BrowseCallbackText = $"Current: {folderName}";
                break;
        }
    };
    
    folderBrowser.ShowDialog();
}
```

### Example 4: Custom Folder Validation

```csharp
private void BrowseWithCustomValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            // Custom validation rules
            if (IsValidProjectFolder(e.Path))
            {
                e.BrowseCallbackText = "Valid project folder";
                e.Dismiss = true;
            }
            else
            {
                e.BrowseCallbackText = "This is not a valid project folder";
                e.Dismiss = false;
            }
        }
    };
    
    folderBrowser.ShowDialog();
}

private bool IsValidProjectFolder(string path)
{
    // Check for specific files or structure
    return Directory.Exists(Path.Combine(path, "src")) ||
           Directory.Exists(Path.Combine(path, "bin"));
}
```

### Example 5: Permission Checking

```csharp
private void BrowseWithPermissionCheck()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.SelChanged)
        {
            // Check if user has write permissions
            if (CanWriteToFolder(e.Path))
            {
                e.BrowseCallbackText = "Folder is writable";
            }
            else
            {
                e.BrowseCallbackText = "No write permissions";
            }
        }
    };
    
    folderBrowser.ShowDialog();
}

private bool CanWriteToFolder(string path)
{
    try
    {
        // Try to create a test file
        string testFile = Path.Combine(path, ".test");
        File.CreateText(testFile).Close();
        File.Delete(testFile);
        return true;
    }
    catch
    {
        return false;
    }
}
```

### Example 6: Complex Validation Workflow

```csharp
private void BrowseWithComplexValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Description = "Select a backup destination";
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        switch (e.FolderBrowserMessage)
        {
            case FolderBrowserMessage.Initialized:
                e.BrowseCallbackText = "Select a destination folder";
                break;
                
            case FolderBrowserMessage.SelChanged:
                ValidateAndUpdateStatus(e);
                break;
                
            case FolderBrowserMessage.ValidateFailed:
                e.BrowseCallbackText = "Invalid path entered";
                e.Dismiss = false;
                break;
        }
    };
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string selected = folderBrowser.DirectoryPath;
        // Use selected path
    }
}

private void ValidateAndUpdateStatus(FolderBrowserCallbackEventArgs e)
{
    try
    {
        // Check existence
        if (!Directory.Exists(e.Path))
        {
            e.BrowseCallbackText = "Folder does not exist";
            return;
        }
        
        // Check permissions
        if (!CanWriteToFolder(e.Path))
        {
            e.BrowseCallbackText = "No write access";
            return;
        }
        
        // Check disk space
        long availableSpace = GetAvailableDiskSpace(e.Path);
        if (availableSpace < 1000000000) // 1 GB
        {
            e.BrowseCallbackText = "Insufficient disk space";
            return;
        }
        
        e.BrowseCallbackText = "Valid backup destination";
    }
    catch (Exception ex)
    {
        e.BrowseCallbackText = "Error validating path";
    }
}

private long GetAvailableDiskSpace(string path)
{
    DriveInfo drive = new DriveInfo(Path.GetPathRoot(path));
    return drive.AvailableFreeSpace;
}
```

## Best Practices

1. **Always Check Message Type** before accessing path-specific properties
2. **Keep Callbacks Responsive** - avoid long-running operations
3. **Provide Clear Feedback** through BrowseCallbackText
4. **Validate Input** before accepting user selection
5. **Handle Edge Cases** for empty or invalid paths

## Next Steps

- Configure [Style Options](style-options.md) for validation scenarios
- Customize [Text Messages](text-customization.md)
