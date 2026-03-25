# Text Visualization

## Table of Contents
- [Overview](#overview)
- [Line Numbers](#line-numbers)
- [Code Outlining](#code-outlining)
- [Word Wrap](#word-wrap)
- [Current Line Highlighting](#current-line-highlighting)
- [Bookmarks](#bookmarks)
- [Underline and Strikethrough](#underline-and-strikethrough)
- [Additional Visual Features](#additional-visual-features)

This guide covers text visualization features including line numbers, code folding, word wrap, bookmarks, and visual indicators.

## When to Read This

Read this guide when you need to:
- Display line numbers with customization
- Enable code outlining and folding
- Configure word wrap behavior
- Highlight the current line
- Create and navigate bookmarks
- Apply underline styles for error indicators
- Show content dividers and column guides
- Create custom visual indicators

## Overview

EditControl provides comprehensive text visualization features for professional code editing:
- Line numbers with custom formatting
- Code outlining with collapsible regions
- Flexible word wrapping options
- Current line highlighting
- Bookmark management
- Custom underline and strikethrough styles
- Content dividers and column guides

## Line Numbers

### Basic Configuration

**C#:**
```csharp
// Enable line numbers
editControl1.ShowLineNumbers = true;

// Alignment
editControl1.LineNumbersAlignment = LineNumberAlignment.Right; // or Left

// Color
editControl1.LineNumbersColor = Color.Gray;

// Font
editControl1.LineNumbersFont = new Font("Consolas", 9);

// Select entire line when clicking line number
editControl1.SelectOnLineNumberClick = true;
```

**VB.NET:**
```vb
' Enable line numbers
editControl1.ShowLineNumbers = True

' Alignment
editControl1.LineNumbersAlignment = LineNumberAlignment.Right

' Color
editControl1.LineNumbersColor = Color.Gray

' Font
editControl1.LineNumbersFont = New Font("Consolas", 9)

' Select entire line when clicking line number
editControl1.SelectOnLineNumberClick = True
```

### Custom Line Number Drawing

**C#:**
```csharp
editControl1.BeforeLineNumberPaint += EditControl1_BeforeLineNumberPaint;

private void EditControl1_BeforeLineNumberPaint(object sender, LineNumberPaintEventArgs e)
{
    // Highlight every 5th line with larger red text
    if (e.LineNumber % 5 == 0)
    {
        e.Graphics.DrawString(
            e.LineNumber.ToString(),
            new Font("Consolas", 12, FontStyle.Bold),
            new SolidBrush(Color.Red),
            e.Bounds
        );
        e.Handled = true; // Prevent default drawing
    }
}
```

### Line Background Color

**C#:**
```csharp
// Register background format
IBackgroundFormat format = editControl1.RegisterBackColorFormat(
    Color.LightYellow,      // Background color
    Color.Orange,            // Foreground color
    HatchStyle.Cross,        // Pattern style
    true                     // Draw background
);

// Apply to specific line
editControl1.SetLineBackColor(5, true, format);

// Remove background from line
editControl1.SetLineBackColor(5, false, null);
```

## Code Outlining

### Enable Outlining

**C#:**
```csharp
// Enable outlining collapsers
editControl1.ShowOutliningCollapsers = true;

// Enable tooltips for collapsed regions
editControl1.ShowOutliningTooltip = true;
```

### Collapse/Expand Operations

**C#:**
```csharp
// Collapse current block
editControl1.Collapse();

// Expand current block
editControl1.Expand();

// Toggle current block
editControl1.ToggleLineCollapsing();

// Collapse all
editControl1.CollapseAll();

// Expand all
editControl1.ExpandAll();
```

**VB.NET:**
```vb
' Collapse current block
editControl1.Collapse()

' Expand current block
editControl1.Expand()

' Toggle current block
editControl1.ToggleLineCollapsing()

' Collapse all
editControl1.CollapseAll()

' Expand all
editControl1.ExpandAll()
```

### Outlining Events

**C#:**
```csharp
// Before collapse
editControl1.OutliningBeforeCollapse += (sender, e) =>
{
    // Cancel collapse if condition met
    if (someCondition)
    {
        e.Cancel = true;
    }
};

// After collapse
editControl1.OutliningCollapse += (sender, e) =>
{
    Console.WriteLine("Region collapsed");
};

// After expand
editControl1.OutliningExpand += (sender, e) =>
{
    Console.WriteLine("Region expanded");
};

// All collapsed
editControl1.CollapsedAll += (sender, e) =>
{
    statusLabel.Text = "All regions collapsed";
};

// All expanded
editControl1.ExpandedAll += (sender, e) =>
{
    statusLabel.Text = "All regions expanded";
};
```

## Word Wrap

### Basic Word Wrap

**C#:**
```csharp
// Enable word wrap
editControl1.WordWrap = true;

// Word wrap modes
editControl1.WordWrapMode = WordWrapMode.Control;         // At control edge
editControl1.WordWrapMode = WordWrapMode.SpecifiedColumn; // At specific column
editControl1.WordWrapMode = WordWrapMode.WordWrapMargin;  // At margin

// Word wrap types
editControl1.WordWrapType = WordWrapType.WrapByWord;      // By word boundary
editControl1.WordWrapType = WordWrapType.WrapByChar;      // By character
```

### Column-Based Wrap

**C#:**
```csharp
// Wrap at column 80
editControl1.WordWrapMode = WordWrapMode.SpecifiedColumn;
editControl1.WordWrapColumn = 80;

// Font for measuring columns
editControl1.WordWrapColumnMeasuringFont = new Font("Consolas", 10);

// Indent wrapped lines
editControl1.WrappedLinesOffset = 10;
```

### Word Wrap Margin

**C#:**
```csharp
// Enable word wrap margin
editControl1.WordWrapMarginVisible = true;

// Margin appearance
editControl1.WordWrapMarginLineStyle = DashStyle.Dash;
editControl1.WordWrapMarginLineColor = Color.LightGray;

// Margin background
editControl1.WordWrapMarginBrush = new BrushInfo(
    GradientStyle.Horizontal,
    Color.White,
    Color.LightYellow
);
```

### Wrap Indicators

**C#:**
```csharp
// Show wrap indicators
editControl1.MarkLineWrapping = true;
editControl1.MarkWrappedLines = true;

// Custom wrap indicator images
editControl1.CustomWrappedLinesMarkingImage = wrapImage;
editControl1.CustomLineWrappingMarkingImage = lineWrapImage;
```

## Current Line Highlighting

**C#:**
```csharp
// Enable current line highlighting
editControl1.HighlightCurrentLine = true;

// Highlight color
editControl1.CurrentLineHighlightColor = Color.LightYellow;
```

**VB.NET:**
```vb
' Enable current line highlighting
editControl1.HighlightCurrentLine = True

' Highlight color
editControl1.CurrentLineHighlightColor = Color.LightYellow
```

## Bookmarks

### Basic Bookmark Operations

**C#:**
```csharp
// Show indicator margin for bookmarks
editControl1.ShowIndicatorMargin = true;
editControl1.MarkerAreaWidth = 20;

// Add bookmark to line 5
editControl1.BookmarkAdd(5);

// Toggle bookmark at current line
editControl1.BookmarkToggle();

// Navigate bookmarks
editControl1.BookmarkNext();
editControl1.BookmarkPrevious();

// Remove bookmark from line 5
editControl1.BookmarkRemove(5);

// Clear all bookmarks
editControl1.BookmarkClear();
```

**VB.NET:**
```vb
' Show indicator margin for bookmarks
editControl1.ShowIndicatorMargin = True
editControl1.MarkerAreaWidth = 20

' Add bookmark to line 5
editControl1.BookmarkAdd(5)

' Toggle bookmark at current line
editControl1.BookmarkToggle()

' Navigate bookmarks
editControl1.BookmarkNext()
editControl1.BookmarkPrevious()

' Remove bookmark from line 5
editControl1.BookmarkRemove(5)

' Clear all bookmarks
editControl1.BookmarkClear()
```

### Custom Bookmarks

**C#:**
```csharp
// Add bookmark with custom gradient
BrushInfo bookmarkBrush = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Yellow,
    Color.Orange
);
editControl1.BookmarkAdd(10, bookmarkBrush);

// Custom bookmark painting
ICustomBookmark customBookmark = editControl1.SetCustomBookmark(
    15,
    new BookmarkPaintEventHandler(CustomBookmarkPainter)
);
customBookmark.UseInBookmarkSearch = true;

private void CustomBookmarkPainter(object sender, BookmarkPaintEventArgs e)
{
    // Draw custom bookmark (e.g., circle)
    e.Graphics.FillEllipse(
        new SolidBrush(Color.Red),
        e.ClipRectangle
    );
    
    // Draw bookmark number
    e.Graphics.DrawString(
        e.LineIndex.ToString(),
        new Font("Arial", 8, FontStyle.Bold),
        Brushes.White,
        e.ClipRectangle
    );
}
```

### Bookmark Tooltips

**C#:**
```csharp
// Enable bookmark tooltips
editControl1.ShowBookmarkTooltip = true;

// Customize tooltip appearance
editControl1.BookmarkTooltipBackgroundBrush = new BrushInfo(
    PatternStyle.Percent05,
    Color.WindowText,
    Color.LightYellow
);
editControl1.BookmarkTooltipBorderColor = Color.Orange;

// Custom tooltip text
editControl1.UpdateBookmarkToolTip += (sender, e) =>
{
    e.Text = $"Bookmark at line {e.LineIndex}\nDouble-click to remove";
};
```

## Underline and Strikethrough

### Underline Styles

**C#:**
```csharp
// Register underline format
ISnippetFormat underlineFormat = editControl1.RegisterUnderlineFormat(
    Color.Red,                    // Underline color
    UnderlineStyle.Wave,          // Style: Solid, Dash, Wave, Dot
    UnderlineWeight.Bold          // Weight: Thin, Normal, Bold, Heavy
);

// Apply underline to range
editControl1.SetUnderline(
    new Point(1, 1),              // Start (line 1, column 1)
    new Point(1, 10),             // End (line 1, column 10)
    underlineFormat
);

// Remove underline
editControl1.RemoveUnderline(
    new Point(1, 1),
    new Point(1, 10)
);
```

**VB.NET:**
```vb
' Register underline format
Dim underlineFormat As ISnippetFormat = editControl1.RegisterUnderlineFormat(
    Color.Red,
    UnderlineStyle.Wave,
    UnderlineWeight.Bold
)

' Apply underline to range
editControl1.SetUnderline(
    New Point(1, 1),
    New Point(1, 10),
    underlineFormat
)

' Remove underline
editControl1.RemoveUnderline(
    New Point(1, 1),
    New Point(1, 10)
)
```

### Strikethrough

**C#:**
```csharp
// Strikethrough entire line
editControl1.StrikeThrough(5, Color.Red);

// Strikethrough specific range
editControl1.StrikeThrough(
    new Point(10, 5),    // Start
    new Point(10, 20),   // End
    Color.Gray
);
```

### Error Indicators Example

**C#:**
```csharp
private void ShowErrorIndicators()
{
    // Register wave underline for errors
    ISnippetFormat errorFormat = editControl1.RegisterUnderlineFormat(
        Color.Red,
        UnderlineStyle.Wave,
        UnderlineWeight.Normal
    );
    
    // Register warning underline
    ISnippetFormat warningFormat = editControl1.RegisterUnderlineFormat(
        Color.Orange,
        UnderlineStyle.Wave,
        UnderlineWeight.Normal
    );
    
    // Apply to syntax errors
    editControl1.SetUnderline(new Point(5, 10), new Point(5, 25), errorFormat);
    
    // Apply to warnings
    editControl1.SetUnderline(new Point(8, 15), new Point(8, 30), warningFormat);
}
```

## Additional Visual Features

### Content Dividers

**C#:**
```csharp
// Show horizontal lines between methods/regions
editControl1.ShowContentDividers = true;
```

### Column Guides

**C#:**
```csharp
// Show vertical column guide lines
editControl1.ShowColumnGuides = true;

// Define column guide positions and colors
editControl1.ColumnGuideItems = new ColumnGuideItem[]
{
    new ColumnGuideItem(80, Color.LightGray),   // Column 80
    new ColumnGuideItem(120, Color.Red)         // Column 120
};

// Font for measuring columns
editControl1.ColumnGuidesMeasuringFont = new Font("Consolas", 10);
```

### Indentation Guidelines

**C#:**
```csharp
// Show vertical lines for indentation levels
editControl1.ShowIndentationGuidelines = true;

// Guideline color
editControl1.IndentLineColor = Color.LightGray;

// Highlight indent block on hover
editControl1.IndentBlockHighlightingColor = Color.LightBlue;
```

### Bracket Highlighting

**C#:**
```csharp
// Highlight matching brackets
editControl1.OnlyHighlightMatchingBraces = true;

// Brackets are automatically highlighted when cursor is adjacent
```

## Complete Visualization Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;

public class VisualEditorForm : Form
{
    private EditControl editControl1;
    private ToolStrip toolStrip;
    
    public VisualEditorForm()
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
            BorderStyle = BorderStyle.Fixed3D
        };
        
        // Apply C# configuration
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        // Line numbers
        editControl1.ShowLineNumbers = true;
        editControl1.LineNumbersColor = Color.Gray;
        editControl1.LineNumbersFont = new Font("Consolas", 9);
        editControl1.SelectOnLineNumberClick = true;
        
        // Outlining
        editControl1.ShowOutliningCollapsers = true;
        editControl1.ShowOutliningTooltip = true;
        
        // Word wrap
        editControl1.WordWrap = true;
        editControl1.WordWrapMode = WordWrapMode.SpecifiedColumn;
        editControl1.WordWrapColumn = 120;
        
        // Current line
        editControl1.HighlightCurrentLine = true;
        editControl1.CurrentLineHighlightColor = Color.FromArgb(240, 245, 255);
        
        // Bookmarks
        editControl1.ShowIndicatorMargin = true;
        editControl1.MarkerAreaWidth = 20;
        
        // Visual guides
        editControl1.ShowIndentationGuidelines = true;
        editControl1.IndentLineColor = Color.LightGray;
        editControl1.ShowContentDividers = true;
        editControl1.ShowColumnGuides = true;
        editControl1.ColumnGuideItems = new ColumnGuideItem[]
        {
            new ColumnGuideItem(80, Color.LightGray),
            new ColumnGuideItem(120, Color.LightCoral)
        };
        
        // Sample code
        editControl1.Text = @"using System;

namespace Example
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(""Hello, World!"");
            // This is a long line that will wrap if it exceeds the word wrap column setting configured in the editor
        }
    }
}";
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupToolbar()
    {
        toolStrip = new ToolStrip { Dock = DockStyle.Top };
        
        var btnToggleLineNumbers = new ToolStripButton("Line Numbers", null, (s, e) =>
        {
            editControl1.ShowLineNumbers = !editControl1.ShowLineNumbers;
        });
        
        var btnToggleWordWrap = new ToolStripButton("Word Wrap", null, (s, e) =>
        {
            editControl1.WordWrap = !editControl1.WordWrap;
        });
        
        var btnToggleBookmark = new ToolStripButton("Toggle Bookmark", null, (s, e) =>
        {
            editControl1.BookmarkToggle();
        });
        
        var btnNextBookmark = new ToolStripButton("Next Bookmark", null, (s, e) =>
        {
            editControl1.BookmarkNext();
        });
        
        var btnCollapseAll = new ToolStripButton("Collapse All", null, (s, e) =>
        {
            editControl1.CollapseAll();
        });
        
        var btnExpandAll = new ToolStripButton("Expand All", null, (s, e) =>
        {
            editControl1.ExpandAll();
        });
        
        toolStrip.Items.AddRange(new ToolStripItem[]
        {
            btnToggleLineNumbers,
            btnToggleWordWrap,
            new ToolStripSeparator(),
            btnToggleBookmark,
            btnNextBookmark,
            new ToolStripSeparator(),
            btnCollapseAll,
            btnExpandAll
        });
        
        this.Controls.Add(toolStrip);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Visual Editor";
        this.Size = new Size(1000, 750);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

## Next Steps

- **[File Operations](file-operations.md)** - Load, save, and export files
- **[Advanced Features](advanced-features.md)** - Split views, find/replace, navigation, and more
