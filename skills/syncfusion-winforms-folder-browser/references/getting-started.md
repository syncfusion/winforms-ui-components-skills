# Getting Started with FolderBrowser

## Assembly Deployment

To use the FolderBrowser control in your Windows Forms application, you need to add the appropriate assembly references.

### Required Assembly

- **Syncfusion.Shared.Base.dll** - Core assembly for FolderBrowser

### Adding Assembly Reference

**Method 1: Manual DLL Reference**

1. In Visual Studio, right-click on the project in Solution Explorer
2. Select **Add Reference**
3. Browse to the Syncfusion installation folder
4. Navigate to: `{System Drive}\Program Files (x86)\Syncfusion\Essential Studio\{Platform}\{Build Version}\precompiledassemblies\{Framework Version}`
5. Select **Syncfusion.Shared.Base.dll** and click OK

**Method 2: NuGet Package**

Open Package Manager Console and run:

```powershell
Install-Package Syncfusion.Shared.Base
```

Or use NuGet Package Manager UI to search for and install the package.

## Adding Control Through Designer

The FolderBrowser control can be easily added through the Visual Studio designer:

1. **Open Your Windows Forms Project** in Visual Studio

2. **Open the Designer View** by double-clicking your form

3. **Access the Toolbox** (View → Toolbox or press Ctrl+Alt+X)

4. **Search for FolderBrowser** in the Syncfusion components section

5. **Drag and Drop** the FolderBrowser control onto your form
   - The required **Syncfusion.Shared.Base** assembly reference will be added automatically

6. **Configure Properties** in the Properties panel

## Adding Control Through Code

### Basic Implementation

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

// In your form class
private void InitializeFolderBrowser()
{
    // Create FolderBrowser instance
    FolderBrowser folderBrowser1 = new FolderBrowser();
    
    // Specify the Start location
    folderBrowser1.StartLocation = FolderBrowserFolder.MyComputer;
    
    // Specify styles for the FolderBrowser Dialog
    folderBrowser1.Style = FolderBrowserStyles.RestrictToFilesystem | 
                          FolderBrowserStyles.BrowseForComputer;
    
    // Display the folder browser dialog window
    folderBrowser1.ShowDialog();
}
```

### With Result Handling

```csharp
private void SelectFolderButton_Click(object sender, EventArgs e)
{
    FolderBrowser folderBrowser = new FolderBrowser();
    
    folderBrowser.StartLocation = FolderBrowserFolder.MyComputer;
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                         FolderBrowserStyles.NewDialogStyle;
    
    // Show dialog and check result
    if (folderBrowser.ShowDialog() == DialogResult.OK)
    {
        string selectedPath = folderBrowser.DirectoryPath;
        MessageBox.Show($"Selected folder: {selectedPath}");
        
        // Use the selected path
        textBoxPath.Text = selectedPath;
    }
    else
    {
        MessageBox.Show("No folder selected");
    }
}
```

## ShowDialog() Method

The `ShowDialog()` method displays the folder browser dialog window and returns a DialogResult:

```csharp
DialogResult result = folderBrowser.ShowDialog();

switch (result)
{
    case DialogResult.OK:
        // User selected a folder
        string path = folderBrowser.DirectoryPath;
        break;
    case DialogResult.Cancel:
        // User cancelled the dialog
        break;
}
```

## Auto-Complete File Path Feature

The FolderBrowser supports editing and auto-complete functionality by setting the Style to **ShowTextBox**:

```csharp
FolderBrowser folderBrowser = new FolderBrowser();

// Enable textbox with auto-complete suggestions
folderBrowser.Style = FolderBrowserStyles.ShowTextBox;

folderBrowser.ShowDialog();
```

When `ShowTextBox` is set:
- Users can manually edit the folder path
- Auto-complete dropdown displays available paths
- Users can select from suggestions or type custom paths

## Complete Working Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private void BrowseFolderButton_Click(object sender, EventArgs e)
    {
        // Create and configure FolderBrowser
        FolderBrowser folderBrowser = new FolderBrowser();
        
        // Configure dialog
        folderBrowser.Description = "Select a folder to browse";
        folderBrowser.StartLocation = FolderBrowserFolder.MyComputer;
        folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem | 
                             FolderBrowserStyles.ShowTextBox | 
                             FolderBrowserStyles.NewDialogStyle;
        
        // Show dialog
        DialogResult result = folderBrowser.ShowDialog();
        
        // Handle result
        if (result == DialogResult.OK)
        {
            string selectedFolder = folderBrowser.DirectoryPath;
            folderPathTextBox.Text = selectedFolder;
            statusLabel.Text = $"Selected: {selectedFolder}";
        }
        else
        {
            statusLabel.Text = "Selection cancelled";
        }
    }
}
```

## VB.NET Implementation

```vb
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Private Sub SelectFolder()
    Dim folderBrowser As New FolderBrowser()
    
    folderBrowser.StartLocation = FolderBrowserFolder.MyComputer
    folderBrowser.Style = FolderBrowserStyles.RestrictToFilesystem Or _
                         FolderBrowserStyles.NewDialogStyle
    
    If folderBrowser.ShowDialog() = DialogResult.OK Then
        Dim selectedPath As String = folderBrowser.DirectoryPath
        MessageBox.Show($"Selected: {selectedPath}")
    End If
End Sub
```

## Common Setup Patterns

**Pattern 1: Minimal Setup**
```csharp
var fb = new FolderBrowser();
fb.ShowDialog();
```

**Pattern 2: Production-Ready**
```csharp
var fb = new FolderBrowser();
fb.StartLocation = FolderBrowserFolder.MyComputer;
fb.Style = FolderBrowserStyles.RestrictToFilesystem | 
          FolderBrowserStyles.NewDialogStyle;
fb.Description = "Select a destination folder";
fb.ShowDialog();
```

## Next Steps

- Configure starting locations in [Location Settings](location-settings.md)
- Customize dialog styles in [Style Options](style-options.md)
- Add validation with [Callback Events](callback-events.md)
