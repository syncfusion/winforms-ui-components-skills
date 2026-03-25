# Getting Started with EditControl

This guide covers installation, setup, and basic configuration of the EditControl for creating code editor applications.

## When to Read This

Read this guide when you need to:
- Install and reference EditControl assemblies
- Add EditControl to a Windows Forms application via designer or code
- Configure basic properties (size, border, dock style)
- Load files into the editor
- Apply built-in syntax highlighting for the first time
- Create a simple code editor application

## Assembly Requirements

The EditControl requires three assemblies:

| Assembly | Purpose |
|----------|---------|
| **Syncfusion.Edit.Windows.dll** | Primary EditControl assembly |
| **Syncfusion.Tools.Windows.dll** | Supporting tools and utilities |
| **Syncfusion.Shared.Base.dll** | Shared base functionality |

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit
Imports Syncfusion.Windows.Forms.Edit.Enums
```

## Installation Methods

### NuGet Package Manager Console

```bash
Install-Package Syncfusion.Edit.Windows
```

This automatically installs all required dependencies.

### NuGet Package Manager UI

1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for **"Syncfusion.Edit.Windows"**
4. Click **Install**
5. Accept the license agreement

### Manual Assembly Reference

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Click **Browse** button
4. Navigate to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\{framework-version}\`)
5. Select the three required DLL files
6. Click **OK**

## Adding EditControl via Designer

### Step 1: Create Windows Forms Project

Create a new Windows Forms Application project in Visual Studio.

### Step 2: Add to Toolbox

If EditControl is not in the toolbox:

1. Right-click on the **Toolbox**
2. Select **Choose Items**
3. Click **Browse** and navigate to the Syncfusion assemblies
4. Select **Syncfusion.Edit.Windows.dll**
5. Click **OK**

### Step 3: Drag and Drop

1. Locate **EditControl** in the toolbox (usually under "Syncfusion Controls")
2. Drag it onto your form designer
3. The control is added, and assemblies are automatically referenced

### Step 4: Configure Properties

Use the Properties window to configure:

| Property | Value | Description |
|----------|-------|-------------|
| **Dock** | `Fill` | Fill the entire form |
| **BorderStyle** | `Fixed3D` | 3D border appearance |
| **Size** | `800, 600` | Control dimensions |
| **(Name)** | `editControl1` | Control identifier |

## Adding EditControl via Code

### Basic Programmatic Creation

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private EditControl editControl1;
    
    public Form1()
    {
        InitializeComponent();
        CreateEditControl();
    }
    
    private void CreateEditControl()
    {
        // Create the EditControl instance
        editControl1 = new EditControl();
        
        // Set size
        editControl1.Size = new Size(800, 600);
        
        // Set dock property
        editControl1.Dock = DockStyle.Fill;
        
        // Set border style
        editControl1.BorderStyle = BorderStyle.Fixed3D;
        
        // Add to form
        this.Controls.Add(editControl1);
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit
Imports System
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Private editControl1 As EditControl
    
    Public Sub New()
        InitializeComponent()
        CreateEditControl()
    End Sub
    
    Private Sub CreateEditControl()
        ' Create the EditControl instance
        editControl1 = New EditControl()
        
        ' Set size
        editControl1.Size = New Size(800, 600)
        
        ' Set dock property
        editControl1.Dock = DockStyle.Fill
        
        ' Set border style
        editControl1.BorderStyle = BorderStyle.Fixed3D
        
        ' Add to form
        Me.Controls.Add(editControl1)
    End Sub
End Class
```

## Loading Files

### LoadFile Method

Use the `LoadFile()` method to load content from a file:

**C#:**
```csharp
// Load file by path
editControl1.LoadFile(@"C:\Code\Sample.cs");

// Load from relative path
string filePath = Path.GetDirectoryName(Application.ExecutablePath) + @"\..\..\Sample.cs";
editControl1.LoadFile(filePath);

// Load with error handling
try
{
    editControl1.LoadFile(filePath);
}
catch (FileNotFoundException ex)
{
    MessageBox.Show($"File not found: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```

**VB.NET:**
```vb
' Load file by path
editControl1.LoadFile("C:\Code\Sample.vb")

' Load from relative path
Dim filePath As String = Path.GetDirectoryName(Application.ExecutablePath) + "\..\..\Sample.vb"
editControl1.LoadFile(filePath)

' Load with error handling
Try
    editControl1.LoadFile(filePath)
Catch ex As FileNotFoundException
    MessageBox.Show($"File not found: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error)
End Try
```

