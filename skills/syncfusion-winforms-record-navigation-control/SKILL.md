---
name: syncfusion-winforms-record-navigation-control
description: Guide for implementing Syncfusion GridRecordNavigationControl in Windows Forms applications for record navigation functionality. Use this skill when implementing Microsoft Access-like navigation bars, first/last/previous/next navigation for grids, or record browsing in Windows Forms. Covers integration with GridControl, GridDataBoundGrid, and GridGroupingControl.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Grid Record Navigation Controls

A comprehensive guide for implementing the Syncfusion GridRecordNavigationControl in Windows Forms applications. This control provides a Microsoft Access-like navigation bar for navigating between records in grid controls.

## When to Use This Skill

Use the GridRecordNavigationControl when you need to:
- Add navigation controls to browse through grid records
- Implement Microsoft Access-like record navigation UI
- Provide first, last, previous, next navigation functionality
- Navigate large datasets in GridControl or GridDataBoundGrid
- Integrate navigation bar with GridGroupingControl
- Create data-driven applications with intuitive record browsing
- Display record count and current position to users

## Component Overview

The GridRecordNavigationControl allows users to navigate between records using a navigation bar positioned at the bottom of a grid. It provides built-in support for navigating to the first, last, previous, and next record with a familiar Microsoft Access-style interface.

**Key Features:**
- **Built-in Navigation Methods** - MoveFirst, MoveLast, MoveNext, MovePrevious
- **Localization Support** - Customize navigation bar text and labels
- **Visual Styles** - Default, Metro, Office2016 themes
- **GridGroupingControl Integration** - ShowNavigationBar property for easy integration
- **Programmatic Control** - Navigate records dynamically through code
- **Record Display** - Shows current record and total record count

## Documentation and Navigation Guide

### Getting Started and Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment requirements (Syncfusion.Grid.Windows, Syncfusion.Shared.Base)
- Creating GridRecordNavigationControl through Visual Studio Designer
- Creating GridRecordNavigationControl programmatically through code
- Adding child grid controls (GridControl, GridDataBoundGrid)
- Basic configuration and initialization
- Complete setup examples in C# and VB.NET

### Navigation Methods and Functionality
📄 **Read:** [references/navigation-methods.md](references/navigation-methods.md)
- MoveFirst() - Navigate to the first record
- MoveLast() - Navigate to the last record
- MoveNext() - Navigate to the next record
- MovePrevious() - Navigate to the previous record
- Use case scenarios for large datasets
- Programmatic navigation examples
- Navigation method reference with code samples

### Styling and Themes
📄 **Read:** [references/styling.md](references/styling.md)
- Visual styles: Default, Metro, Office2016
- Style property configuration
- Office2016 color schemes (Black, Colorful, DarkGray, White)
- GridOfficeScrollBars integration
- Theme examples with screenshots
- Appearance customization

### GridGroupingControl Integration
📄 **Read:** [references/gridgrouping-integration.md](references/gridgrouping-integration.md)
- ShowNavigationBar property for GridGroupingControl
- RecordNavigationBar property access
- Built-in navigation support integration
- Enabling navigation bar for grouped data
- Complete integration examples
- Record navigation in grouped grids

## Quick Start Example

### Through Designer (Recommended)

```csharp
// Step 1: Drag GridRecordNavigationControl from toolbox to form
// Step 2: Position and size the control
// Step 3: Drag GridDataBoundGrid into the GridRecordNavigationControl
// Step 4: Set GridDataBoundGrid.DataSource property

// Designer generates this code:
this.recordNavigationControl1 = new Syncfusion.Windows.Forms.Grid.GridRecordNavigationControl();
this.gridDataBoundGrid1 = new Syncfusion.Windows.Forms.Grid.GridDataBoundGrid();

this.recordNavigationControl1.Controls.Add(this.gridDataBoundGrid1);
this.gridDataBoundGrid1.DataSource = myDataSource;
```

### Through Code

```csharp
using Syncfusion.Windows.Forms.Grid;
using System.Drawing;

// Create GridRecordNavigationControl
this.recordNavigationControl1 = new GridRecordNavigationControl();
this.recordNavigationControl1.Location = new Point(32, 48);
this.recordNavigationControl1.Size = new Size(520, 256);
this.recordNavigationControl1.MaxRecord = 1000;
this.recordNavigationControl1.MaxLabel = "of 1000";
this.recordNavigationControl1.NavigationBarWidth = 237;

// Create child grid
this.gridControl1 = new GridControl();
this.gridControl1.RowCount = 1000;
this.gridControl1.ColCount = 16;
this.gridControl1.NumberedRowHeaders = false;

// Add grid to navigation control
this.recordNavigationControl1.Controls.Add(this.gridControl1);

// Add to form
this.Controls.Add(this.recordNavigationControl1);
```

## Common Patterns

### Pattern 1: Data-Bound Grid with Navigation

