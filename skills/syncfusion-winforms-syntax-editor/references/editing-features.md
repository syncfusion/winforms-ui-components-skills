# Editing Features

## Table of Contents
- [Overview](#overview)
- [Clipboard Operations](#clipboard-operations)
- [Undo and Redo](#undo-and-redo)
- [Selection Modes](#selection-modes)
- [Text Manipulation](#text-manipulation)
- [Indent and Outdent](#indent-and-outdent)
- [Context Menu](#context-menu)
- [Change Tracking](#change-tracking)

This guide covers comprehensive editing features including clipboard operations, undo/redo, selection modes, text manipulation, and context menu customization.

## When to Read This

Read this guide when you need to:
- Implement clipboard operations (Cut, Copy, Paste)
- Configure unlimited undo/redo functionality
- Enable normal or rectangular block selection modes
- Manipulate text programmatically (insert, delete, append)
- Add indent and outdent capabilities
- Customize the context menu
- Track document modifications

## Overview

EditControl provides Visual Studio-like editing capabilities with full clipboard support, unlimited undo/redo, multiple selection modes, and comprehensive text manipulation APIs.

## Clipboard Operations

### Basic Operations

**C#:**
```csharp
// Copy selected text
editControl1.Copy();

// Cut selected text
editControl1.Cut();

// Paste from clipboard
editControl1.Paste();

// Clear clipboard
editControl1.ClearClipboard();
```

**VB.NET:**
```vb
' Copy selected text
editControl1.Copy()

' Cut selected text
editControl1.Cut()

' Paste from clipboard
editControl1.Paste()

' Clear clipboard
editControl1.ClearClipboard()
```

### Check Availability

**C#:**
```csharp
// Check if copy is available (text selected)
if (editControl1.CanCopy)
{
    editControl1.Copy();
}

// Check if cut is available (text selected and not read-only)
if (editControl1.CanCut)
{
    editControl1.Cut();
}

// Check if paste is available (clipboard has text)
if (editControl1.CanPaste)
{
    editControl1.Paste();
}
```

### FIPS Compliance

For FIPS (Federal Information Processing Standards) compliant environments:

**C#:**
```csharp
// Disable MD5 for FIPS compliance
editControl1.EnableMD5 = false;

// Now clipboard operations will work in FIPS mode
editControl1.Copy();
editControl1.Paste();
```

## Undo and Redo

### Basic Operations

**C#:**
```csharp
// Undo last action
editControl1.Undo();

// Redo last undone action
editControl1.Redo();

// Check availability
if (editControl1.CanUndo)
{
    editControl1.Undo();
}

if (editControl1.CanRedo)
{
    editControl1.Redo();
}
```

**VB.NET:**
```vb
' Undo last action
editControl1.Undo()

' Redo last undone action
editControl1.Redo()

' Check availability
If editControl1.CanUndo Then
    editControl1.Undo()
End If

If editControl1.CanRedo Then
    editControl1.Redo()
End If
```

### Grouping Operations

Group multiple operations into a single undo/redo action:

**C#:**
```csharp
// Enable grouping
editControl1.GroupUndo = true;

// Start undo group
editControl1.UndoGroupOpen();

// Perform multiple operations
editControl1.InsertText(5, 10, "First change");
editControl1.InsertText(10, 15, "Second change");
editControl1.InsertText(15, 20, "Third change");

// Close undo group - all actions become one undo operation
editControl1.UndoGroupClose();

// Now one Undo() will revert all three changes
editControl1.Undo();
```

### Reset Undo Buffer

**C#:**
```csharp
// Clear undo history
editControl1.ResetUndoInfo();

// After this, CanUndo will be false
bool canUndo = editControl1.CanUndo; // false
```

### Undo/Redo Events

**C#:**
```csharp
editControl1.CanUndoRedoChanged += (sender, e) =>
{
    // Update UI buttons
    btnUndo.Enabled = editControl1.CanUndo;
    btnRedo.Enabled = editControl1.CanRedo;
};
```

## Selection Modes

### Default Selection Mode

Default mode selects entire lines:

**C#:**
```csharp
// Set default selection mode
editControl1.SelectionMode = SelectionModes.Default;

// Select all text
editControl1.SelectAll();
```

### Block Selection Mode

Rectangular block selection like Visual Studio:

**C#:**
```csharp
// Enable block (rectangular) selection
editControl1.SelectionMode = SelectionModes.Block;

// Now user can select rectangular blocks with mouse or keyboard
// Hold Alt key and drag mouse for rectangular selection
```

**VB.NET:**
```vb
' Enable block (rectangular) selection
editControl1.SelectionMode = SelectionModes.Block
```

### Programmatic Selection

**C#:**
```csharp
// Select from (line 1, column 1) to (line 20, column 20)
editControl1.StartSelection(1, 1);
editControl1.StopSelection(20, 20);

// Select all
editControl1.SelectAll();

// Get selected text
string selectedText = editControl1.SelectedText;

// Get selection range
Point selectionStart = editControl1.SelectionStart;
Point selectionEnd = editControl1.SelectionEnd;
```

### Selection Events

**C#:**
```csharp
editControl1.SelectionChanged += (sender, e) =>
{
    // Update status bar with selection info
    int lineCount = Math.Abs(editControl1.SelectionEnd.Y - editControl1.SelectionStart.Y) + 1;
    int charCount = editControl1.SelectedText.Length;
    
    statusLabel.Text = $"Selected: {lineCount} lines, {charCount} characters";
};
```

## Text Manipulation

### Insert Text

**C#:**
```csharp
// Enable insert mode
editControl1.InsertMode = true;

// Insert text at specific position (line 7, column 5)
editControl1.InsertText(7, 5, "Inserted text here");

// Insert at current cursor position
editControl1.InsertText(editControl1.CurrentLine, editControl1.CurrentColumn, "New text");
```

### Delete Operations

**C#:**
```csharp
// Delete character to the right of cursor
editControl1.DeleteChar();

// Delete character to the left of cursor (Backspace)
editControl1.DeleteCharLeft();

// Delete word to the right
editControl1.DeleteWord();

// Delete word to the left
editControl1.DeleteWordLeft();

// Delete all text
editControl1.DeleteAll();
```

**VB.NET:**
```vb
' Delete character to the right of cursor
editControl1.DeleteChar()

' Delete character to the left of cursor (Backspace)
editControl1.DeleteCharLeft()

' Delete word to the right
editControl1.DeleteWord()

' Delete word to the left
editControl1.DeleteWordLeft()

' Delete all text
editControl1.DeleteAll()
```

### Append Text

**C#:**
```csharp
// Append text to the end
editControl1.AppendText("This text is appended to the end");

// Append multiple lines
editControl1.AppendText("\nLine 1\nLine 2\nLine 3");
```

### Working with Lines

**C#:**
```csharp
// Set all lines at once
editControl1.Lines = new string[] 
{
    "Line 1",
    "Line 2",
    "Line 3"
};

// Get line count
int lineCount = editControl1.LineCount;

// Get specific line text
string line5 = editControl1.GetLine(5).Text;

// Delete specific line
editControl1.DeleteLine(10);
```

### Complete Text Manipulation Example

**C#:**
```csharp
private void ManipulateTextExample()
{
    // Insert header
    editControl1.InsertText(1, 1, "// Generated Code\n// Date: " + DateTime.Now + "\n\n");
    
    // Append footer
    editControl1.AppendText("\n\n// End of file");
    
    // Insert text at current cursor
    editControl1.InsertText(
        editControl1.CurrentLine, 
        editControl1.CurrentColumn, 
        "// TODO: Implement this method"
    );
    
    // Replace selected text
    if (editControl1.SelectedText.Length > 0)
    {
        int startLine = editControl1.SelectionStart.Y;
        int startCol = editControl1.SelectionStart.X;
        
        editControl1.DeleteSelection();
        editControl1.InsertText(startLine, startCol, "Replacement text");
    }
}
```

## Indent and Outdent

### Tab Settings

**C#:**
```csharp
// Set tab size (number of spaces)
editControl1.TabSize = 4;

// Use actual tab characters
editControl1.UseTabs = true;

// Convert tabs to spaces
editControl1.UseTabs = false;
```

### Manual Indent/Outdent

**C#:**
```csharp
// Indent selection
editControl1.IndentSelection();

// Outdent selection
editControl1.OutdentSelection();

// Indent specific range
editControl1.IndentText(new Point(5, 5), new Point(10, 10));

// Outdent specific range
editControl1.OutdentText(new Point(5, 5), new Point(10, 10));
```

**VB.NET:**
```vb
' Indent selection
editControl1.IndentSelection()

' Outdent selection
editControl1.OutdentSelection()

' Indent specific range
editControl1.IndentText(New Point(5, 5), New Point(10, 10))

' Outdent specific range
editControl1.OutdentText(New Point(5, 5), New Point(10, 10))
```

### Auto Indent

**C#:**
```csharp
// Auto indent modes
editControl1.AutoIndentMode = AutoIndentMode.None;   // No auto-indent
editControl1.AutoIndentMode = AutoIndentMode.Block;  // Match previous line
editControl1.AutoIndentMode = AutoIndentMode.Smart;  // Context-aware indent

// Enable smart indent within blocks
editControl1.EnableSmartInBlockIndent = true;
```

### Indentation Guidelines

**C#:**
```csharp
// Show indentation guide lines
editControl1.ShowIndentationGuidelines = true;

// Customize indentation line color
editControl1.IndentLineColor = Color.Gray;

// Highlight indent block on hover
editControl1.IndentBlockHighlightingColor = Color.LightBlue;
```

## Context Menu

### Enable/Disable Context Menu

**C#:**
```csharp
// Enable built-in context menu
editControl1.ContextMenuEnabled = true;

// Disable context menu
editControl1.ContextMenuEnabled = false;
```

### Customize Context Menu

**C#:**
```csharp
editControl1.MenuFill += EditControl1_MenuFill;

private void EditControl1_MenuFill(object sender, EventArgs e)
{
    ContextMenuManager cm = (ContextMenuManager)sender;
    
    // Add custom menu items
    cm.AddMenuItem("&Find...", ShowFindDialog);
    cm.AddMenuItem("&Replace...", ShowReplaceDialog);
    cm.AddSeparator();
    cm.AddMenuItem("&Comment Selection", CommentSelection);
    cm.AddMenuItem("&Uncomment Selection", UncommentSelection);
    
    // Remove specific default items
    cm.ClearMenu(); // Remove all default items
    
    // Or disable specific items
    cm.ContextMenuProvider.SetContextMenuItemEnabled("&Redo", false);
}

private void ShowFindDialog(object sender, EventArgs e)
{
    editControl1.ShowFindDialog();
}

private void ShowReplaceDialog(object sender, EventArgs e)
{
    editControl1.ShowReplaceDialog();
}

private void CommentSelection(object sender, EventArgs e)
{
    // Add comment symbols to selected lines
    string[] lines = editControl1.SelectedText.Split('\n');
    string commented = string.Join("\n", lines.Select(line => "// " + line));
    
    int startLine = editControl1.SelectionStart.Y;
    int startCol = editControl1.SelectionStart.X;
    
    editControl1.DeleteSelection();
    editControl1.InsertText(startLine, startCol, commented);
}

private void UncommentSelection(object sender, EventArgs e)
{
    // Remove comment symbols from selected lines
    string[] lines = editControl1.SelectedText.Split('\n');
    string uncommented = string.Join("\n", 
        lines.Select(line => line.TrimStart().StartsWith("//") 
            ? line.TrimStart().Substring(2).TrimStart() 
            : line));
    
    int startLine = editControl1.SelectionStart.Y;
    int startCol = editControl1.SelectionStart.X;
    
    editControl1.DeleteSelection();
    editControl1.InsertText(startLine, startCol, uncommented);
}
```

### Custom Shortcut Keys

**C#:**
```csharp
editControl1.RegisteringKeyCommands += EditControl1_RegisteringKeyCommands;

private void EditControl1_RegisteringKeyCommands(object sender, EventArgs e)
{
    // Bind Ctrl+L to Cut
    editControl1.KeyBinder.BindToCommand(Keys.Control | Keys.L, "Clipboard.Cut");
    
    // Bind Ctrl+K to Copy
    editControl1.KeyBinder.BindToCommand(Keys.Control | Keys.K, "Clipboard.Copy");
    
    // Bind Ctrl+Shift+D to duplicate line
    editControl1.KeyBinder.BindToCommand(Keys.Control | Keys.Shift | Keys.D, "Edit.DuplicateLine");
}
```

## Change Tracking

### Track Modifications

**C#:**
```csharp
// Check if document has been modified
bool isModified = editControl1.IsModified;

// Check if file has been modified since last save
bool fileModified = editControl1.FileModified;

// Reset modified flag
editControl1.IsModified = false;
```

### Modification Events

**C#:**
```csharp
editControl1.TextChanged += (sender, e) =>
{
    // Update window title with asterisk if modified
    this.Text = editControl1.IsModified 
        ? $"*{Path.GetFileName(editControl1.FileName)} - Editor" 
        : $"{Path.GetFileName(editControl1.FileName)} - Editor";
};

editControl1.TextChanging += (sender, e) =>
{
    // Optionally cancel text change
    if (someCondition)
    {
        e.Cancel = true;
    }
};
```

### Line Modification Markers

Visual indicators for changed lines:

**C#:**
```csharp
// Line modification markers are automatically shown
// Yellow marker = line changed since last save
// Green marker = line saved
// Lines without markers = unchanged
```

## Complete Editing Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using System;
using System.Drawing;
using System.Windows.Forms;

public class AdvancedEditorForm : Form
{
    private EditControl editControl1;
    private ToolStrip toolStrip;
    private ToolStripButton btnUndo, btnRedo, btnCut, btnCopy, btnPaste;
    private ToolStripComboBox cmbSelectionMode;
    
    public AdvancedEditorForm()
    {
        InitializeComponent();
        SetupEditor();
        SetupToolbar();
    }
    
    private void SetupEditor()
    {
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            ShowLineNumbers = true,
            ShowIndentationGuidelines = true,
            TabSize = 4,
            UseTabs = false,
            AutoIndentMode = AutoIndentMode.Smart
        };
        
        // Events
        editControl1.SelectionChanged += UpdateToolbar;
        editControl1.CanUndoRedoChanged += UpdateToolbar;
        editControl1.TextChanged += EditControl1_TextChanged;
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupToolbar()
    {
        toolStrip = new ToolStrip { Dock = DockStyle.Top };
        
        btnUndo = new ToolStripButton("Undo", null, (s, e) => editControl1.Undo());
        btnRedo = new ToolStripButton("Redo", null, (s, e) => editControl1.Redo());
        btnCut = new ToolStripButton("Cut", null, (s, e) => editControl1.Cut());
        btnCopy = new ToolStripButton("Copy", null, (s, e) => editControl1.Copy());
        btnPaste = new ToolStripButton("Paste", null, (s, e) => editControl1.Paste());
        
        cmbSelectionMode = new ToolStripComboBox { DropDownStyle = ComboBoxStyle.DropDownList };
        cmbSelectionMode.Items.AddRange(new object[] { "Default", "Block" });
        cmbSelectionMode.SelectedIndex = 0;
        cmbSelectionMode.SelectedIndexChanged += (s, e) =>
        {
            editControl1.SelectionMode = cmbSelectionMode.SelectedIndex == 0 
                ? SelectionModes.Default 
                : SelectionModes.Block;
        };
        
        toolStrip.Items.AddRange(new ToolStripItem[] 
        { 
            btnUndo, btnRedo, 
            new ToolStripSeparator(),
            btnCut, btnCopy, btnPaste,
            new ToolStripSeparator(),
            new ToolStripLabel("Selection Mode:"),
            cmbSelectionMode
        });
        
        this.Controls.Add(toolStrip);
    }
    
    private void UpdateToolbar(object sender, EventArgs e)
    {
        btnUndo.Enabled = editControl1.CanUndo;
        btnRedo.Enabled = editControl1.CanRedo;
        btnCut.Enabled = editControl1.CanCut;
        btnCopy.Enabled = editControl1.CanCopy;
        btnPaste.Enabled = editControl1.CanPaste;
    }
    
    private void EditControl1_TextChanged(object sender, EventArgs e)
    {
        this.Text = editControl1.IsModified ? "*Editor" : "Editor";
    }
    
    private void InitializeComponent()
    {
        this.Text = "Advanced Editor";
        this.Size = new Size(900, 700);
    }
}
```

## Next Steps

- **[IntelliSense](intellisense.md)** - Configure auto-complete and context prompts
- **[Text Visualization](text-visualization.md)** - Enable line numbers, outlining, and bookmarks
- **[File Operations](file-operations.md)** - Load, save, and export files