### Setting Text Directly

**C#:**
```csharp
// Set text content
editControl1.Text = "// Sample C# code\nusing System;\n\nnamespace Example\n{\n    class Program\n    {\n    }\n}";

// Load from string
string code = File.ReadAllText(@"C:\Code\Sample.cs");
editControl1.Text = code;
```

## Applying Built-in Syntax Highlighting

The EditControl supports 11 built-in languages via the `KnownLanguages` enum:

### Available Languages

| Language | Enum Value | File Extensions |
|----------|-----------|-----------------|
| **C#** | `KnownLanguages.CSharp` | .cs |
| **VB.NET** | `KnownLanguages.VB` | .vb |
| **XML** | `KnownLanguages.XML` | .xml |
| **HTML** | `KnownLanguages.HTML` | .html, .htm |
| **Java** | `KnownLanguages.Java` | .java |
| **SQL** | `KnownLanguages.SQL` | .sql |
| **PowerShell** | `KnownLanguages.PowerShell` | .ps1 |
| **C** | `KnownLanguages.C` | .c, .h |
| **JavaScript** | `KnownLanguages.JScript` | .js |
| **VBScript** | `KnownLanguages.VBScript` | .vbs |
| **Delphi** | `KnownLanguages.Delphi` | .pas |

### Applying Configuration

**C#:**
```csharp
// Apply C# syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.CSharp);

// Apply XML syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.XML);

// Apply SQL syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.SQL);
```

**VB.NET:**
```vb
' Apply C# syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.CSharp)

' Apply XML syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.XML)

' Apply SQL syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.SQL)
```

## Complete Application Example

### Simple C# Code Editor

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;

public class SimpleCodeEditorForm : Form
{
    private EditControl editControl1;
    private Button btnOpen;
    private Button btnSave;
    
    public SimpleCodeEditorForm()
    {
        InitializeComponent();
        SetupEditor();
    }
    
    private void SetupEditor()
    {
        // Create EditControl
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.Fixed3D,
            ShowLineNumbers = true
        };
        
        // Apply C# syntax highlighting
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        // Create buttons
        Panel buttonPanel = new Panel { Dock = DockStyle.Top, Height = 40 };
        
        btnOpen = new Button
        {
            Text = "Open File",
            Location = new Point(10, 8),
            Size = new Size(100, 25)
        };
        btnOpen.Click += BtnOpen_Click;
        
        btnSave = new Button
        {
            Text = "Save File",
            Location = new Point(120, 8),
            Size = new Size(100, 25)
        };
        btnSave.Click += BtnSave_Click;
        
        buttonPanel.Controls.Add(btnOpen);
        buttonPanel.Controls.Add(btnSave);
        