```csharp
using Syncfusion.Windows.Forms.Grid;
using System.Data;

// Create navigation control
var navControl = new GridRecordNavigationControl();
navControl.Location = new Point(20, 20);
navControl.Size = new Size(600, 400);

// Create data-bound grid
var dataGrid = new GridDataBoundGrid();
dataGrid.DataSource = GetDataTable(); // Your data source

// Add grid to navigation control
navControl.Controls.Add(dataGrid);

// Configure navigation
navControl.MaxRecord = dataGrid.Model.RowCount;
navControl.MaxLabel = $"of {dataGrid.Model.RowCount}";

this.Controls.Add(navControl);
```

### Pattern 2: Programmatic Navigation

```csharp
private RecordNavigationControl recordNavigationControl;
// Navigate to first record
this.recordNavigationControl.NavigationBar.MoveFirst();

// Navigate to last record
this.recordNavigationControl.NavigationBar.MoveLast();

// Navigate to next record
this.recordNavigationControl.NavigationBar.MoveNext();

// Navigate to previous record
this.recordNavigationControl.NavigationBar.MovePrevious();
```

### Pattern 3: Styled Navigation Control

```csharp
using Syncfusion.Windows.Forms;

// Apply Metro theme
this.recordNavigationControl1.Style = Appearance.Metro;

// Apply Office2016 theme with Black color scheme
this.recordNavigationControl1.Style = Appearance.Office2016;
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.Black;
```

### Pattern 4: GridGroupingControl with Navigation

```csharp
using Syncfusion.Windows.Forms.Grid.Grouping;

// Enable navigation bar on GridGroupingControl
this.gridGroupingControl1.ShowNavigationBar = true;

// Access navigation methods through RecordNavigationBar property
this.gridGroupingControl1.RecordNavigationBar.MoveFirst();
this.gridGroupingControl1.RecordNavigationBar.MoveNext();
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `MaxRecord` | int | Total number of records in the dataset |
| `MaxLabel` | string | Label showing total record count (e.g., "of 1000") |
| `NavigationBarWidth` | int | Width of the navigation bar in pixels |
| `SplitBars` | DynamicSplitBars | Split bar configuration (None, Both, Horizontal, Vertical) |
| `Style` | Appearance | Visual style (Default, Metro, Office2016) |
| `Office2016ScrollBarsColorScheme` | ScrollBarOffice2016ColorScheme | Office2016 color scheme |
| `GridOfficeScrollBars` | OfficeScrollBars | Office scrollbar style |
| `ShowNavigationBar` | bool | (GridGroupingControl) Shows/hides navigation bar |

## Key Methods

| Method | Description |
|--------|-------------|
| `MoveFirst()` | Navigate to the first record |
| `MoveLast()` | Navigate to the last record |
| `MoveNext()` | Navigate to the next record |
| `MovePrevious()` | Navigate to the previous record |

## Common Use Cases

### Use Case 1: Database Viewer Application
Create a data viewer that allows users to browse through database records one at a time, similar to Microsoft Access forms.

### Use Case 2: Record Editor
Build an editor interface where users can navigate between records while editing data, with clear indication of current position.

### Use Case 3: Large Dataset Navigation
Enable efficient navigation through large datasets (1000+ records) without loading all records into view at once.

### Use Case 4: Grouped Data Navigation
Navigate through grouped records in GridGroupingControl while maintaining group structure.

## Required Assemblies

To use GridRecordNavigationControl, add these assembly references:

1. **Syncfusion.Grid.Windows.dll** - Core grid functionality and navigation control
2. **Syncfusion.Shared.Base.dll** - Shared styles, editors, and base components

## Implementation Checklist

- [ ] Add required assembly references
- [ ] Create GridRecordNavigationControl (designer or code)
- [ ] Add child grid control (GridControl or GridDataBoundGrid)
- [ ] Configure MaxRecord and MaxLabel properties
- [ ] Set data source if using data-bound grid
- [ ] Apply visual style/theme (optional)
- [ ] Test navigation methods (first, last, next, previous)
- [ ] Handle any custom navigation events (optional)

## Best Practices

1. **Designer First**: Use Visual Studio Designer for initial setup - it generates correct initialization code
2. **Drop Order**: When using designer, drop navigation control first, then drop grid INTO the navigation control (not onto the form)
3. **Set MaxRecord**: Always set MaxRecord property to match your dataset size for accurate navigation
4. **Update MaxLabel**: Keep MaxLabel synchronized with data changes
5. **Theme Consistency**: Apply the same visual style across all Syncfusion controls in your application
6. **Assembly References**: Verify both required assemblies are referenced before using the control

## Troubleshooting

**Navigation doesn't work:**
- Verify child grid is added to navigation control's Controls collection
- Check that MaxRecord is set correctly
- Ensure grid has data source or populated rows

**Designer issues:**
- Drop grid control INTO navigation control, not onto the form
- If controls aren't nested properly, recreate in correct order

**Styling not applied:**
- Verify Office2016ScrollBarsColorScheme is set when using Office2016 style
- Check that GridOfficeScrollBars matches the Style property

