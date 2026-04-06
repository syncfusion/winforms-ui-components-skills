# Adding Child Controls to SplitterControl

This guide covers how to add child controls (such as GridControl, SfDataGrid, or any Syncfusion/framework control) to SplitterControl.

## Overview

SplitterControl is a container control that can host other Windows Forms controls. Child controls are added directly to the `Controls` collection.

## Basic Pattern

The simplest way to add a child control:

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

// Create SplitterControl
SplitterControl splitterControl1 = new SplitterControl();
splitterControl1.Dock = DockStyle.Fill;
splitterControl1.SplitBars = DynamicSplitBars.Both;

// Create child control
GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true;

// Add child control directly to SplitterControl
splitterControl1.Controls.Add(gridControl1);

// Add to form
this.Controls.Add(splitterControl1);
```

## Adding GridControl

When adding GridControl to SplitterControl, set the `FillSplitterPane` property:

```csharp
GridControl gridControl1 = new GridControl();
gridControl1.FillSplitterPane = true; // Important for proper display in split panes
gridControl1.RowCount = 50;
gridControl1.ColCount = 10;

splitterControl1.Controls.Add(gridControl1);
```

**Key Property:**
- `FillSplitterPane = true` - Ensures GridControl properly fills the splitter panes

## Adding SfDataGrid

When adding SfDataGrid to SplitterControl, use standard docking:

```csharp
using Syncfusion.WinForms.DataGrid;

SfDataGrid sfDataGrid1 = new SfDataGrid();
sfDataGrid1.Dock = DockStyle.Fill;
sfDataGrid1.AutoGenerateColumns = true;

splitterControl1.Controls.Add(sfDataGrid1);
```

**Key Property:**
- `Dock = DockStyle.Fill` - Standard WinForms docking for SfDataGrid

## Adding Other Controls

For any other Syncfusion or framework control:

```csharp
// Example with a standard control
RichTextBox richTextBox1 = new RichTextBox();
richTextBox1.Dock = DockStyle.Fill;

splitterControl1.Controls.Add(richTextBox1);
```

Use appropriate docking or sizing properties based on the control type.

## NuGet Package Installation

**Required Packages:**
1. `Syncfusion.Shared.Base` - For SplitterControl
2. Child control package - See child control's `getting-started.md`

```powershell
Install-Package Syncfusion.Shared.Base
Install-Package Syncfusion.Grid.Windows          # For GridControl
Install-Package Syncfusion.SfDataGrid.WinForms   # For SfDataGrid
```

## Integration Pattern

1. Create SplitterControl and configure split behavior
2. Create child control with appropriate properties:
   - GridControl: Set `FillSplitterPane = true`
   - SfDataGrid: Set `Dock = DockStyle.Fill`
   - Other controls: Use standard WinForms properties
3. Add to Controls: `splitterControl1.Controls.Add(childControl)`
4. Add to form: `this.Controls.Add(splitterControl1)`

**For child control details**, reference: `syncfusion-winforms-{control-name}/SKILL.md`