        // Add controls to form
        this.Controls.Add(editControl1);
        this.Controls.Add(buttonPanel);
    }
    
    private void BtnOpen_Click(object sender, EventArgs e)
    {
        OpenFileDialog openDialog = new OpenFileDialog
        {
            Filter = "C# Files (*.cs)|*.cs|All Files (*.*)|*.*"
        };
        
        if (openDialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.LoadFile(openDialog.FileName);
            this.Text = $"Code Editor - {Path.GetFileName(openDialog.FileName)}";
        }
    }
    
    private void BtnSave_Click(object sender, EventArgs e)
    {
        SaveFileDialog saveDialog = new SaveFileDialog
        {
            Filter = "C# Files (*.cs)|*.cs|All Files (*.*)|*.*"
        };
        
        if (saveDialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.Save(saveDialog.FileName);
            MessageBox.Show("File saved successfully!", "Success", 
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "Simple Code Editor";
        this.Size = new Size(900, 700);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit
Imports Syncfusion.Windows.Forms.Edit.Enums
Imports System
Imports System.Drawing
Imports System.IO
Imports System.Windows.Forms

Public Class SimpleCodeEditorForm
    Inherits Form
    
    Private editControl1 As EditControl
    Private btnOpen As Button
    Private btnSave As Button
    
    Public Sub New()
        InitializeComponent()
        SetupEditor()
    End Sub
    
    Private Sub SetupEditor()
        ' Create EditControl
        editControl1 = New EditControl With {
            .Dock = DockStyle.Fill,
            .BorderStyle = BorderStyle.Fixed3D,
            .ShowLineNumbers = True
        }
        
        ' Apply C# syntax highlighting
        editControl1.ApplyConfiguration(KnownLanguages.CSharp)
        
        ' Create buttons
        Dim buttonPanel As New Panel With {.Dock = DockStyle.Top, .Height = 40}
        
        btnOpen = New Button With {
            .Text = "Open File",
            .Location = New Point(10, 8),
            .Size = New Size(100, 25)
        }
        AddHandler btnOpen.Click, AddressOf BtnOpen_Click
        
        btnSave = New Button With {
            .Text = "Save File",
            .Location = New Point(120, 8),
            .Size = New Size(100, 25)
        }
        AddHandler btnSave.Click, AddressOf BtnSave_Click
        
        buttonPanel.Controls.Add(btnOpen)
        buttonPanel.Controls.Add(btnSave)
        
        ' Add controls to form
        Me.Controls.Add(editControl1)
        Me.Controls.Add(buttonPanel)
    End Sub
    
    Private Sub BtnOpen_Click(sender As Object, e As EventArgs)
        Dim openDialog As New OpenFileDialog With {
            .Filter = "C# Files (*.cs)|*.cs|All Files (*.*)|*.*"
        }
        
        If openDialog.ShowDialog() = DialogResult.OK Then
            editControl1.LoadFile(openDialog.FileName)
            Me.Text = $"Code Editor - {Path.GetFileName(openDialog.FileName)}"
        End If
    End Sub
    
    Private Sub BtnSave_Click(sender As Object, e As EventArgs)
        Dim saveDialog As New SaveFileDialog With {
            .Filter = "C# Files (*.cs)|*.cs|All Files (*.*)|*.*"
        }
        
        If saveDialog.ShowDialog() = DialogResult.OK Then
            editControl1.Save(saveDialog.FileName)
            MessageBox.Show("File saved successfully!", "Success",
                MessageBoxButtons.OK, MessageBoxIcon.Information)
        End If
    End Sub
    
    Private Sub InitializeComponent()
        Me.Text = "Simple Code Editor"
        Me.Size = New Size(900, 700)
        Me.StartPosition = FormStartPosition.CenterScreen
    End Sub
End Class
```

## Troubleshooting

### Issue: EditControl not in Toolbox

**Solutions:**
1. Verify assemblies are referenced in project
2. Rebuild the solution (Build → Rebuild Solution)
3. Manually add to toolbox (right-click Toolbox → Choose Items)
4. Check Visual Studio version compatibility
5. Ensure NuGet packages are properly installed

### Issue: Syntax Highlighting Not Working

**Solutions:**
1. Verify `ApplyConfiguration()` is called after control creation
2. Ensure the correct `KnownLanguages` enum value is used
3. Check that file extension matches the language
4. Reload the file after applying configuration
5. Verify assemblies are up to date

### Issue: File Not Loading

**Solutions:**
1. Check file path is correct and exists
2. Verify file is not locked by another process
3. Ensure appropriate file permissions
4. Use try-catch to identify specific exceptions
5. Check file encoding compatibility

### Issue: Designer Errors

**Solutions:**
1. Clean and rebuild the solution
2. Close and reopen the designer
3. Verify all assembly references are valid
4. Check for multiple versions of assemblies
5. Reset Visual Studio toolbox

### Issue: Namespace Not Found

**Solutions:**
1. Add `using Syncfusion.Windows.Forms.Edit;` (C#) or `Imports Syncfusion.Windows.Forms.Edit` (VB.NET)
2. Verify **Syncfusion.Edit.Windows.dll** is referenced
3. Check target framework compatibility
4. Rebuild the project
5. Verify NuGet package installation

## Next Steps

- **[Syntax Highlighting](syntax-highlighting.md)** - Learn about built-in languages and custom configurations
- **[Editing Features](editing-features.md)** - Explore clipboard, undo/redo, and selection modes
- **[IntelliSense](intellisense.md)** - Configure auto-complete and auto-correct
- **[Text Visualization](text-visualization.md)** - Enable line numbers, outlining, and bookmarks
