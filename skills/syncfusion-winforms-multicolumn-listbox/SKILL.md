---
name: syncfusion-winforms-multicolumn-listbox
description: Guide for implementing Syncfusion GridListControl in Windows Forms applications for multi-column list functionality. Use this skill when implementing multi-column listboxes, data-bound lists with headers, or tabular list displays in Windows Forms. Covers GridListControl setup, data binding, selection modes, appearance customization, and ComboBoxBase integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing GridList Control

## When to Use This Skill

Use this skill when the user needs to:
- Display a list of items in multi-column format with headers
- Create data-bound list controls in Windows Forms applications
- Implement single or multiple selection in list views
- Build multi-column dropdown controls with ComboBoxBase
- Customize list appearance (colors, grid lines, headers, backgrounds)
- Handle large datasets with virtual mode
- Add tooltips, formulas, or touch support to list controls
- Migrate from standard ListBox to advanced multi-column list view

## Component Overview

The **GridListControl** is a multi-column list view component for Windows Forms that provides rich data display capabilities beyond the standard ListBox control. It combines grid-like functionality with list behavior, offering powerful features for desktop applications.

**Key Capabilities:**
- Multi-column display with customizable headers
- Data binding to any data source (ArrayList, DataTable, custom objects)
- Three selection modes: single, multi-simple, multi-extended
- Extensive styling (colors, grid lines, backgrounds, images)
- ComboBoxBase integration for dropdown scenarios
- Virtual mode for high-performance large datasets
- Formula support for calculated values
- Touch and keyboard navigation
- Column/row resizing
- Cell-level tooltips

## Key Features

- **Data Populating** - Bind to any data source including databases, collections, and custom objects
- **Styling** - Customize cell styles, colors, fonts, grid lines, and backgrounds
- **Selection** - Single and multiple row selections with keyboard support
- **Tooltip** - Display hover text on cells
- **Resizing** - Auto-resize columns and rows based on content
- **Visual Styles** - Multiple built-in themes and custom styling
- **Virtual Mode** - Load data on-demand for optimal performance
- **Formulas** - Support algebraic and arithmetic expressions
- **Drop-down** - Combine with ComboBoxBase for multi-column dropdowns
- **Touch Support** - Selection and swiping on touch devices

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and dependencies
- Adding GridListControl through designer (drag-drop, SqlDataAdapter wizard)
- Adding GridListControl through code (programmatic setup)
- Basic configuration properties (DataSource, MultiColumn, ShowColumnHeader)
- Initial data binding examples
- Designer vs code approach comparison

### Data Binding and Selection Modes
📄 **Read:** [references/data-binding-selection.md](references/data-binding-selection.md)
- Data binding overview and DataSource property
- Binding to ArrayList and custom objects
- Database binding with DataTable/DataSet
- Selection mode types (One, MultiSimple, MultiExtended)
- SelectionMode property configuration
- Keyboard navigation in multi-extended mode
- Getting selected items (SelectedIndex, SelectedItem)

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- TransparentBackground property
- Grid line display (DisplayVertLines, DisplayHorzLines)
- GridLineColor customization
- Header appearance (Buttons3D, HeaderBackColor, HeaderTextColor)
- Control BackColor and BackgroundImage
- Visual styles and theming
- Touch support configuration
- Column and row resizing
- Tooltip configuration

### ComboBoxBase Integration
📄 **Read:** [references/combobox-integration.md](references/combobox-integration.md)
- Combining GridListControl with ComboBoxBase
- ListControl property setup
- Creating multi-column dropdown effects
- DropDownWidth configuration
- Use cases for dropdown lists
- Integration code examples

## Quick Start Example

### Basic Multi-Column List with Data Binding

```csharp
using Syncfusion.Windows.Forms.Grid;
using System.Collections;

// Create data source
ArrayList states = new ArrayList();
states.Add(new StateInfo { LongName = "California", ShortName = "CA" });
states.Add(new StateInfo { LongName = "Texas", ShortName = "TX" });
states.Add(new StateInfo { LongName = "New York", ShortName = "NY" });

// Configure GridListControl
gridListControl1.DataSource = states;
gridListControl1.MultiColumn = true;
gridListControl1.ShowColumnHeader = true;
gridListControl1.SelectionMode = SelectionMode.One;
gridListControl1.FillLastColumn = true;

// Custom class for data
public class StateInfo
{
    public string LongName { get; set; }
    public string ShortName { get; set; }
}
```

### Multi-Column Dropdown with ComboBoxBase

```csharp
// Setup GridListControl
gridListControl1.DataSource = dataSource;
gridListControl1.MultiColumn = true;

// Attach to ComboBoxBase for dropdown
comboBoxBase1.ListControl = gridListControl1;
```

## Common Patterns

### Designer-Based Setup
1. Drag SqlDataAdapter from toolbox → Configure database connection
2. Generate DataSet from adapter
3. Drag GridListControl to form
4. Set DataSource property to the DataSet
5. Configure display properties in Properties window

