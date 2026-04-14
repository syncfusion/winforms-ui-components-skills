---
name: syncfusion-winforms-syntax-editor
description: Guide for implementing Syncfusion EditControl (SyntaxEditor) in Windows Forms applications. Use when creating interactive code editors with syntax highlighting, IntelliSense, multi-language support, or Visual Studio-like editing capabilities. Covers installation, syntax highlighting for 11 built-in languages (C#, VB.NET, XML, HTML, Java, SQL, PowerShell, JavaScript), custom language configuration, code outlining, auto-completion, find/replace dialogs, file operations, export (XML/RTF/HTML), split views, and comprehensive event handling for building professional code editor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syntax Editors (EditControl)

This skill guides the implementation of Syncfusion's EditControl, a powerful text editor control for creating interactive code editor applications with Visual Studio-like features including syntax highlighting, IntelliSense, code outlining, and comprehensive editing capabilities.

## When to Use This Skill

Use this skill when you need to:
- Create a code editor application with syntax highlighting for popular languages
- Build a text editor with Visual Studio-like editing features
- Implement syntax-aware code editing with IntelliSense support
- Add code outlining and folding capabilities to text editors
- Create custom language configurations for specialized syntax highlighting
- Build source code viewers or editors for multiple programming languages
- Implement advanced text editing with clipboard operations and undo/redo
- Add find and replace dialogs to text editing applications
- Create split-view code editors for viewing multiple sections simultaneously
- Export or print code with syntax highlighting preserved
- Implement auto-completion and auto-correction in text editors
- Build applications requiring line numbering, bookmarks, and code navigation
- Create text editors with rectangular block selection like Visual Studio

## Component Overview

**Control:** `EditControl`  
**Namespace:** `Syncfusion.Windows.Forms.Edit`  
**Assemblies Required:**
- `Syncfusion.Edit.Windows.dll` (primary)
- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Shared.Base.dll`

### Key Capabilities

The EditControl provides comprehensive code editing features:
- **Syntax Highlighting**: Built-in support for 11 languages (C#, VB.NET, XML, HTML, Java, SQL, PowerShell, JavaScript, VBScript, Delphi, C) plus custom language configuration via XML
- **Editing Features**: Clipboard operations, unlimited undo/redo, drag-and-drop, normal and rectangular block selection, change tracking with line modification markers
- **IntelliSense**: Auto-complete with predefined data, auto-correct for common typos, context tooltips for collapsed code blocks, custom IntelliSense popup
- **Code Outlining**: Expand/collapse code blocks with configurable outlining regions, bracket matching, collapsible sections
- **Text Visualization**: Line numbers, word wrap, current line highlighting, content dividers, underline styles (solid/dash/wave/dot), bookmark and custom indicators
- **Navigation**: Character, word, line, page, and document-level navigation with extensive API
- **Find & Replace**: Built-in dialogs with Visual Studio-like search and replace capabilities
- **File Operations**: Create, open, save with encoding support, export to XML/RTF/HTML, print with formatting
- **Advanced Features**: Split views for viewing multiple document sections, status bar with line/column/caret tracking, single-line mode for TextBox-like behavior, customizable shortcut keys
- **Globalization**: Complete localization support, RTL (right-to-left) text layout
- **Events**: Comprehensive event model for document changes, selection, navigation, and user interactions

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and reference EditControl assemblies
- Create EditControl via designer or code
- Configure basic properties (size, border, dock)
- Load initial content into the editor
- Set up a basic code editor application

### Syntax Highlighting

📄 **Read:** [references/syntax-highlighting.md](references/syntax-highlighting.md)

When you need to:
- Apply built-in syntax highlighting for C#, VB.NET, XML, HTML, Java, SQL, PowerShell, JavaScript, etc.
- Use `ApplyConfiguration()` with `KnownLanguages` enum
- Create custom language configurations using XML
- Define lexems, formats, and code coloring rules
- Configure language-specific syntax elements
- Handle multiple language types in one application

### Editing Features

📄 **Read:** [references/editing-features.md](references/editing-features.md)

When you need to:
- Implement clipboard operations (Cut, Copy, Paste)
- Configure unlimited undo/redo functionality
- Enable normal or rectangular block selection modes
- Support drag-and-drop text editing
- Add block indent and outdent capabilities
- Display line modification markers for change tracking
- Customize the context menu for editing operations

### IntelliSense Features

📄 **Read:** [references/intellisense.md](references/intellisense.md)

When you need to:
- Configure auto-complete with predefined data sources
- Enable auto-correct for common typos and misspellings
- Display context tooltips for collapsed code
- Create custom IntelliSense popups
- Handle IntelliSense events and user interactions
- Customize IntelliSense item appearance and behavior

### Text Visualization

📄 **Read:** [references/text-visualization.md](references/text-visualization.md)

When you need to:
- Display line numbers with customization
- Enable code outlining and folding
- Configure word wrap behavior
- Highlight the current line with custom colors
- Add content dividers between sections
- Apply underline styles (solid, dash, wave, dot)
- Create bookmarks and custom indicators
- Navigate between bookmarks

### File Operations and Export

📄 **Read:** [references/file-operations.md](references/file-operations.md)

When you need to:
- Create new documents programmatically
- Open and load files with `LoadFile()`
- Save documents with `Save()` and `SaveAs()`
- Handle file encoding (UTF-8, Unicode, ASCII)
- Export content to XML, RTF, or HTML formats
- Print documents with syntax highlighting
- Configure print settings and page setup

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Create split views for viewing multiple document sections
- Configure and customize the status bar
- Use single-line mode for TextBox-like behavior
- Implement find and replace dialogs
- Use text navigation methods programmatically
- Customize shortcut key bindings
- Enable RTL (right-to-left) support
- Localize the editor to different languages
- Handle comprehensive event model
- Optimize scrolling behavior for large files

## Quick Start Example

### Basic Code Editor with C# Syntax Highlighting

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;

public class CodeEditorForm : Form
{
    private EditControl editControl1;
    
    public CodeEditorForm()
    {
        InitializeComponent();
        SetupCodeEditor();
    }
    
    private void SetupCodeEditor()
    {
        // Create EditControl
        editControl1 = new EditControl();
        
        // Basic configuration
        editControl1.Dock = DockStyle.Fill;
        editControl1.BorderStyle = BorderStyle.Fixed3D;
        
        // Apply C# syntax highlighting
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        // Enable line numbers
        editControl1.ShowLineNumbers = true;
        
        // Enable code outlining
        editControl1.ShowOutliningCollapsers = true;
        
        // Add to form
        this.Controls.Add(editControl1);
        
        // Load sample code (optional)
        string sampleCode = @"using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(""Hello, World!"");
        }
    }
}";
        editControl1.Text = sampleCode;
    }
    
    private void InitializeComponent()
    {
        this.Text = "C# Code Editor";
        this.Size = new Size(800, 600);
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

Public Class CodeEditorForm
    Inherits Form
    
    Private editControl1 As EditControl
    
    Public Sub New()
        InitializeComponent()
        SetupCodeEditor()
    End Sub
    
    Private Sub SetupCodeEditor()
        ' Create EditControl
        editControl1 = New EditControl()
        
        ' Basic configuration
        editControl1.Dock = DockStyle.Fill
        editControl1.BorderStyle = BorderStyle.Fixed3D
        
        ' Apply C# syntax highlighting
        editControl1.ApplyConfiguration(KnownLanguages.CSharp)
        
        ' Enable line numbers
        editControl1.ShowLineNumbers = True
        
        ' Enable code outlining
        editControl1.ShowOutliningCollapsers = True
        
        ' Add to form
        Me.Controls.Add(editControl1)
        
        ' Load sample code (optional)
        Dim sampleCode As String = "using System;" & vbCrLf & vbCrLf & _
            "namespace HelloWorld" & vbCrLf & _
            "{" & vbCrLf & _
            "    class Program" & vbCrLf & _
            "    {" & vbCrLf & _
            "        static void Main(string[] args)" & vbCrLf & _
            "        {" & vbCrLf & _
            "            Console.WriteLine(""Hello, World!"");" & vbCrLf & _
            "        }" & vbCrLf & _
            "    }" & vbCrLf & _
            "}"
        editControl1.Text = sampleCode
    End Sub
    
    Private Sub InitializeComponent()
        Me.Text = "C# Code Editor"
        Me.Size = New Size(800, 600)
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Multi-Language Code Editor

Create an editor that switches between different programming languages:

**C#:**
```csharp
private ComboBox languageSelector;
private EditControl editControl1;

private void SetupMultiLanguageEditor()
{
    // Language selector
    languageSelector = new ComboBox
    {
        Dock = DockStyle.Top,
        DropDownStyle = ComboBoxStyle.DropDownList
    };
    languageSelector.Items.AddRange(new object[] 
    { 
        "C#", "VB.NET", "XML", "HTML", "Java", "SQL", "PowerShell", "JavaScript" 
    });
    languageSelector.SelectedIndex = 0;
    languageSelector.SelectedIndexChanged += LanguageSelector_SelectedIndexChanged;
    
    // Editor
    editControl1 = new EditControl
    {
        Dock = DockStyle.Fill,
        ShowLineNumbers = true,
        ShowOutliningCollapsers = true
    };
    
    this.Controls.Add(editControl1);
    this.Controls.Add(languageSelector);
    
    // Apply initial language
    ApplyLanguage("C#");
}

private void LanguageSelector_SelectedIndexChanged(object sender, EventArgs e)
{
    ApplyLanguage(languageSelector.SelectedItem.ToString());
}

private void ApplyLanguage(string language)
{
    switch (language)
    {
        case "C#":
            editControl1.ApplyConfiguration(KnownLanguages.CSharp);
            break;
        case "VB.NET":
            editControl1.ApplyConfiguration(KnownLanguages.VB);
            break;
        case "XML":
            editControl1.ApplyConfiguration(KnownLanguages.XML);
            break;
        case "HTML":
            editControl1.ApplyConfiguration(KnownLanguages.HTML);
            break;
        case "Java":
            editControl1.ApplyConfiguration(KnownLanguages.Java);
            break;
        case "SQL":
            editControl1.ApplyConfiguration(KnownLanguages.SQL);
            break;
        case "PowerShell":
            editControl1.ApplyConfiguration(KnownLanguages.PowerShell);
            break;
        case "JavaScript":
            editControl1.ApplyConfiguration(KnownLanguages.JScript);
            break;
    }
}
```

### Pattern 2: File Editor with Save/Load

Create an editor with file operations:

**C#:**
```csharp
private EditControl editControl1;
private string currentFilePath = null;

private void SetupFileEditor()
{
    editControl1 = new EditControl
    {
        Dock = DockStyle.Fill,
        ShowLineNumbers = true
    };
    
    // Menu bar
    MenuStrip menuStrip = new MenuStrip();
    
    ToolStripMenuItem fileMenu = new ToolStripMenuItem("File");
    fileMenu.DropDownItems.Add("New", null, NewFile_Click);
    fileMenu.DropDownItems.Add("Open...", null, OpenFile_Click);
    fileMenu.DropDownItems.Add("Save", null, SaveFile_Click);
    fileMenu.DropDownItems.Add("Save As...", null, SaveFileAs_Click);
    
    menuStrip.Items.Add(fileMenu);
    
    this.Controls.Add(editControl1);
    this.Controls.Add(menuStrip);
    this.MainMenuStrip = menuStrip;
}

private void NewFile_Click(object sender, EventArgs e)
{
    editControl1.Text = string.Empty;
    currentFilePath = null;
    this.Text = "Untitled - Code Editor";
}

private void OpenFile_Click(object sender, EventArgs e)
{
    OpenFileDialog openDialog = new OpenFileDialog
    {
        Filter = "All Files (*.*)|*.*|C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb"
    };
    
    if (openDialog.ShowDialog() == DialogResult.OK)
    {
        editControl1.LoadFile(openDialog.FileName);
        currentFilePath = openDialog.FileName;
        this.Text = Path.GetFileName(currentFilePath) + " - Code Editor";
        
        // Auto-detect language based on extension
        string ext = Path.GetExtension(currentFilePath).ToLower();
        if (ext == ".cs")
            editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        else if (ext == ".vb")
            editControl1.ApplyConfiguration(KnownLanguages.VB);
        else if (ext == ".xml")
            editControl1.ApplyConfiguration(KnownLanguages.XML);
    }
}

private void SaveFile_Click(object sender, EventArgs e)
{
    if (string.IsNullOrEmpty(currentFilePath))
    {
        SaveFileAs_Click(sender, e);
    }
    else
    {
        editControl1.SaveFile(currentFilePath);
    }
}

private void SaveFileAs_Click(object sender, EventArgs e)
{
    SaveFileDialog saveDialog = new SaveFileDialog
    {
        Filter = "All Files (*.*)|*.*|C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb"
    };
    
    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        editControl1.SaveFile(saveDialog.FileName);
        currentFilePath = saveDialog.FileName;
        this.Text = Path.GetFileName(currentFilePath) + " - Code Editor";
    }
}
```

### Pattern 3: Code Editor with IntelliSense

**C#:**
```csharp
editControl1 = new EditControl { Dock = DockStyle.Fill, ShowLineNumbers = true };
editControl1.ApplyConfiguration(KnownLanguages.CSharp);

