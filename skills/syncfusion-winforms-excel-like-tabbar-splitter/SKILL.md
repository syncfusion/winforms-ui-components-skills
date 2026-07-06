---
name: syncfusion-winforms-excel-like-tabbar-splitter
description: Guide for implementing Syncfusion TabBarSplitterControl in Windows Forms applications for creating workbook-like tabbed interfaces. Use this skill when implementing Excel-like tabs, tabbed pages with splitters, or multiple sheet interfaces in Windows Forms. Covers NuGet installation, TabBarPage management, programmatic creation, and child control integration.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  platform: "Windows Forms"
---

# Implementing TabBarSplitterControl

Guide for implementing Syncfusion's TabBarSplitterControl in Windows Forms applications. This control creates tabbed pages with dynamic splitters, providing a workbook-like appearance ideal for GridControl integration and multi-sheet interfaces.

## When to Use This Skill

Use TabBarSplitterControl when you need to:
- Create tabbed interfaces with dynamic splitters in Windows Forms
- Build workbook-like UI layouts with multiple pages
- Integrate GridControl with tabbed pages
- Implement multi-sheet data views
- Create Excel-like tabbed interfaces
- Organize multiple data grids in separate pages
- Support formula cells and cross-reference sheets
- Build applications requiring page-based navigation with controls

## Component Overview

**TabBarSplitterControl** enables creation of tabbed pages with dynamic splitters. When used with GridControl, it provides a workbook-like appearance where each tab can contain separate grid instances.

**Key Features:**
- Multiple TabBarPage support
- Dynamic splitter functionality
- GridControl integration
- Workbook-like appearance
- Designer and programmatic creation
- Controls collection per page
- Page management and organization

**Assembly:** `Syncfusion.Shared.Base`

**Namespace:** `Syncfusion.Windows.Forms`

