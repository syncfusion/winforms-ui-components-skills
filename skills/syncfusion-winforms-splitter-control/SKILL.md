---
name: syncfusion-winforms-splitter-control
description: Guide for implementing Syncfusion SplitterControl in Windows Forms applications for creating split views and resizable panes. Use this skill when implementing split views, resizable panes, grid splitting, or synchronized scrolling between multiple views. Covers NuGet installation, programmatic creation, split behavior configuration, and child control integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "Windows Forms"
---

# Implementing Syncfusion Windows Forms Splitter Control

Guide for implementing Syncfusion's SplitterControl in Windows Forms applications. This control enables displaying multiple views of the same grid by using split panes, allowing independent scrolling through each pane.

## When to Use This Skill

Use SplitterControl when you need to:
- Display multiple views of the same grid or worksheet data
- Enable independent scrolling through different sections of content
- Create split-pane interfaces where users can adjust pane sizes by dragging
- Implement synchronized content across multiple panes
- Provide flexible layouts with horizontal and/or vertical dividers
- Build data comparison views where users examine different parts of the same dataset
- Create Excel-like split view functionality
- Integrate with GridControl or other child controls (SfDataGrid, Chart, or any Syncfusion/framework control)

## Component Overview

**SplitterControl** is a container control that enables split-pane functionality with draggable splitter bars. Users can divide the view horizontally, vertically, or both directions.

**Key Features:**
- Horizontal and vertical splitting
- Draggable splitter bars
- Multiple split modes (None, SplitRows, SplitColumns, Both)
- Customizable scrollbar styles (Metro, Office2007, Office2010, None)
- Independent pane scrolling
- Child control integration

**Assembly:** `Syncfusion.Shared.Base`

**Namespace:** `Syncfusion.Windows.Forms`