// Configure IntelliSense suggestions using ContextChoiceController
// Populate suggestions when the context choice opens so they are context-aware
editControl1.ContextChoiceOpen += (s, e) =>
{
    var controller = editControl1.ContextChoiceController;
    controller.Items.Clear();
    controller.Items.Add("Console");
    controller.Items.Add("Console.WriteLine");
    controller.Items.Add("string");
    controller.Items.Add("int");
    controller.Items.Add("public");
    controller.Items.Add("private");
    controller.Items.Add("class");
};

// Configure simple auto-replace (auto-correct) triggers on the editor language
editControl1.Language.AutoReplaceTriggers.Add(new AutoReplaceTrigger("teh", "the"));
editControl1.Language.AutoReplaceTriggers.Add(new AutoReplaceTrigger("cosole", "console"));
editControl1.UseAutoreplaceTriggers = true;

this.Controls.Add(editControl1);
```

## Key Properties and Methods

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | `string` | Gets or sets the text content of the editor |
| **ShowLineNumbers** | `bool` | Shows or hides line numbers in the margin |
| **ShowOutliningCollapsers** | `bool` | Enables code outlining collapse/expand controls |
| **WordWrap** | `bool` | Enables or disables word wrapping |
| **ReadOnly** | `bool` | Makes the editor read-only |
| **TabSize** | `int` | Sets the number of spaces for tab character |
| **AutoCompleteMode** | `AutoCompleteMode` | Configures auto-complete behavior (All, Suggest, None) |
| **EnableAutoCorrect** | `bool` | Enables automatic correction of common typos |
| **StatusBarSettings** | `StatusBarSettings` | Configures the built-in status bar |
| **ContextChoiceOpen** | `bool` | Gets whether IntelliSense context menu is open |
| **CanUndo** | `bool` | Indicates if undo operation is available |
| **CanRedo** | `bool` | Indicates if redo operation is available |
| **IsModified** | `bool` | Indicates if the document has been modified |

### Key Methods

| Method | Description |
|--------|-------------|
| **ApplyConfiguration(KnownLanguages)** | Applies built-in syntax highlighting configuration |
| **LoadFile(string)** | Loads a file into the editor |
| **Save()** | Opens the Save dialog (no arguments) |
| **SaveFile(string)** | Saves the current content to a specified path |
| **SaveAs()** | Opens the Save As dialog |
| **Undo()** | Performs undo operation |
| **Redo()** | Performs redo operation |
| **Cut()** | Cuts selected text to clipboard |
| **Copy()** | Copies selected text to clipboard |
| **Paste()** | Pastes text from clipboard |
| **SelectAll()** | Selects all text in the editor |
| **ShowFindDialog()** | Opens the find dialog |
| **ShowReplaceDialog()** | Opens the replace dialog |
| **GoTo(int)** | Navigates to specified line number |
| **Text (property)** | Gets or sets the text content of the editor |
| **SaveAsXML(string)**, **SaveAsHTML(string)**, **SaveAsRTF(string)** | Export content to XML, HTML, or RTF formats |
| **Print()** | Opens print dialog for the document |

## Common Use Cases

1. **Source Code Editor**: Build Visual Studio-like code editors for C#, VB.NET, Java with syntax highlighting and IntelliSense
2. **Configuration File Editor**: XML, JSON editors with syntax validation
3. **SQL Query Editor**: Database query editors with SQL syntax highlighting
4. **Script Editor**: PowerShell, JavaScript editors for automation
5. **Code Snippet Manager**: Store and organize code snippets with syntax highlighting
6. **Log File Viewer**: View structured logs with syntax highlighting
7. **Multi-Language IDE**: Build IDEs supporting multiple languages with split views

---

## Next Steps

Start with **[Getting Started](references/getting-started.md)** to install and configure the EditControl, then explore specific features through the navigation guide above based on your application requirements.