### Code-Based Setup
1. Create data source (ArrayList, List<T>, DataTable)
2. Initialize GridListControl
3. Set DataSource property
4. Configure MultiColumn, ShowColumnHeader
5. Set SelectionMode
6. Optionally customize appearance

### Virtual Mode for Large Datasets
```csharp
// Enable virtual mode for performance
gridListControl1.VirtualMode = true;

// Implement on-demand data loading
// (Refer to customization.md for details)
```

## Key Properties

### Data and Display
- **`DataSource`** - Sets the data source for the list (ArrayList, DataTable, IList)
- **`MultiColumn`** - Enables multi-column display (default: false)
- **`ShowColumnHeader`** - Shows/hides column headers (default: false)
- **`FillLastColumn`** - Makes last column fill remaining space (default: false)

### Selection
- **`SelectionMode`** - Controls selection behavior:
  - `SelectionMode.One` - Single row selection only
  - `SelectionMode.MultiSimple` - Multiple rows, click to toggle
  - `SelectionMode.MultiExtended` - Multiple rows with SHIFT/CTRL/arrows

### Appearance
- **`BackColor`** - Background color of the control
- **`TransparentBackground`** - Makes background transparent (default: false)
- **`HeaderBackColor`** - Background color of column headers
- **`HeaderTextColor`** - Text color of column headers
- **`BackgroundImage`** - Background image for the control

### Grid Lines
- **`Properties.DisplayHorzLines`** - Shows horizontal grid lines
- **`Properties.DisplayVertLines`** - Shows vertical grid lines
- **`Grid.Properties.GridLineColor`** - Color of grid lines
- **`Properties.Buttons3D`** - Renders 3D-style headers

### Advanced
- **`VirtualMode`** - Enables on-demand data loading for performance
- **`ImageList`** - Associates images with list items
- **`ToolTip`** - Configures cell tooltips

## Common Use Cases

### 1. Database Record Viewer
Display database records in a multi-column list with headers, allowing users to browse and select records.

**When to use:** Data browsing, record selection, master-detail views

### 2. Multi-Column Dropdown Selector
Combine with ComboBoxBase to create dropdown lists showing multiple columns (e.g., customer name + ID + city).

**When to use:** Complex selection scenarios, lookup fields, data-rich pickers

### 3. Categorized Item Lists
Display items with multiple attributes (name, category, price, status) in a structured list format.

**When to use:** Product catalogs, inventory management, order lists

### 4. Selection Interfaces
Build interfaces requiring single or multiple item selection with visual feedback.

**When to use:** Batch operations, multi-select forms, checklist UIs

### 5. Touch-Enabled Lists
Create touch-friendly list controls for tablet or touch-screen applications.

**When to use:** Kiosk applications, tablet apps, touch-optimized UIs

### 6. Large Dataset Browsers
Use virtual mode to efficiently display and navigate datasets with thousands of items.

**When to use:** Log viewers, large database tables, high-volume data

## Decision Guide

**Choose GridListControl when you need:**
- ✅ Multi-column list display
- ✅ Column headers
- ✅ Data binding to objects/databases
- ✅ Advanced selection modes
- ✅ Grid-like styling
- ✅ ComboBoxBase dropdown integration
- ✅ Virtual mode for performance

**Use standard ListBox when:**
- ❌ Simple single-column list is sufficient
- ❌ No headers needed
- ❌ Minimal styling requirements
- ❌ Simple data display only

## Best Practices

1. **Performance**
   - Use virtual mode for datasets with 1000+ items
   - Disable grid lines for faster rendering if not needed
   - Use BeginUpdate/EndUpdate when changing multiple properties

2. **Data Binding**
   - Validate data source before binding
   - Use strongly-typed collections (List<T>) for type safety
   - Handle data changes appropriately (refresh, rebind)

3. **Selection**
   - Choose appropriate SelectionMode for user workflow
   - MultiExtended is most flexible but requires user training
   - One is simplest for single-selection scenarios

4. **Styling**
   - Apply consistent visual theme across application
   - Use TransparentBackground carefully (performance impact)
   - Test custom colors for readability

5. **Integration**
   - When using with ComboBoxBase, configure GridList first
   - Ensure data source is ready before binding
   - Handle selection events in parent form, not ComboBoxBase

## Troubleshooting

**Common Issues:**

**GridListControl not showing data**
- Verify DataSource is set and not null
- Check if MultiColumn = true for multi-column display
- Ensure data source has items

**Headers not visible**
- Set ShowColumnHeader = true
- Check if header colors match background

**Selection not working**
- Verify SelectionMode is not None
- Check if control is enabled
- Ensure focus is on control

**Performance issues**
- Enable virtual mode for large datasets
- Disable unnecessary features (grid lines, tooltips)
- Use BeginUpdate/EndUpdate for bulk changes

For detailed troubleshooting and advanced scenarios, refer to the reference files.

## Next Steps

Start with **[Getting Started](references/getting-started.md)** to install and configure the GridListControl, then explore specific features through the navigation guide above based on your application requirements.