**Key Classes:**
- `TabBarSplitterControl` - Main container control
- `TabBarPage` - Individual page within the control

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.Shared.Base for TabBarSplitterControl)
- Child control NuGet packages (reference child control's skill file in skills folder)
- Assembly and namespace references
- Basic control overview and purpose
- When to use TabBarSplitterControl
- Child control integration basics (GridControl, SfDataGrid, or any Syncfusion/framework control)
- Designer availability note

### Programmatic Implementation
📄 **Read:** [references/programmatic-implementation.md](references/programmatic-implementation.md)
- Creating TabBarSplitterControl in code
- Initializing and configuring TabBarPages
- Adding controls to pages programmatically
- Setting page properties (Text, Controls)
- Multiple page scenarios
- C# and VB.NET implementation examples
- GridControl integration patterns
- Common configuration patterns

## Quick Start

### Installation

**TabBarSplitterControl NuGet Package:**

Install the Syncfusion.Shared.Base NuGet package (latest version):

```powershell
Install-Package Syncfusion.Shared.Base -Version *
```

Or via .NET CLI:
```bash
dotnet add package Syncfusion.Shared.Base --version *
```

**Note:** Using `*` ensures the latest version is installed during restore.

**Child Control NuGet Packages:**

When using TabBarSplitterControl with child controls (GridControl, SfDataGrid, or any Syncfusion/framework control), install the required NuGet packages for those child controls. Reference the child control's `references/getting-started.md` file in the skills folder for specific NuGet package installation instructions.

**Examples:**
- For **GridControl**: Check `syncfusion-winforms-grid-control/references/getting-started.md` for NuGet installation
- For **SfDataGrid**: Check `syncfusion-winforms-datagrid/references/getting-started.md` for NuGet installation
- For any other control: Check `syncfusion-winforms-{control-name}/references/getting-started.md` for NuGet installation

### Basic Implementation (C#)

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

// Create TabBarSplitterControl
TabBarSplitterControl tabBarSplitter = new TabBarSplitterControl();

// Create TabBarPages
TabBarPage page1 = new TabBarPage();
page1.Text = "Sheet1";

TabBarPage page2 = new TabBarPage();
page2.Text = "Sheet2";

// Add GridControls to pages
page1.Controls.Add(new GridControl());
page2.Controls.Add(new GridControl());

// Add pages to TabBarSplitterControl
tabBarSplitter.Controls.Add(page1);
tabBarSplitter.Controls.Add(page2);

// Add to form
this.Controls.Add(tabBarSplitter);
```

### Basic Implementation (VB.NET)

```vb
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Grid

' Create TabBarPages
Dim page1 As New TabBarPage()
page1.Text = "Sheet1"

Dim page2 As New TabBarPage()
page2.Text = "Sheet2"

' Add GridControls to pages
page1.Controls.Add(New GridControl())
page2.Controls.Add(New GridControl())

' Add pages to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(page1)
tabBarSplitterControl1.Controls.Add(page2)
```

## Common Patterns

### Pattern 1: Creating Multiple Named Pages

```csharp
// Initialize TabBarSplitterControl
TabBarSplitterControl tabBarSplitter = new TabBarSplitterControl();

// Create multiple pages with descriptive names
TabBarPage income = new TabBarPage { Text = "Income" };
TabBarPage expenses = new TabBarPage { Text = "Expenses" };
TabBarPage summary = new TabBarPage { Text = "Summary" };

// Add GridControl to each page
income.Controls.Add(new GridControl());
expenses.Controls.Add(new GridControl());
summary.Controls.Add(new GridControl());

// Add all pages to control
tabBarSplitter.Controls.Add(income);
tabBarSplitter.Controls.Add(expenses);
tabBarSplitter.Controls.Add(summary);

// Add to form
this.Controls.Add(tabBarSplitter);
```

### Pattern 2: Using Existing GridControl Instances

```csharp
// Assume you have pre-configured GridControls
GridControl gridIncome = new GridControl();
GridControl gridExpenses = new GridControl();

// Configure grids (add data, formulas, etc.)
// ... grid configuration code ...

// Create pages
TabBarPage incomePage = new TabBarPage { Text = "Income" };
TabBarPage expensesPage = new TabBarPage { Text = "Expenses" };

// Add existing grids to pages
incomePage.Controls.Add(gridIncome);
expensesPage.Controls.Add(gridExpenses);

// Add pages to TabBarSplitterControl
tabBarSplitter.Controls.Add(incomePage);
tabBarSplitter.Controls.Add(expensesPage);
```

### Pattern 3: VB.NET Implementation

```vb
' Create TabBarPages
Private IncomePage As New Syncfusion.Windows.Forms.TabBarPage()
Private ExpensesPage As New Syncfusion.Windows.Forms.TabBarPage()

' Configure pages
IncomePage.Text = "Income"
IncomePage.Controls.Add(Me.gridControl1)

ExpensesPage.Text = "Expenses"
ExpensesPage.Controls.Add(Me.gridControl2)

' Add to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(Me.IncomePage)
tabBarSplitterControl1.Controls.Add(Me.ExpensesPage)
```

## Key Properties and Classes

### TabBarSplitterControl

**Purpose:** Main container control that hosts TabBarPages

**Key Members:**
- `Controls` - Collection of TabBarPage objects
- Standard WinForms control properties (Location, Size, Dock, etc.)

### TabBarPage

**Purpose:** Individual page within TabBarSplitterControl

**Key Properties:**
- `Text` - Page tab label
- `Controls` - Collection of controls within the page (typically GridControl)
- Standard WinForms control properties

## Common Use Cases

### 1. Financial Data Workbook
Create separate sheets for Income, Expenses, and Summary with GridControls or SfDataGrids for each. Reference child control skill files for specific NuGet requirements.

### 2. Multi-Sheet Reports
Organize different report views in separate tabs with cross-references between sheets. Use appropriate child controls based on customer requirements.

### 3. Data Analysis Interface
Display multiple data grids with formulas that reference data across different pages. Check GridControl or SfDataGrid skill files for implementation details.

### 4. Spreadsheet-Like Applications
Build Excel-like interfaces where each tab contains a separate data grid or any Syncfusion/framework control.

### 5. Dashboard with Multiple Views
Create tabbed interface where each page shows different data visualization, grid view, or custom controls. Reference respective skill files for child control implementation.

## Designer Integration Note

TabBarSplitterControl can also be added via Visual Studio Designer:
- Drag and drop from toolbox
- Use TabBarPageCollectionEditor for page management
- Edit option available in designer mode

**Note:** Claude cannot perform manual designer operations. For programmatic implementation, use the patterns and examples provided in this skill.

## Related Documentation

For additional properties and methods of TabBarSplitterControl, refer to the Syncfusion documentation for the Splitter overview.

## Next Steps

- For TabBarSplitterControl installation and setup: Read `references/getting-started.md`
- For TabBarSplitterControl code implementation: Read `references/programmatic-implementation.md`
- For child control integration (GridControl, SfDataGrid, or any control):
  - Reference the child control's `references/getting-started.md` file in the skills folder for NuGet package installation
  - Check the child control's reference files for proper configuration
  - Ensure child control is properly configured before adding to TabBarPages
