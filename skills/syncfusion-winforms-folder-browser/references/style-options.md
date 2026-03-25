# Style Options in FolderBrowser

## Overview

The `Style` property uses flags to control the appearance and behavior of the FolderBrowser dialog. Multiple flags can be combined to create custom dialog experiences.

## Table of Contents

- [Style Property Overview](#style-property-overview)
- [Available Style Flags](#available-style-flags)
- [Common Style Combinations](#common-style-combinations)
- [Detailed Flag Descriptions](#detailed-flag-descriptions)
- [Code Examples](#code-examples)

## Style Property Overview

The `Style` property accepts `FolderBrowserStyles` flags, which can be combined using the bitwise OR operator (`|`):

```csharp
folderBrowser.Style = FolderBrowserStyles.Flag1 | FolderBrowserStyles.Flag2 | FolderBrowserStyles.Flag3;
```

## Available Style Flags

| Flag | Description |
|------|-------------|
| `RestrictToFilesystem` | Restricts selection to file system directories only |
| `RestrictToSubfolders` | Returns only file system ancestors |
| `RestrictToDomain` | Excludes network folders below the domain level |
| `BrowseForComputer` | Displays only computers |
| `BrowseForEverything` | Displays files as well as folders |
| `BrowseForPrinter` | Displays only printers |
| `NewDialogStyle` | Uses the new resizable folder selection dialog |
| `AllowUrls` | Displays URLs (requires NewDialogStyle + BrowseForEverything) |
| `ShowAdministrativeShares` | Displays administrative shares on remote systems |
| `ShowShares` | Displays shareable resources on remote systems |
| `ShowTextBox` | Displays textbox for manual path entry with auto-complete |
| `StatusText` | Includes status area in the dialog box |
| `UAHint` | Adds usage hint (only works with NewDialogStyle) |
| `Validate` | Invalid path in textbox triggers FolderBrowserCallback event |

## Common Style Combinations

### Combination 1: Basic Filesystem Browsing
```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem;
```
**Use Case:** Simple folder selection from local filesystem

### Combination 2: Modern Resizable Dialog
```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                     FolderBrowserStyles.NewDialogStyle;
```
**Use Case:** Professional folder selection with modern UI

### Combination 3: With Text Input
```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                     FolderBrowserStyles.ShowTextBox | 
                     FolderBrowserStyles.NewDialogStyle;
```
**Use Case:** Allow users to type folder paths with auto-complete suggestions

### Combination 4: With Validation
```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                     FolderBrowserStyles.ShowTextBox | 
                     FolderBrowserStyles.Validate | 
                     FolderBrowserStyles.NewDialogStyle;
```
**Use Case:** Validate input paths in real-time during browsing

### Combination 5: Computer Selection
```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForComputer;
```
**Use Case:** Allow users to select a computer on the network

### Combination 6: Network with Shares
```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                     FolderBrowserStyles.ShowShares | 
                     FolderBrowserStyles.NewDialogStyle;
```
**Use Case:** Browse network shared folders

### Combination 7: Everything with URLs
```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForEverything | 
                     FolderBrowserStyles.AllowUrls | 
                     FolderBrowserStyles.NewDialogStyle;
```
**Use Case:** Browse folders and enter URLs

## Detailed Flag Descriptions

### RestrictToFilesystem
Restricts the folder browser to displaying only file system directories. Network locations and virtual folders are hidden.

```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem;
```

### RestrictToSubfolders
Returns only file system ancestors (parent directories) of the root location.

```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToSubfolders;
```

### RestrictToDomain
Excludes network folders below the domain level when browsing network locations.

```csharp
folderBrowser.Style = FolderBrowserStyles.RestrictToDomain;
```

### BrowseForComputer
Displays only computers in the network. Used for computer selection dialogs.

```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForComputer;
```

### BrowseForEverything
Displays both files and folders. Allows browsing everything in the filesystem.

```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForEverything;
```

### BrowseForPrinter
Displays only printers available on the network.

```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForPrinter;
```

### NewDialogStyle
Uses the modern Windows dialog style with resizing capability and better UI.

```csharp
folderBrowser.Style = FolderBrowserStyles.NewDialogStyle;
```

**Recommended:** Always use this flag for better user experience.

### AllowUrls
Allows entering URLs in the path field. Must be combined with `BrowseForEverything` and `NewDialogStyle`.

```csharp
folderBrowser.Style = FolderBrowserStyles.BrowseForEverything | 
                     FolderBrowserStyles.AllowUrls | 
                     FolderBrowserStyles.NewDialogStyle;
```

### ShowAdministrativeShares
Displays administrative shares (hidden shares ending with $) on remote systems.

```csharp
folderBrowser.Style = FolderBrowserStyles.ShowAdministrativeShares;
```

### ShowShares
Displays all shareable resources (shared folders, printers) on the network.

```csharp
folderBrowser.Style = FolderBrowserStyles.ShowShares;
```

### ShowTextBox
Adds a textbox to the dialog for manual path entry with auto-complete suggestions.

```csharp
folderBrowser.Style = FolderBrowserStyles.ShowTextBox;
```

### StatusText
Includes a status area in the dialog where messages can be displayed (via callback event).

```csharp
folderBrowser.Style = FolderBrowserStyles.StatusText;
```

**Note:** StatusText doesn't apply to `NewDialogStyle`.

### UAHint
Adds a usage hint for User Account Control. Only works with `NewDialogStyle`.

```csharp
folderBrowser.Style = FolderBrowserStyles.UAHint | 
                     FolderBrowserStyles.NewDialogStyle;
```

### Validate
When enabled with `ShowTextBox`, triggers the `FolderBrowserCallback` event when an invalid path is typed.

```csharp
folderBrowser.Style = FolderBrowserStyles.ShowTextBox | 
                     FolderBrowserStyles.Validate;
```

## Code Examples

### Example 1: Production-Ready Folder Selection

```csharp
private void SelectFolder()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    // Combine essential styles for best UX
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle | 
                         FolderBrowserStyles.ShowTextBox;
    
    folderBrowser.Description = "Select a folder";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string path = folderBrowser.DirectoryPath;
    }
}
```

### Example 2: Network Computer Selection

```csharp
private void SelectNetworkComputer()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.BrowseForComputer | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.Description = "Select a computer";
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string computerName = folderBrowser.DirectoryPath;
    }
}
```

### Example 3: Network Shared Folder Selection

```csharp
private void SelectNetworkShare()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowShares | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.StartLocation = FolderBrowserFolder.MyNetwork;
    
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string shareFolder = folderBrowser.DirectoryPath;
    }
}
```

### Example 4: With Validation and Callback

```csharp
private void SelectWithValidation()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.ShowTextBox | 
                         FolderBrowserStyles.Validate | 
                         FolderBrowserStyles.NewDialogStyle;
    
    folderBrowser.FolderBrowserCallback += (sender, e) =>
    {
        if (e.FolderBrowserMessage == FolderBrowserMessage.ValidateFailed)
        {
            e.BrowseCallbackText = "Invalid path entered";
        }
    };
    
    folderBrowser.ShowDialog();
}
```

### Example 5: Everything Browser

```csharp
private void BrowseEverything()
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.Style = FolderBrowserStyles.BrowseForEverything | 
                         FolderBrowserStyles.NewDialogStyle | 
                         FolderBrowserStyles.ShowTextBox;
    
    folderBrowser.Description = "Select any item";
    
    folderBrowser.ShowDialog();
}
```

## Best Practices

1. **Always include NewDialogStyle** for modern appearance
2. **Use RestrictToFilesystem** when local folders only are needed
3. **Combine ShowTextBox with Validate** for input flexibility
4. **Test flag combinations** - some combinations may not work together
5. **Document the intent** of your style choices in comments

## Common Issues

**Issue:** ShowTextBox doesn't appear
**Solution:** Ensure you're using `ShowTextBox` flag

**Issue:** Validate event not firing
**Solution:** Confirm both `ShowTextBox` and `Validate` flags are set

**Issue:** AllowUrls not working
**Solution:** Must use with `BrowseForEverything` AND `NewDialogStyle`

## Next Steps

- Handle user actions in [Callback Events](callback-events.md)
- Combine with [Location Settings](location-settings.md)
