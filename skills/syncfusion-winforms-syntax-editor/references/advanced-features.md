# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Split Views](#split-views)
- [Status Bar](#status-bar)
- [Find and Replace](#find-and-replace)
- [Navigation](#navigation)
- [Scrollbar Customization](#scrollbar-customization)
- [Margins](#margins)
- [Right-to-Left Support](#right-to-left-support)
- [Single Line Mode](#single-line-mode)
- [Code Snippets](#code-snippets)
- [Zoom](#zoom)
- [Localization](#localization)
- [Event Reference](#event-reference)

## When to Read This

Read this guide when you need to:
- Implement split-view editing (side-by-side or quad-view)
- Add status bar with file information and cursor position
- Implement find and replace functionality
- Navigate through code programmatically
- Customize scrollbars with Office styles
- Work with selection, user, and indicator margins
- Support right-to-left languages
- Use EditControl as a single-line input
- Add code snippet support
- Implement zoom functionality
- Localize the control
- Handle comprehensive editor events

## Overview

EditControl provides advanced features for professional code editing:
- **Split Views**: Horizontal, vertical, or quad-view layouts
- **Find/Replace**: Built-in dialogs with regex support
- **Navigation**: Comprehensive navigation methods
- **Status Bar**: Customizable status information
- **Margins**: Selection, user, and indicator margins
- **Snippets**: Code template support
- **Events**: Rich event model for customization

## Split Views

### Split Operations

**C#:**
```csharp
// Split into four quadrants
editControl1.SplitFourQuadrants();

// Split horizontally (top and bottom)
editControl1.SplitHorizontally();

// Split vertically (left and right)
editControl1.SplitVertically();

// Enable splitter visibility
editControl1.ShowHorizontalSplitters = true;
editControl1.ShowVerticalSplitters = true;
```

**VB.NET:**
```vb
' Split into four quadrants
editControl1.SplitFourQuadrants()

' Split horizontally
editControl1.SplitHorizontally()

' Split vertically
editControl1.SplitVertically()

' Enable splitter visibility
editControl1.ShowHorizontalSplitters = True
editControl1.ShowVerticalSplitters = True
```

### Splitter Customization

**C#:**
```csharp
// Customize splitter appearance
editControl1.SplitterBackgroundBrush = new BrushInfo(Color.LightGray);

// Set splitter positions (percentage 0-100)
editControl1.HorizontalSplitterPosition = 50;       // Middle
editControl1.TopVerticalSplitterPosition = 50;      // Top left/right split
editControl1.BottomVerticalSplitterPosition = 50;   // Bottom left/right split
```

## Status Bar

### Enable Status Bar

**C#:**
```csharp
// Enable status bar
editControl1.StatusBarSettings.Visible = true;

// Configure panels
editControl1.StatusBarSettings.TextPanel.Visible = true;
editControl1.StatusBarSettings.StatusPanel.Visible = true;
editControl1.StatusBarSettings.FileNamePanel.Visible = true;
editControl1.StatusBarSettings.CoordsPanel.Visible = true;
```

**VB.NET:**
```vb
' Enable status bar
editControl1.StatusBarSettings.Visible = True

' Configure panels
editControl1.StatusBarSettings.TextPanel.Visible = True
editControl1.StatusBarSettings.StatusPanel.Visible = True
editControl1.StatusBarSettings.FileNamePanel.Visible = True
editControl1.StatusBarSettings.CoordsPanel.Visible = True
```

### Status Bar Styling

**C#:**
```csharp
// Apply Office 2007 style
editControl1.StatusBarSettings.VisualStyle = StatusBarAdvVisualStyle.Office2007;
editControl1.StatusBarSettings.Office2007ColorScheme = Office2007Theme.Blue;

// Apply Office 2010 style
editControl1.StatusBarSettings.VisualStyle = StatusBarAdvVisualStyle.Office2010;
editControl1.StatusBarSettings.Office2010ColorScheme = Office2010Theme.Blue;

// Apply Metro style
editControl1.StatusBarSettings.VisualStyle = StatusBarAdvVisualStyle.Metro;
editControl1.StatusBarSettings.MetroColorScheme = MetroTheme.Default;

// Show resize grip
editControl1.StatusBarSettings.GripVisibility = StatusBarSizingGrip.Show;
```

## Find and Replace

### Show Dialogs

**C#:**
```csharp
// Show find dialog
editControl1.ShowFindDialog();

// Show replace dialog
editControl1.ShowReplaceDialog();

// Show go to line dialog
editControl1.ShowGoToDialog();
```

**VB.NET:**
```vb
' Show find dialog
editControl1.ShowFindDialog()

' Show replace dialog
editControl1.ShowReplaceDialog()

' Show go to line dialog
editControl1.ShowGoToDialog()
```

### Programmatic Find

**C#:**
```csharp
// Find text
bool found = editControl1.FindText(
    "searchText",
    false,  // matchCase
    true,   // wholeWord
    false   // searchUp
);

// Find in range
Point startLocation = new Point(1, 1);
Point endLocation = new Point(100, 50);
bool foundInRange = editControl1.FindRange(
    "searchText",
    ref startLocation,
    ref endLocation,
    FindOptions.MatchCase | FindOptions.WholeWord
);

// Find using regex
bool foundRegex = editControl1.FindRegex(
    @"\b\d{3}-\d{4}\b",  // Pattern: 123-4567
    false,                // searchUp
    false                 // matchCase
);

// Find current word under cursor
editControl1.FindCurrentText();

// Find next occurrence
editControl1.FindNext();
```

### Programmatic Replace

**C#:**
```csharp
// Replace text
bool replaced = editControl1.ReplaceText(
    "oldText",
    "newText",
    false,  // matchCase
    true,   // wholeWord
    false   // searchUp
);

// Replace all occurrences
int replacedCount = editControl1.ReplaceAll(
    "oldText",
    "newText",
    false,  // matchCase
    true    // wholeWord
);

MessageBox.Show($"Replaced {replacedCount} occurrences");
```

### Find and Replace History

**C#:**
```csharp
// Add to find history
editControl1.FindHistory.Insert(0, "searchTerm");

// Add to replace history
editControl1.ReplaceHistory.Insert(0, "replacementText");

// Clear histories
editControl1.FindHistory.Clear();
editControl1.ReplaceHistory.Clear();

// Remove from history
editControl1.FindHistory.RemoveAt(0);
```

## Navigation

### Character Navigation

**C#:**
```csharp
// Move by character
editControl1.MoveUp();
editControl1.MoveDown();
editControl1.MoveLeft();
editControl1.MoveRight();
```

### Word Navigation

**C#:**
```csharp
// Move by word
editControl1.MoveLeftWord();
editControl1.MoveRightWord();
```

### Line Navigation

**C#:**
```csharp
// Move to line start/end
editControl1.MoveToLineStart();
editControl1.MoveToLineEnd();
```

### Page Navigation

**C#:**
```csharp
// Move by page
editControl1.MovePageUp();
editControl1.MovePageDown();
```

### Document Navigation

**C#:**
```csharp
// Move to document start/end
editControl1.MoveToBeginning();
editControl1.MoveToEnd();
```

### Block Navigation

**C#:**
```csharp
// Jump to indent block boundaries
editControl1.JumpToIndentBlockStart();
editControl1.JumpToIndentBlockEnd();
```

### Go To Line

**C#:**
```csharp
// Go to specific line
editControl1.GoTo(50);

// Go to line with lines above visible
editControl1.GoTo(100, 5);  // Line 100 with 5 lines above visible

// Show go to dialog
editControl1.ShowGoToDialog();
```

### Position Properties

**C#:**
```csharp
// Get current position
int currentLine = editControl1.CurrentLine;
int currentColumn = editControl1.CurrentColumn;
string currentWord = editControl1.GetCurrentWord();

// Display in status bar
statusLabel.Text = $"Line {currentLine}, Column {currentColumn}";
```

## Scrollbar Customization

### Basic Scrollbar Settings

**C#:**
```csharp
// Show/hide scrollbars
editControl1.ShowVerticalScroller = true;
editControl1.ShowHorizontalScroller = true;

// Always show scrollbars (even when not needed)
editControl1.AlwaysShowScrollers = true;
```

### Scroll Mode

**C#:**
```csharp
// Scroll mode
editControl1.VScrollMode = ScrollMode.Immediate;  // or ScrollMode.Deferred, ScrollMode.Pixel
```

### Scrollbar Styling

**C#:**
```csharp
// Apply Office 2007 style
editControl1.ScrollVisualStyle = ScrollBarCustomDrawStyles.Office2007;
editControl1.ScrollColorScheme = Office2007ColorScheme.Blue;

// Apply Office 2010 style
editControl1.ScrollVisualStyle = ScrollBarCustomDrawStyles.Office2010;
editControl1.ScrollColorScheme = Office2010ColorScheme.Blue;

// Apply Metro style
editControl1.ScrollVisualStyle = ScrollBarCustomDrawStyles.Metro;
editControl1.ScrollColorScheme = MetroColorScheme.Default;

// Customize with managed colors
Office2007Colors.ApplyManagedColors(editControl1, Color.Blue);
```

### Scroll Position and Offset

**C#:**
```csharp
// Set scroll position
editControl1.ScrollPosition = new Point(0, 100);

// Set scroll offsets (margin from edges)
editControl1.ScrollOffsetTop = 3;
editControl1.ScrollOffsetBottom = 3;
editControl1.ScrollOffsetLeft = 10;
editControl1.ScrollOffsetRight = 10;
```

### Scrollbar Buttons

**C#:**
```csharp
// Add custom buttons to scrollbar
editControl1.ScrollbarBottomButtons.AddRange(new Button[]
{
    new Button { Text = "↓", Size = new Size(16, 16) }
});

editControl1.ScrollbarTopButtons.AddRange(new Button[]
{
    new Button { Text = "↑", Size = new Size(16, 16) }
});
```

## Margins

### Selection Margin

**C#:**
```csharp
// Enable selection margin (left margin for line selection)
editControl1.ShowSelectionMargin = true;
editControl1.SelectionMarginWidth = 15;
editControl1.SelectionMarginForegroundColor = Color.Gray;
editControl1.SelectionMarginBackgroundColor = Color.LightGray;

// Select entire line on line number click
editControl1.SelectOnLineNumberClick = true;
```

### User Margin

**C#:**
```csharp
// Enable user margin (custom margin for annotations)
editControl1.ShowUserMargin = true;
editControl1.UserMarginWidth = 30;
editControl1.UserMarginPlacement = UserMarginPlacement.Left;  // or Right

// Customize appearance
editControl1.UserMarginBackgroundColor = new BrushInfo(Color.LightYellow);
editControl1.UserMarginTextColor = Color.DarkBlue;
editControl1.UserMarginBorderColor = Color.Gray;

// Draw custom content in user margin
editControl1.DrawUserMarginText += (sender, e) =>
{
    // Draw line annotations based on line index
    if (e.LineIndex % 10 == 0)
    {
        e.Text = "★";
        e.Font = new Font("Segoe UI Symbol", 10F);
        e.Color = Color.Gold;
    }
};
```

**VB.NET:**
```vb
' Enable user margin
editControl1.ShowUserMargin = True
editControl1.UserMarginWidth = 30
editControl1.UserMarginPlacement = UserMarginPlacement.Left

' Customize appearance
editControl1.UserMarginBackgroundColor = New BrushInfo(Color.LightYellow)
editControl1.UserMarginTextColor = Color.DarkBlue
editControl1.UserMarginBorderColor = Color.Gray
```

### Indicator Margin

**C#:**
```csharp
// Enable indicator margin (for bookmarks and breakpoints)
editControl1.ShowIndicatorMargin = true;
editControl1.MarkerAreaWidth = 20;
editControl1.IndicatorMarginBackColor = Color.WhiteSmoke;

// Handle indicator margin clicks
editControl1.IndicatorMarginClick += (sender, e) =>
{
    // Toggle bookmark on line
    editControl1.BookmarkToggle(e.LineIndex);
};

editControl1.IndicatorMarginDoubleClick += (sender, e) =>
{
    // Custom action on double-click
    MessageBox.Show($"Double-clicked line {e.LineIndex}");
};
```

## Right-to-Left Support

**C#:**
```csharp
// Enable right-to-left rendering
editControl1.RenderRightToLeft = true;

// Users can toggle RTL with keyboard shortcuts:
// Right Shift + Ctrl = RTL
// Left Shift + Ctrl = LTR
```

**VB.NET:**
```vb
' Enable right-to-left rendering
editControl1.RenderRightToLeft = True
```

## Single Line Mode

**C#:**
```csharp
// Use EditControl as single-line TextBox
editControl1.SingleLineMode = true;
editControl1.BorderStyle = BorderStyle.Fixed3D;
editControl1.Height = 25;
```

## Code Snippets

### Add Code Snippets

**C#:**
```csharp
// Add a code snippet
editControl1.AddCodeSnippet(
    "for",  // Title/shortcut
    new string[] { "i", "max" },  // Literals (placeholders)
    "for (int $i$ = 0; $i$ < $max$; $i$++)\r\n{\r\n\t$end$\r\n}"  // Code template
);

// Add more snippets
editControl1.AddCodeSnippet(
    "prop",
    new string[] { "type", "name" },
    "public $type$ $name$ { get; set; }"
);

editControl1.AddCodeSnippet(
    "if",
    new string[] { "condition" },
    "if ($condition$)\r\n{\r\n\t$end$\r\n}"
);
```

**VB.NET:**
```vb
' Add a code snippet
editControl1.AddCodeSnippet(
    "for",
    New String() {"i", "max"},
    "for (int $i$ = 0; $i$ < $max$; $i$++)" & vbCrLf & "{" & vbCrLf & vbTab & "$end$" & vbCrLf & "}"
)
```

### Snippet Configuration

**C#:**
```csharp
// Load snippets from file
CodeSnippetsContainer container = CodeSnippetsContainer.Extract(@"Snippets\CSharp.xml");
editControl1.CodeSnippetsContainer.AddContainer(container);

// Show code snippets list
editControl1.ShowCodeSnippets();

// Customize appearance
editControl1.DrawCodeSnippetBorder = true;
editControl1.CodeSnippetSize = new Size(200, 100);
```

### Snippet Events

**C#:**
```csharp
// Handle snippet activation
editControl1.CodeSnippetActivating += (sender, e) =>
{
    // Validate or prevent snippet activation
    if (editControl1.CurrentLine > 100)
    {
        e.Cancel = true;
        MessageBox.Show("Snippets not allowed after line 100");
    }
};
```

## Zoom

### Enable Zoom

**C#:**
```csharp
// Enable zoom
editControl1.AllowZoom = true;

// Set zoom factor (1.0 = 100%, 1.5 = 150%, 0.5 = 50%)
editControl1.ZoomFactor = 1.5F;  // 150%
```

**VB.NET:**
```vb
' Enable zoom
editControl1.AllowZoom = True

' Set zoom factor
editControl1.ZoomFactor = 1.5F
```

### Zoom Events

**C#:**
```csharp
// Handle zoom changes
editControl1.ZoomFactorChanged += (sender, e) =>
{
    statusLabel.Text = $"Zoom: {editControl1.ZoomFactor * 100}%";
};

// Prevent certain zoom levels
editControl1.ZoomFactorChanging += (sender, e) =>
{
    // Limit zoom between 50% and 300%
    if (e.NewValue < 0.5F || e.NewValue > 3.0F)
    {
        e.Cancel = true;
        MessageBox.Show("Zoom must be between 50% and 300%");
    }
};
```

## Localization

**C#:**
```csharp
// Localize control to French
editControl1.Localize("Resources\\EditControl_fr.xml");

// Localize default language auto-replace triggers
editControl1.DefaultLanguage.AutoReplaceTriggers.Add(
    new AutoReplaceTrigger("le", "the")
);
```

**Example localization XML:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Strings>
  <String Name="FindDialog_Title">Rechercher</String>
  <String Name="ReplaceDialog_Title">Remplacer</String>
  <String Name="GoToDialog_Title">Aller à la ligne</String>
  <String Name="Find_Button">Rechercher</String>
  <String Name="Replace_Button">Remplacer</String>
  <String Name="ReplaceAll_Button">Remplacer tout</String>
</Strings>
```

## Event Reference

### Text Events

**C#:**
```csharp
// Text changed
editControl1.TextChanged += (sender, e) =>
{
    // Update UI, mark as modified
    this.Text = $"*{currentFileName}";
};

// Text changing (can cancel)
editControl1.TextChanging += (sender, e) =>
{
    // Prevent certain changes
    if (readOnlyMode)
    {
        e.Cancel = true;
    }
};

// Line events
editControl1.LineChanged += (sender, e) => { };
editControl1.LineInserted += (sender, e) => { };
editControl1.LineDeleted += (sender, e) => { };
```

### Selection and Cursor Events

**C#:**
```csharp
// Selection changed
editControl1.SelectionChanged += (sender, e) =>
{
    statusLabel.Text = $"Selected: {editControl1.SelectedText.Length} chars";
};

// Cursor position changed
editControl1.CursorPositionChanged += (sender, e) =>
{
    coordsLabel.Text = $"Ln {editControl1.CurrentLine}, Col {editControl1.CurrentColumn}";
};
```

### Undo/Redo Events

**C#:**
```csharp
// Undo/redo availability changed
editControl1.CanUndoRedoChanged += (sender, e) =>
{
    btnUndo.Enabled = editControl1.CanUndo;
    btnRedo.Enabled = editControl1.CanRedo;
};
```

### Configuration Events

**C#:**
```csharp
// Configuration changed
editControl1.ConfigurationChanged += (sender, e) =>
{
    // Update UI for new configuration
};

// Language changed
editControl1.LanguageChanged += (sender, e) =>
{
    statusLabel.Text = $"Language: {editControl1.Language.Name}";
};
```

### Outlining Events

**C#:**
```csharp
// Before collapse (can cancel)
editControl1.OutliningBeforeCollapse += (sender, e) =>
{
    // Prevent collapsing certain regions
    if (e.Line == 1)  // Don't collapse first line
    {
        e.Cancel = true;
    }
};

// After operations
editControl1.OutliningCollapse += (sender, e) => { };
editControl1.OutliningExpand += (sender, e) => { };
editControl1.CollapsedAll += (sender, e) => 
{
    MessageBox.Show("All regions collapsed");
};
editControl1.ExpandedAll += (sender, e) => 
{
    MessageBox.Show("All regions expanded");
};
```

### Scroll Events

**C#:**
```csharp
// Scroll events
editControl1.HorizontalScroll += (sender, e) =>
{
    // Track horizontal scroll position
};

editControl1.VerticalScroll += (sender, e) =>
{
    // Track vertical scroll position
};
```

### File Events

**C#:**
```csharp
// Closing (can save or cancel)
editControl1.Closing += (sender, e) =>
{
    if (editControl1.IsModified)
    {
        e.Action = SaveChangesAction.ShowDialog;  // Prompt user
        // or SaveChangesAction.Save, SaveChangesAction.Discard
    }
};
```

## Complete Advanced Features Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.Windows.Forms;

public class AdvancedEditorForm : Form
{
    private EditControl editControl1;
    private StatusStrip statusStrip;
    private ToolStripStatusLabel lblPosition;
    private ToolStripStatusLabel lblZoom;
    private MenuStrip menuStrip;
    
    public AdvancedEditorForm()
    {
        InitializeComponent();
        SetupEditor();
        SetupUI();
    }
    
    private void SetupEditor()
    {
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            ShowLineNumbers = true,
            ShowOutliningCollapsers = true,
            AllowZoom = true
        };
        
        // Apply C# configuration
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        // Enable status bar
        editControl1.StatusBarSettings.Visible = true;
        editControl1.StatusBarSettings.TextPanel.Visible = true;
        editControl1.StatusBarSettings.CoordsPanel.Visible = true;
        editControl1.StatusBarSettings.FileNamePanel.Visible = true;
        editControl1.StatusBarSettings.VisualStyle = StatusBarAdvVisualStyle.Office2007;
        
        // Enable margins
        editControl1.ShowSelectionMargin = true;
        editControl1.ShowIndicatorMargin = true;
        editControl1.ShowUserMargin = true;
        editControl1.UserMarginWidth = 25;
        
        // Add code snippets
        AddCodeSnippets();
        
        // Wire events
        editControl1.CursorPositionChanged += (s, e) => UpdatePosition();
        editControl1.ZoomFactorChanged += (s, e) => UpdateZoom();
        editControl1.IndicatorMarginClick += (s, e) => editControl1.BookmarkToggle(e.LineIndex);
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupUI()
    {
        // Menu
        menuStrip = new MenuStrip();
        
        ToolStripMenuItem viewMenu = new ToolStripMenuItem("&View");
        viewMenu.DropDownItems.Add("&Find...", null, (s, e) => editControl1.ShowFindDialog());
        viewMenu.DropDownItems.Add("&Replace...", null, (s, e) => editControl1.ShowReplaceDialog());
        viewMenu.DropDownItems.Add("&Go To Line...", null, (s, e) => editControl1.ShowGoToDialog());
        viewMenu.DropDownItems.Add(new ToolStripSeparator());
        viewMenu.DropDownItems.Add("Split &Horizontally", null, (s, e) => editControl1.SplitHorizontally());
        viewMenu.DropDownItems.Add("Split &Vertically", null, (s, e) => editControl1.SplitVertically());
        viewMenu.DropDownItems.Add("Split &Four", null, (s, e) => editControl1.SplitFourQuadrants());
        viewMenu.DropDownItems.Add(new ToolStripSeparator());
        viewMenu.DropDownItems.Add("Zoom &In", null, (s, e) => editControl1.ZoomFactor += 0.1F);
        viewMenu.DropDownItems.Add("Zoom &Out", null, (s, e) => editControl1.ZoomFactor -= 0.1F);
        viewMenu.DropDownItems.Add("Reset Zoom", null, (s, e) => editControl1.ZoomFactor = 1.0F);
        
        menuStrip.Items.Add(viewMenu);
        this.Controls.Add(menuStrip);
        this.MainMenuStrip = menuStrip;
        
        // Status strip
        statusStrip = new StatusStrip();
        lblPosition = new ToolStripStatusLabel { Spring = true, TextAlign = ContentAlignment.MiddleLeft };
        lblZoom = new ToolStripStatusLabel { Width = 80 };
        statusStrip.Items.Add(lblPosition);
        statusStrip.Items.Add(lblZoom);
        this.Controls.Add(statusStrip);
        
        UpdatePosition();
        UpdateZoom();
    }
    
    private void AddCodeSnippets()
    {
        editControl1.AddCodeSnippet(
            "for",
            new string[] { "i", "max" },
            "for (int $i$ = 0; $i$ < $max$; $i$++)\r\n{\r\n\t$end$\r\n}"
        );
        
        editControl1.AddCodeSnippet(
            "foreach",
            new string[] { "item", "collection" },
            "foreach (var $item$ in $collection$)\r\n{\r\n\t$end$\r\n}"
        );
        
        editControl1.AddCodeSnippet(
            "prop",
            new string[] { "type", "name" },
            "public $type$ $name$ { get; set; }"
        );
        
        editControl1.AddCodeSnippet(
            "class",
            new string[] { "name" },
            "public class $name$\r\n{\r\n\t$end$\r\n}"
        );
    }
    
    private void UpdatePosition()
    {
        lblPosition.Text = $"Line {editControl1.CurrentLine}, Column {editControl1.CurrentColumn}";
    }
    
    private void UpdateZoom()
    {
        lblZoom.Text = $"Zoom: {(int)(editControl1.ZoomFactor * 100)}%";
    }
    
    private void InitializeComponent()
    {
        this.Text = "Advanced Code Editor";
        this.Size = new Size(1200, 800);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit
Imports Syncfusion.Windows.Forms.Edit.Enums

Public Class AdvancedEditorForm
    Inherits Form
    
    Private editControl1 As EditControl
    Private lblPosition As ToolStripStatusLabel
    Private lblZoom As ToolStripStatusLabel
    
    Public Sub New()
        InitializeComponent()
        SetupEditor()
    End Sub
    
    Private Sub SetupEditor()
        editControl1 = New EditControl With {
            .Dock = DockStyle.Fill,
            .ShowLineNumbers = True,
            .AllowZoom = True
        }
        
        editControl1.ApplyConfiguration(KnownLanguages.CSharp)
        
        ' Enable status bar
        editControl1.StatusBarSettings.Visible = True
        editControl1.StatusBarSettings.CoordsPanel.Visible = True
        
        ' Wire events
        AddHandler editControl1.CursorPositionChanged, Sub() UpdatePosition()
        AddHandler editControl1.ZoomFactorChanged, Sub() UpdateZoom()
        
        Me.Controls.Add(editControl1)
    End Sub
    
    Private Sub UpdatePosition()
        lblPosition.Text = $"Line {editControl1.CurrentLine}, Column {editControl1.CurrentColumn}"
    End Sub
    
    Private Sub UpdateZoom()
        lblZoom.Text = $"Zoom: {CInt(editControl1.ZoomFactor * 100)}%"
    End Sub
End Class
```

## Next Steps

- Review complete skill: **[SKILL.md](../SKILL.md)** - Comprehensive EditControl guide
- Explore related features: **[Editing Features](editing-features.md)** - Clipboard, undo/redo, selection
