# Programmatic Implementation of TabBarSplitterControl

Complete guide for creating and configuring TabBarSplitterControl programmatically in C# and VB.NET. This guide covers all aspects of code-based implementation.

## Table of Contents
- [Basic Creation Pattern](#basic-creation-pattern)
- [Creating TabBarSplitterControl](#creating-tabbarsplittercontrol)
- [Creating and Adding TabBarPages](#creating-and-adding-tabbarpages)
- [Adding Controls to Pages](#adding-controls-to-pages)
- [Complete Examples](#complete-examples)
- [Multiple Page Scenarios](#multiple-page-scenarios)
- [GridControl Integration](#gridcontrol-integration)
- [Common Configuration Patterns](#common-configuration-patterns)

## Basic Creation Pattern

The general workflow for programmatic implementation:

1. **Create TabBarSplitterControl instance**
2. **Create TabBarPage instances** (one for each tab)
3. **Set TabBarPage properties** (Text, Name, etc.)
4. **Add controls to each TabBarPage** (GridControl or other controls)
5. **Add TabBarPages to TabBarSplitterControl** (Controls.Add)
6. **Add TabBarSplitterControl to form** (this.Controls.Add)

## Creating TabBarSplitterControl

### C# Implementation

```csharp
using Syncfusion.Windows.Forms;

// Create TabBarSplitterControl instance
TabBarSplitterControl tabBarSplitterControl1 = new TabBarSplitterControl();

// Optional: Set control properties
tabBarSplitterControl1.Dock = DockStyle.Fill;
tabBarSplitterControl1.Location = new Point(0, 0);
tabBarSplitterControl1.Size = new Size(800, 600);
```

### VB.NET Implementation

```vb
Imports Syncfusion.Windows.Forms

' Create TabBarSplitterControl instance
Dim tabBarSplitterControl1 As New TabBarSplitterControl()

' Optional: Set control properties
tabBarSplitterControl1.Dock = DockStyle.Fill
tabBarSplitterControl1.Location = New Point(0, 0)
tabBarSplitterControl1.Size = New Size(800, 600)
```

## Creating and Adding TabBarPages

### C# Implementation

```csharp
// Create TabBarPage instances
TabBarPage page1 = new TabBarPage();
TabBarPage page2 = new TabBarPage();

// Set page properties
page1.Text = "Sheet1";  // This appears on the tab
page2.Text = "Sheet2";

// Optional: Set additional properties
page1.Name = "page1";
page2.Name = "page2";
```

### VB.NET Implementation

```vb
' Create TabBarPage instances
Dim page1 As New TabBarPage()
Dim page2 As New TabBarPage()

' Set page properties
page1.Text = "Sheet1"  ' This appears on the tab
page2.Text = "Sheet2"

' Optional: Set additional properties
page1.Name = "page1"
page2.Name = "page2"
```

## Adding Controls to Pages

### Adding GridControl to TabBarPages

**C#:**
```csharp
using Syncfusion.Windows.Forms.Grid;

// Create GridControl instances
GridControl gridControl1 = new GridControl();
GridControl gridControl2 = new GridControl();

// Optional: Configure GridControls
gridControl1.Dock = DockStyle.Fill;
gridControl2.Dock = DockStyle.Fill;

// Add GridControls to TabBarPages
page1.Controls.Add(gridControl1);
page2.Controls.Add(gridControl2);
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Grid

' Create GridControl instances
Dim gridControl1 As New GridControl()
Dim gridControl2 As New GridControl()

' Optional: Configure GridControls
gridControl1.Dock = DockStyle.Fill
gridControl2.Dock = DockStyle.Fill

' Add GridControls to TabBarPages
page1.Controls.Add(gridControl1)
page2.Controls.Add(gridControl2)
```

### Adding Other Controls to Pages

TabBarPages can host any WinForms control:

```csharp
// Create any control
TextBox textBox = new TextBox();
Button button = new Button();
Panel panel = new Panel();

// Add to TabBarPage
page1.Controls.Add(textBox);
page1.Controls.Add(button);
page2.Controls.Add(panel);
```

## Complete Examples

### Example 1: Two-Page Workbook (C#)

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

public class MyForm : Form
{
    public MyForm()
    {
        // Initialize TabBarSplitterControl
        TabBarSplitterControl tabBarSplitterControl1 = new TabBarSplitterControl();
        tabBarSplitterControl1.Dock = DockStyle.Fill;

        // Create TabBarPages
        TabBarPage incomePage = new TabBarPage();
        TabBarPage expensesPage = new TabBarPage();

        incomePage.Text = "Income";
        expensesPage.Text = "Expenses";

        // Add GridControls to pages
        GridControl incomeGrid = new GridControl();
        incomeGrid.Dock = DockStyle.Fill;
        incomePage.Controls.Add(incomeGrid);

        GridControl expensesGrid = new GridControl();
        expensesGrid.Dock = DockStyle.Fill;
        expensesPage.Controls.Add(expensesGrid);

        // Add pages to TabBarSplitterControl
        tabBarSplitterControl1.Controls.Add(incomePage);
        tabBarSplitterControl1.Controls.Add(expensesPage);

        // Add TabBarSplitterControl to form
        this.Controls.Add(tabBarSplitterControl1);
    }
}
```

### Example 2: Two-Page Workbook (VB.NET)

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Grid

Public Class MyForm
    Inherits Form

    Private tabBarSplitterControl1 As TabBarSplitterControl
    Private IncomePage As TabBarPage
    Private ExpensesPage As TabBarPage
    Private gridControl1 As GridControl
    Private gridControl2 As GridControl

    Public Sub New()
        ' Initialize TabBarSplitterControl
        tabBarSplitterControl1 = New TabBarSplitterControl()
        tabBarSplitterControl1.Dock = DockStyle.Fill

        ' Create TabBarPages
        IncomePage = New TabBarPage()
        ExpensesPage = New TabBarPage()

        IncomePage.Text = "Income"
        ExpensesPage.Text = "Expenses"

        ' Create GridControls
        gridControl1 = New GridControl()
        gridControl1.Dock = DockStyle.Fill
        gridControl2 = New GridControl()
        gridControl2.Dock = DockStyle.Fill

        ' Add GridControls to pages
        IncomePage.Controls.Add(gridControl1)
        ExpensesPage.Controls.Add(gridControl2)

        ' Add pages to TabBarSplitterControl
        tabBarSplitterControl1.Controls.Add(IncomePage)
        tabBarSplitterControl1.Controls.Add(ExpensesPage)

        ' Add TabBarSplitterControl to form
        Me.Controls.Add(tabBarSplitterControl1)
    End Sub
End Class
```

### Example 3: Inline Creation Pattern (C#)

```csharp
// Initialize a TabBarSplitterControl
TabBarSplitterControl tabBarSplitterControl1 = new TabBarSplitterControl();

// Initialize required number of TabBarPages
TabBarPage Income = new TabBarPage();
TabBarPage Spent = new TabBarPage();

// Add a new GridControl in the TabBarPage named Income
Income.Text = "Income";
Income.Controls.Add(new GridControl());

// Add a new GridControl in the TabBarPage named Spent
Spent.Text = "Spent";
Spent.Controls.Add(new GridControl());

// Add the TabBarPages to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(Income);
tabBarSplitterControl1.Controls.Add(Spent);

// Add to form
this.Controls.Add(tabBarSplitterControl1);
```

### Example 4: Inline Creation Pattern (VB.NET)

```vb
' Create TabBarPage Controls
Private Income As New Syncfusion.Windows.Forms.TabBarPage()
Private Spent As New Syncfusion.Windows.Forms.TabBarPage()

' Add the gridcontrol1 for Income page
Income.Text = "Income"
Income.Controls.Add(Me.gridControl1)

' Add the gridcontrol2 for Spent page
Spent.Text = "Spent"
Spent.Controls.Add(Me.gridControl2)

' Add the TabBarPages to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(Me.Income)
tabBarSplitterControl1.Controls.Add(Me.Spent)
```

## Multiple Page Scenarios

### Creating Three or More Pages

**C# Example:**
```csharp
// Create multiple pages
TabBarPage page1 = new TabBarPage { Text = "Income" };
TabBarPage page2 = new TabBarPage { Text = "Expenses" };
TabBarPage page3 = new TabBarPage { Text = "Summary" };
TabBarPage page4 = new TabBarPage { Text = "Report" };

// Add controls to each page
page1.Controls.Add(new GridControl { Dock = DockStyle.Fill });
page2.Controls.Add(new GridControl { Dock = DockStyle.Fill });
page3.Controls.Add(new GridControl { Dock = DockStyle.Fill });
page4.Controls.Add(new GridControl { Dock = DockStyle.Fill });

// Add all pages to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(page1);
tabBarSplitterControl1.Controls.Add(page2);
tabBarSplitterControl1.Controls.Add(page3);
tabBarSplitterControl1.Controls.Add(page4);
```

**VB.NET Example:**
```vb
' Create multiple pages
Dim page1 As New TabBarPage With {.Text = "Income"}
Dim page2 As New TabBarPage With {.Text = "Expenses"}
Dim page3 As New TabBarPage With {.Text = "Summary"}
Dim page4 As New TabBarPage With {.Text = "Report"}

' Add controls to each page
page1.Controls.Add(New GridControl With {.Dock = DockStyle.Fill})
page2.Controls.Add(New GridControl With {.Dock = DockStyle.Fill})
page3.Controls.Add(New GridControl With {.Dock = DockStyle.Fill})
page4.Controls.Add(New GridControl With {.Dock = DockStyle.Fill})

' Add all pages to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(page1)
tabBarSplitterControl1.Controls.Add(page2)
tabBarSplitterControl1.Controls.Add(page3)
tabBarSplitterControl1.Controls.Add(page4)
```

### Using Arrays or Lists for Multiple Pages

**C#:**
```csharp
// Create pages using array
string[] pageNames = { "Q1", "Q2", "Q3", "Q4" };

foreach (string name in pageNames)
{
    TabBarPage page = new TabBarPage { Text = name };
    GridControl grid = new GridControl { Dock = DockStyle.Fill };
    page.Controls.Add(grid);
    tabBarSplitterControl1.Controls.Add(page);
}
```

## GridControl Integration

### Using Pre-Configured GridControl Instances

```csharp
// Create and configure GridControls separately
GridControl salesGrid = new GridControl();
salesGrid.Dock = DockStyle.Fill;
// Configure grid properties, data, formulas, etc.
salesGrid.RowCount = 100;
salesGrid.ColCount = 10;

GridControl inventoryGrid = new GridControl();
inventoryGrid.Dock = DockStyle.Fill;
// Configure grid properties
inventoryGrid.RowCount = 50;
inventoryGrid.ColCount = 8;

// Create pages and add pre-configured grids
TabBarPage salesPage = new TabBarPage { Text = "Sales" };
salesPage.Controls.Add(salesGrid);

TabBarPage inventoryPage = new TabBarPage { Text = "Inventory" };
inventoryPage.Controls.Add(inventoryGrid);

// Add to TabBarSplitterControl
tabBarSplitterControl1.Controls.Add(salesPage);
tabBarSplitterControl1.Controls.Add(inventoryPage);
```

### Formula and Cross-Reference Support

TabBarSplitterControl is particularly useful when GridControls contain formula cells that reference other sheets:

```csharp
// This is ideal for scenarios where:
// - Income sheet has data
// - Expenses sheet has data
// - Summary sheet has formulas referencing Income and Expenses sheets
// - Cross-references work because all grids are in the same TabBarSplitterControl
```

## Common Configuration Patterns

### Pattern 1: Docked TabBarSplitterControl

```csharp
TabBarSplitterControl tabBarSplitter = new TabBarSplitterControl();
tabBarSplitter.Dock = DockStyle.Fill;  // Fill entire form
```

### Pattern 2: Named Pages for Reference

```csharp
TabBarPage incomePage = new TabBarPage 
{ 
    Text = "Income",  // Display name on tab
    Name = "incomePage"  // Control name for code reference
};
```

### Pattern 3: Adding Multiple Controls to One Page

```csharp
TabBarPage page = new TabBarPage { Text = "Dashboard" };

// Add multiple controls to one page
Panel topPanel = new Panel { Dock = DockStyle.Top, Height = 100 };
GridControl grid = new GridControl { Dock = DockStyle.Fill };

page.Controls.Add(grid);
page.Controls.Add(topPanel);

tabBarSplitterControl1.Controls.Add(page);
```

### Pattern 4: Dynamic Page Creation

```csharp
public void AddNewSheet(string sheetName)
{
    TabBarPage newPage = new TabBarPage { Text = sheetName };
    GridControl newGrid = new GridControl { Dock = DockStyle.Fill };
    newPage.Controls.Add(newGrid);
    tabBarSplitterControl1.Controls.Add(newPage);
}

// Usage
AddNewSheet("NewSheet1");
AddNewSheet("NewSheet2");
```

## Best Practices

1. **Always set Dock = DockStyle.Fill** for GridControls to fill the TabBarPage area
2. **Set meaningful Text property** for TabBarPages (this is what users see on tabs)
3. **Create controls before adding to pages** for better organization
4. **Use descriptive variable names** that reflect the page purpose
5. **Consider using object initializer syntax** for cleaner code (C#)
6. **Group related page creation code together** for maintainability

## Common Issues and Solutions

### Issue: GridControl doesn't fill the page
**Solution:** Set `gridControl.Dock = DockStyle.Fill;`

### Issue: Pages not showing tabs
**Solution:** Ensure you're adding TabBarPages to the TabBarSplitterControl's Controls collection, not the form directly

### Issue: Can't reference GridControl later
**Solution:** Store GridControl references as class fields if you need to access them after creation

```csharp
// Store as fields
private GridControl incomeGrid;
private GridControl expensesGrid;

// Initialize in constructor or method
incomeGrid = new GridControl();
expensesGrid = new GridControl();
```

## Next Steps

With programmatic implementation knowledge, you can:
- Create dynamic tabbed interfaces
- Build workbook-like applications
- Implement multi-sheet data views
- Support formula cells and cross-references
- Organize complex data in tabbed layouts