**Key Class:** `SplitterControl`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.Shared.Base for SplitterControl)
- Child control NuGet packages (reference child control's skill file)
- Assembly and namespace references
- Basic control creation
- Designer availability

### Split Behavior Configuration
📄 **Read:** [references/split-behavior.md](references/split-behavior.md)
- Understanding split modes (None, SplitColumns, SplitRows, Both)
- Configuring DynamicSplitBars property
- Enabling horizontal splitting
- Enabling vertical splitting
- User interaction with split bars

### Scrollbar Customization
📄 **Read:** [references/scrollbar-customization.md](references/scrollbar-customization.md)
- GridOfficeScrollBars property
- Office2007, Office2010, Metro scrollbar styles

### Scrollbar Visibility Features
📄 **Read:** [references/scrollbar-features.md](references/scrollbar-features.md)
- Scrollbar visibility control
- ShowHorizontalScrollBar and ShowVerticalScrollBar properties
- Minimalist UI configurations

### Visual Customization
📄 **Read:** [references/visual-customization.md](references/visual-customization.md)
- ShowSizeGrip property
- Predefined control styles
- Theme configurations

### Adding Child Controls
📄 **Read:** [references/adding-child-controls.md](references/adding-child-controls.md)
- Adding GridControl to splitter
- Adding SfDataGrid to splitter
- Adding any Syncfusion or framework controls
- Simple direct addition pattern
- FillSplitterPane property usage
- Child control configuration

## Quick Start

### Basic Implementation

**Installation:** See [references/getting-started.md](references/getting-started.md) for NuGet package installation steps.

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

public class Form1 : Form
{
    private SplitterControl splitterControl1;
    private GridControl gridControl1;
    
    public Form1()
    {
        // Create SplitterControl
        splitterControl1 = new SplitterControl();
        splitterControl1.Dock = DockStyle.Fill;
        splitterControl1.SplitBars = DynamicSplitBars.Both; // Enable both horizontal and vertical splits
        splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;
        
        // Create GridControl
        gridControl1 = new GridControl();
        gridControl1.FillSplitterPane = true; // Important for proper display in split panes
        
        // Add GridControl to SplitterControl
        splitterControl1.Controls.Add(gridControl1);
        
        // Add SplitterControl to form
        this.Controls.Add(splitterControl1);
    }
}
```

## Common Patterns

### Pattern 1: Simple SplitterControl with GridControl

**Use case:** Basic grid with split view capability

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

// Create SplitterControl
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;

// Create GridControl
GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

// Add GridControl directly to SplitterControl
splitterControl1.Controls.Add(gridControl1);

// Add to form
this.Controls.Add(splitterControl1);
```

### Pattern 2: Horizontal Split Only (Top-Bottom)

**Use case:** Split view with top and bottom panes

```csharp
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.SplitRows; // Horizontal split only
splitterControl1.HSplitPos = 50; // Split at 50% position

GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

splitterControl1.Controls.Add(gridControl1);
this.Controls.Add(splitterControl1);
```

### Pattern 3: Vertical Split Only (Left-Right)

**Use case:** Side-by-side split view

```csharp
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.SplitColumns; // Vertical split only
splitterControl1.VSplitPos = 50; // Split at 50% position

GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

splitterControl1.Controls.Add(gridControl1);
this.Controls.Add(splitterControl1);
```

### Pattern 4: Both Horizontal and Vertical (Quad Pane)

**Use case:** Excel-like split view with four panes

```csharp
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both; // Both directions
splitterControl1.HSplitPos = 50; // Horizontal split at 50%
splitterControl1.VSplitPos = 50; // Vertical split at 50%
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;

GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

splitterControl1.Controls.Add(gridControl1);
this.Controls.Add(splitterControl1);
```

### Pattern 5: SplitterControl with SfDataGrid

**Use case:** Modern data grid with split view

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.DataGrid;

SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;

SfDataGrid sfDataGrid1 = new SfDataGrid();
sfDataGrid1.Dock = DockStyle.Fill; // Use Dock for SfDataGrid

// Add SfDataGrid directly to SplitterControl
splitterControl1.Controls.Add(sfDataGrid1);

this.Controls.Add(splitterControl1);
```

### Pattern 6: Custom Scrollbar Style

**Use case:** Match application theme

```csharp
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;

// Choose scrollbar style:
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;     // Modern Metro style
// splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Office2007;
// splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
// splitterControl1.GridOfficeScrollBars = OfficeScrollBars.None;

GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

splitterControl1.Controls.Add(gridControl1);
this.Controls.Add(splitterControl1);
```

### Pattern 7: Hide Scrollbars

**Use case:** Minimal UI without scrollbars

```csharp
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;
splitterControl1.ShowHorizontalScrollBar = false;
splitterControl1.ShowVerticalScrollBar = false;

GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

splitterControl1.Controls.Add(gridControl1);
this.Controls.Add(splitterControl1);
```

## Key Properties

### SplitterControl Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `SplitBars` | `DynamicSplitBars` | Controls split direction (None, SplitColumns, SplitRows, Both) | None |
| `HSplitPos` | `int` | Horizontal split position percentage (0-100) | 50 |
| `VSplitPos` | `int` | Vertical split position percentage (0-100) | 50 |
| `GridOfficeScrollBars` | `OfficeScrollBars` | Scrollbar style (Metro, Office2007, Office2010, None) | None |
| `ShowHorizontalScrollBar` | `bool` | Shows/hides horizontal scrollbar | true |
| `ShowVerticalScrollBar` | `bool` | Shows/hides vertical scrollbar | true |
| `ShowSizeGrip` | `bool` | Shows/hides sizing grip indicator | true |
| `ShowToolTips` | `bool` | Shows tooltips on splitter bars | false |

### DynamicSplitBars Enum Values

- `None` - No splitting enabled
- `SplitRows` - Horizontal split (top-bottom)
- `SplitColumns` - Vertical split (left-right)
- `Both` - Both horizontal and vertical splits

### OfficeScrollBars Enum Values

- `None` - No custom scrollbar style
- `Metro` - Modern Metro style scrollbars
- `Office2007` - Office 2007 style scrollbars
- `Office2010` - Office 2010 style scrollbars
- `Office2016` - Office 2016 style scrollbars

## Child Control Integration

### Overview

SplitterControl is a container control that supports hosting any Syncfusion or framework control. Child controls are added directly to the `Controls` collection, allowing you to create split views with grids, charts, text editors, or any custom controls.

### Integration Rules

**Rule 1: Direct Addition**
- Add child controls directly to `SplitterControl.Controls` collection
- No intermediate panels or containers required
- SplitterControl automatically manages panes when split bars are enabled

**Rule 2: Control-Specific Properties**
- **For GridControl**: Set `FillSplitterPane = true` property
- **For SfDataGrid**: Use `Dock = DockStyle.Fill` property
- **For other controls**: Apply standard WinForms layout properties (Dock, Anchor, Size)

**Rule 3: Split Configuration First**
- Configure `SplitBars` property before adding child controls
- Set split position properties (`HSplitPos`, `VSplitPos`) if needed
- Child controls will automatically appear in split panes

**Rule 4: NuGet Package Requirements**
- Install `Syncfusion.Shared.Base` for SplitterControl
- Install child control's NuGet package (reference child control's skill file)
- Verify all assemblies are correctly referenced

### Adding Controls to SplitterControl

```csharp
// Step 1: Create and configure SplitterControl
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;

// Step 2: Create and configure child control
GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true; // Required for GridControl
gridControl1.RowCount = 50;
gridControl1.ColCount = 10;

// Step 3: Add child control to SplitterControl
splitterControl1.Controls.Add(gridControl1);

// Step 4: Add SplitterControl to form
this.Controls.Add(splitterControl1);
```

### NuGet Package Installation

**Required packages:**
- `Syncfusion.Shared.Base` - For SplitterControl
- Child control package - Reference the child control's skill file at `syncfusion-winforms-{control-name}/references/getting-started.md`

**Installation (preferred):**

To ensure the latest package version is used during restore, add package references in your project file with `Version="*"`:

```xml
<PackageReference Include="Syncfusion.Shared.Base" Version="*" />
<PackageReference Include="Syncfusion.Grid.Windows" Version="*" />       <!-- For GridControl -->
<PackageReference Include="Syncfusion.SfDataGrid.WinForms" Version="*" />  <!-- For SfDataGrid -->
```

Or use `dotnet add package` / `Install-Package` to install the packages (these install the latest stable by default):

```powershell
dotnet add package Syncfusion.Shared.Base
dotnet add package Syncfusion.Grid.Windows
dotnet add package Syncfusion.SfDataGrid.WinForms
```

See [references/getting-started.md](references/getting-started.md) and the child control's getting-started guide for detailed installation steps.

### Integration Examples

#### Example 1: SplitterControl with GridControl

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

// Configure splitter
SplitterControl splitter = new SplitterControl();
splitter.Dock = DockStyle.Fill;
splitter.SplitBars = DynamicSplitBars.Both;

// Add GridControl
GridControl grid = new GridControl();
grid.FillSplitterPane = true;
grid.RowCount = 100;
grid.ColCount = 20;

splitter.Controls.Add(grid);
this.Controls.Add(splitter);
```

#### Example 2: SplitterControl with SfDataGrid

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.DataGrid;

// Configure splitter
SplitterControl splitter = new SplitterControl();
splitter.Dock = DockStyle.Fill;
splitter.SplitBars = DynamicSplitBars.SplitColumns;

// Add SfDataGrid
SfDataGrid dataGrid = new SfDataGrid();
dataGrid.Dock = DockStyle.Fill;
dataGrid.AutoGenerateColumns = true;

splitter.Controls.Add(dataGrid);
this.Controls.Add(splitter);
```

#### Example 3: SplitterControl with Framework Controls

```csharp
using Syncfusion.Windows.Forms;

// Configure splitter
SplitterControl splitter = new SplitterControl();
splitter.Dock = DockStyle.Fill;
splitter.SplitBars = DynamicSplitBars.SplitRows;

// Add RichTextBox (framework control)
RichTextBox textBox = new RichTextBox();
textBox.Dock = DockStyle.Fill;

splitter.Controls.Add(textBox);
this.Controls.Add(splitter);
```

### Related Skills

When implementing SplitterControl with child controls, you must reference the child control's skill file for proper implementation.

**How to Find Child Control Skills:**

1. **Identify the child control** requested by the user (e.g., GridControl, SfDataGrid, Chart)
2. **Navigate to the skills folder** using the pattern: `syncfusion-winforms-{control-name}`
3. **Read the SKILL.md file** for that control to understand:
   - Proper initialization and configuration
   - Required NuGet packages
   - Control-specific properties and methods
   - Common patterns and best practices

**Example Skill File Locations:**

```
skills/syncfusion-winforms-grid-control/SKILL.md
```

**Implementation Workflow:**

1. Read child control's `SKILL.md` → Understand control basics
2. Read child control's `references/getting-started.md` → Get NuGet package info
3. Combine child control initialization with SplitterControl patterns (see Common Patterns section)
4. Apply control-specific properties (FillSplitterPane for GridControl, Dock for others)

**Note:** Always read the child control's SKILL.md file when user requests integration with SplitterControl. Each control has unique initialization requirements, properties, and configuration needs.

## Common Use Cases

### Use Case 1: Excel-Style Worksheet Viewer
Create split views like Excel's freeze panes to view headers while scrolling data.

**Implementation:** Pattern 4 (Both directions) with GridControl

### Use Case 2: Document Comparison
Compare two sections of the same document side-by-side.

**Implementation:** Pattern 3 (Vertical split) with appropriate child control

### Use Case 3: Code Editor Split View
View different parts of the same file simultaneously.

**Implementation:** Pattern 2 or 3 (Horizontal or Vertical split)

### Use Case 4: Data Analysis Dashboard
View raw data, summaries, and charts simultaneously. Use appropriate child controls based on requirements.

**Implementation:** Pattern 4 (Both directions) with child controls

### Use Case 5: Financial Spreadsheet
Split view for income, expenses, and summary with cross-references. Reference GridControl skill for specific implementation.

**Implementation:** Pattern 1 or 4 with GridControl

## Next Steps

- For SplitterControl installation and setup: Read `references/getting-started.md`
- For SplitterControl split behavior details: Read `references/split-behavior.md`
- For SplitterControl scrollbar customization: Read `references/scrollbar-customization.md`
- For SplitterControl child control integration: Read `references/adding-child-controls.md`
- For child control integration (GridControl, SfDataGrid, or any control):
  - Reference the child control's `references/getting-started.md` file in the skills folder for NuGet package installation
  - Check the child control's reference files for proper configuration
  - Ensure child control is properly configured before adding to SplitterControl
