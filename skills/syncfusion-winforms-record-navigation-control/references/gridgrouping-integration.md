# GridGroupingControl Integration

This guide covers integrating GridRecordNavigationControl with GridGroupingControl to provide built-in navigation support for grouped data.

## Table of Contents
- [Overview](#overview)
- [ShowNavigationBar Property](#shownavigationbar-property)
- [RecordNavigationBar Property](#recordnavigationbar-property)
- [Complete Integration Example](#complete-integration-example)
- [Navigation Methods with GridGroupingControl](#navigation-methods-with-gridgroupingcontrol)
- [Use Case Scenarios](#use-case-scenarios)
- [Configuration and Setup](#configuration-and-setup)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The GridGroupingControl provides built-in support for record navigation through an integrated navigation bar. Unlike the standalone GridRecordNavigationControl, the GridGroupingControl has native navigation support that can be enabled with a single property.

**Key Features:**
- **Built-in Navigation** - No separate control needed
- **ShowNavigationBar Property** - Enable/disable with one line
- **RecordNavigationBar Access** - Direct access to navigation methods
- **Grouped Data Support** - Navigate records while maintaining group structure
- **Four Navigation Types** - First, Last, Previous, Next

## Use Case Scenarios

Use GridGroupingControl with navigation when you need to:
- Navigate through large datasets with grouping
- Browse grouped records sequentially
- Provide Microsoft Access-like navigation for grouped data
- Display record position within grouped datasets
- Enable programmatic navigation in grouped grids

**When to use this approach vs standalone GridRecordNavigationControl:**
- Use GridGroupingControl integration for **grouped data**
- Use standalone GridRecordNavigationControl for **flat grid data** (GridControl, GridDataBoundGrid)

## ShowNavigationBar Property

The `ShowNavigationBar` property enables or disables the navigation bar in GridGroupingControl.

### Enabling Navigation Bar

**C# Example:**
```csharp
this.gridGroupingControl1.ShowNavigationBar = true;
```

**VB.NET Example:**
```vb
Me.gridGroupingControl1.ShowNavigationBar = True
```

### Property Details

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowNavigationBar` | bool | false | Shows/hides the navigation bar at the bottom of the grid |

**Effect:**
- When `true`: Navigation bar appears at bottom with first/previous/next/last buttons and record counter
- When `false`: Navigation bar is hidden

## RecordNavigationBar Property

The `RecordNavigationBar` property provides access to the navigation bar object and its methods.

### Accessing Navigation Methods

```csharp
// The navigation bar must be enabled first
this.gridGroupingControl1.ShowNavigationBar = true;

// Access navigation methods through RecordNavigationBar
this.gridGroupingControl1.RecordNavigationBar.MoveFirst();
this.gridGroupingControl1.RecordNavigationBar.MoveLast();
this.gridGroupingControl1.RecordNavigationBar.MoveNext();
this.gridGroupingControl1.RecordNavigationBar.MovePrevious();
```

## Navigation Methods with GridGroupingControl

### Method Reference Table

| Method | Description | Parameters | Return Type |
|--------|-------------|------------|-------------|
| `MoveFirst()` | Navigate to the first record | N/A | void |
| `MoveLast()` | Navigate to the last record | N/A | void |
| `MoveNext()` | Navigate to the next record | N/A | void |
| `MovePrevious()` | Navigate to the previous record | N/A | void |

### MoveFirst() Method

Navigate to the first record in the grouped dataset.

```csharp
this.gridGroupingControl1.RecordNavigationBar.MoveFirst();
```

**VB.NET:**
```vb
Me.gridGroupingControl1.RecordNavigationBar.MoveFirst()
```

### MoveLast() Method

Navigate to the last record in the grouped dataset.

```csharp
this.gridGroupingControl1.RecordNavigationBar.MoveLast();
```

**VB.NET:**
```vb
Me.gridGroupingControl1.RecordNavigationBar.MoveLast()
```

### MoveNext() Method

Navigate to the next record.

```csharp
this.gridGroupingControl1.RecordNavigationBar.MoveNext();
```

**VB.NET:**
```vb
Me.gridGroupingControl1.RecordNavigationBar.MoveNext()
```

### MovePrevious() Method

Navigate to the previous record.

```csharp
this.gridGroupingControl1.RecordNavigationBar.MovePrevious();
```

**VB.NET:**
```vb
Me.gridGroupingControl1.RecordNavigationBar.MovePrevious()
```

## Complete Integration Example

### Basic Setup with Navigation

```csharp
using Syncfusion.Windows.Forms.Grid.Grouping;
using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;

namespace GridGroupingNavigationExample
{
    public partial class MainForm : Form
    {
        private GridGroupingControl gridGroupingControl1;
        
        public MainForm()
        {
            InitializeComponent();
            SetupGridGroupingControl();
            LoadData();
            EnableNavigation();
        }
        
        private void SetupGridGroupingControl()
        {
            // Create GridGroupingControl
            gridGroupingControl1 = new GridGroupingControl
            {
                Location = new Point(10, 10),
                Size = new Size(760, 500)
            };
            
            this.Controls.Add(gridGroupingControl1);
        }
        
        private void LoadData()
        {
            // Create sample data
            DataTable employees = CreateEmployeeData();
            
            // Bind data to grid
            gridGroupingControl1.DataSource = employees;
            
            // Configure grouping
            gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");
        }
        
        private void EnableNavigation()
        {
            // Enable the navigation bar
            gridGroupingControl1.ShowNavigationBar = true;
        }
        
        private DataTable CreateEmployeeData()
        {
            DataTable dt = new DataTable("Employees");
            dt.Columns.Add("EmployeeID", typeof(int));
            dt.Columns.Add("Name", typeof(string));
            dt.Columns.Add("Department", typeof(string));
            dt.Columns.Add("Salary", typeof(decimal));
            
            // Add sample records
            dt.Rows.Add(1, "John Smith", "Sales", 55000);
            dt.Rows.Add(2, "Jane Doe", "Marketing", 60000);
            dt.Rows.Add(3, "Bob Johnson", "Sales", 52000);
            dt.Rows.Add(4, "Alice Brown", "IT", 70000);
            dt.Rows.Add(5, "Charlie Wilson", "Marketing", 58000);
            dt.Rows.Add(6, "Diana Davis", "IT", 72000);
            dt.Rows.Add(7, "Eve Martinez", "Sales", 54000);
            dt.Rows.Add(8, "Frank Garcia", "IT", 68000);
            
            return dt;
        }
    }
}
```

### Example with Custom Navigation Buttons

```csharp
using Syncfusion.Windows.Forms.Grid.Grouping;
using System;
using System.Windows.Forms;

public partial class NavigationForm : Form
{
    private GridGroupingControl gridGroupingControl1;
    private Button btnFirst, btnPrevious, btnNext, btnLast;
    
    private void InitializeNavigationButtons()
    {
        // Create custom navigation buttons
        btnFirst = new Button
        {
            Text = "⏮ First",
            Location = new System.Drawing.Point(10, 520),
            Size = new System.Drawing.Size(80, 30)
        };
        
        btnPrevious = new Button
        {
            Text = "◀ Previous",
            Location = new System.Drawing.Point(100, 520),
            Size = new System.Drawing.Size(90, 30)
        };
        
        btnNext = new Button
        {
            Text = "Next ▶",
            Location = new System.Drawing.Point(200, 520),
            Size = new System.Drawing.Size(90, 30)
        };
        
        btnLast = new Button
        {
            Text = "Last ⏭",
            Location = new System.Drawing.Point(300, 520),
            Size = new System.Drawing.Size(80, 30)
        };
        
        // Wire up events
        btnFirst.Click += (s, e) => gridGroupingControl1.RecordNavigationBar.MoveFirst();
        btnPrevious.Click += (s, e) => gridGroupingControl1.RecordNavigationBar.MovePrevious();
        btnNext.Click += (s, e) => gridGroupingControl1.RecordNavigationBar.MoveNext();
        btnLast.Click += (s, e) => gridGroupingControl1.RecordNavigationBar.MoveLast();
        
        // Add to form
        this.Controls.AddRange(new Control[] { btnFirst, btnPrevious, btnNext, btnLast });
        
        // Enable built-in navigation bar
        gridGroupingControl1.ShowNavigationBar = true;
    }
}
```

## Configuration and Setup

### Complete Setup Checklist

Follow these steps to add navigation to GridGroupingControl:

1. **Create GridGroupingControl**
```csharp
var gridGroupingControl = new GridGroupingControl();
this.Controls.Add(gridGroupingControl);
```

2. **Set Data Source**
```csharp
gridGroupingControl.DataSource = myDataTable;
```

3. **Configure Grouping (Optional)**
```csharp
gridGroupingControl.TableDescriptor.GroupedColumns.Add("CategoryColumn");
```

4. **Enable Navigation Bar**
```csharp
gridGroupingControl.ShowNavigationBar = true;
```

5. **Use Navigation Methods**
```csharp
gridGroupingControl.RecordNavigationBar.MoveFirst();
```

### VB.NET Complete Example

```vb
Imports Syncfusion.Windows.Forms.Grid.Grouping
Imports System.Data

Public Class NavigationForm
    Private gridGroupingControl1 As GridGroupingControl
    
    Private Sub InitializeGrid()
        ' Create control
        Me.gridGroupingControl1 = New GridGroupingControl()
        Me.gridGroupingControl1.Location = New Point(10, 10)
        Me.gridGroupingControl1.Size = New Size(760, 500)
        
        ' Add to form
        Me.Controls.Add(Me.gridGroupingControl1)
        
        ' Set data source
        Dim dt As DataTable = CreateSampleData()
        Me.gridGroupingControl1.DataSource = dt
        
        ' Configure grouping
        Me.gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department")
        
        ' Enable navigation bar
        Me.gridGroupingControl1.ShowNavigationBar = True
    End Sub
    
    Private Function CreateSampleData() As DataTable
        Dim dt As New DataTable("Data")
        dt.Columns.Add("ID", GetType(Integer))
        dt.Columns.Add("Name", GetType(String))
        dt.Columns.Add("Department", GetType(String))
        
        For i As Integer = 1 To 100
            dt.Rows.Add(i, "Employee " & i, "Dept " & (i Mod 5))
        Next
        
        Return dt
    End Function
    
    ' Navigation button event handlers
    Private Sub BtnFirst_Click(sender As Object, e As EventArgs)
        Me.gridGroupingControl1.RecordNavigationBar.MoveFirst()
    End Sub
    
    Private Sub BtnLast_Click(sender As Object, e As EventArgs)
        Me.gridGroupingControl1.RecordNavigationBar.MoveLast()
    End Sub
    
    Private Sub BtnNext_Click(sender As Object, e As EventArgs)
        Me.gridGroupingControl1.RecordNavigationBar.MoveNext()
    End Sub
    
    Private Sub BtnPrevious_Click(sender As Object, e As EventArgs)
        Me.gridGroupingControl1.RecordNavigationBar.MovePrevious()
    End Sub
End Class
```

## Use Case Scenarios

### Use Case 1: Large Grouped Dataset Navigation

Navigate through large datasets organized by groups:

```csharp
private void SetupLargeGroupedData()
{
    // Create large dataset
    DataTable orders = new DataTable("Orders");
    orders.Columns.Add("OrderID", typeof(int));
    orders.Columns.Add("CustomerName", typeof(string));
    orders.Columns.Add("Region", typeof(string));
    orders.Columns.Add("Amount", typeof(decimal));
    orders.Columns.Add("OrderDate", typeof(DateTime));
    
    // Add 10,000 sample orders
    Random rnd = new Random();
    string[] regions = { "North", "South", "East", "West", "Central" };
    
    for (int i = 1; i <= 10000; i++)
    {
        orders.Rows.Add(
            i,
            $"Customer {i}",
            regions[i % regions.Length],
            rnd.Next(100, 10000),
            DateTime.Now.AddDays(-rnd.Next(0, 365))
        );
    }
    
    // Setup grid with grouping
    gridGroupingControl1.DataSource = orders;
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Region");
    gridGroupingControl1.ShowNavigationBar = true;
    
    // Now users can easily navigate through 10,000 records
}
```

### Use Case 2: Sequential Record Review

Review records one by one for approval/rejection:

```csharp
private int reviewedCount = 0;
private int totalRecords = 0;

private void InitializeReviewWorkflow()
{
    gridGroupingControl1.ShowNavigationBar = true;
    
    // Get total records
    if (gridGroupingControl1.DataSource is DataTable dt)
    {
        totalRecords = dt.Rows.Count;
    }
    
    // Start at first record
    gridGroupingControl1.RecordNavigationBar.MoveFirst();
    UpdateReviewStatus();
}

private void ApproveAndNext_Click(object sender, EventArgs e)
{
    // Approve current record
    ApproveCurrentRecord();
    reviewedCount++;
    
    // Move to next
    if (reviewedCount < totalRecords)
    {
        gridGroupingControl1.RecordNavigationBar.MoveNext();
        UpdateReviewStatus();
    }
    else
    {
        MessageBox.Show("All records reviewed!", "Complete");
    }
}

private void UpdateReviewStatus()
{
    statusLabel.Text = $"Reviewed: {reviewedCount} of {totalRecords}";
}
```

### Use Case 3: Multi-Level Group Navigation

Navigate through hierarchically grouped data:

```csharp
private void SetupHierarchicalGrouping()
{
    // Create hierarchical data
    DataTable sales = CreateSalesData();
    
    // Setup grid
    gridGroupingControl1.DataSource = sales;
    
    // Multi-level grouping
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Region");
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Month");
    
    // Enable navigation
    gridGroupingControl1.ShowNavigationBar = true;
    
    // Navigation works across all group levels
}
```

### Use Case 4: Filtered Data Navigation

Navigate through filtered/searched results:

```csharp
private void SearchAndNavigate(string searchTerm)
{
    // Apply filter
    gridGroupingControl1.TableDescriptor.RecordFilters.Add(
        new Syncfusion.Grouping.RecordFilterDescriptor(
            $"ProductName LIKE '%{searchTerm}%'"
        )
    );
    
    // Enable navigation for filtered results
    gridGroupingControl1.ShowNavigationBar = true;
    
    // Navigate to first filtered result
    gridGroupingControl1.RecordNavigationBar.MoveFirst();
}
```

## Visual Example

When navigation is enabled, the navigation bar appears at the bottom of the GridGroupingControl:

The navigation bar displays:
- **First button** (⏮) - Go to first record
- **Previous button** (◀) - Go to previous record
- **Record position** - Current record number
- **Next button** (▶) - Go to next record
- **Last button** (⏭) - Go to last record
- **Record count** - Total number of records

## Best Practices

### ✅ Do

1. **Enable ShowNavigationBar before calling navigation methods**
```csharp
gridGroupingControl1.ShowNavigationBar = true;
gridGroupingControl1.RecordNavigationBar.MoveFirst(); // ✓ Safe
```

2. **Check if navigation bar is enabled**
```csharp
if (gridGroupingControl1.ShowNavigationBar)
{
    gridGroupingControl1.RecordNavigationBar.MoveNext();
}
```

3. **Use with grouped data**
```csharp
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
gridGroupingControl1.ShowNavigationBar = true;
```

4. **Provide visual feedback during navigation**
```csharp
private void NavigateWithFeedback(Action navigationAction)
{
    navigationAction.Invoke();
    UpdateStatusBar();
    RefreshUI();
}
```

### ❌ Don't

1. **Don't call navigation methods without enabling ShowNavigationBar**
```csharp
// ✗ Wrong - ShowNavigationBar not enabled
gridGroupingControl1.RecordNavigationBar.MoveFirst();
```

2. **Don't forget to handle null references**
```csharp
// ✗ Wrong - could throw NullReferenceException
gridGroupingControl1.RecordNavigationBar.MoveNext();

// ✓ Correct
if (gridGroupingControl1.ShowNavigationBar && 
    gridGroupingControl1.RecordNavigationBar != null)
{
    gridGroupingControl1.RecordNavigationBar.MoveNext();
}
```

## Troubleshooting

### Navigation Bar Not Visible

**Problem:** ShowNavigationBar is set to true but bar doesn't appear

**Solution:** 
- Verify data source is set before enabling navigation
- Check that GridGroupingControl has sufficient height
- Ensure no layout issues covering the bottom area

```csharp
// Correct order
gridGroupingControl1.DataSource = myData;
gridGroupingControl1.ShowNavigationBar = true; // Enable after data is bound
```

### Navigation Methods Not Working

**Problem:** Calling navigation methods has no effect

**Solution:** Ensure ShowNavigationBar is enabled first

```csharp
// Check before using
if (!gridGroupingControl1.ShowNavigationBar)
{
    gridGroupingControl1.ShowNavigationBar = true;
}

gridGroupingControl1.RecordNavigationBar.MoveFirst();
```

### NullReferenceException on RecordNavigationBar

**Problem:** RecordNavigationBar is null

**Solution:** Enable ShowNavigationBar to initialize the navigation bar

```csharp
// Initialize first
gridGroupingControl1.ShowNavigationBar = true;

// Now safe to use
gridGroupingControl1.RecordNavigationBar.MoveNext();
```

## Comparison: GridGroupingControl vs GridRecordNavigationControl

| Feature | GridGroupingControl | GridRecordNavigationControl |
|---------|-------------------|---------------------------|
| **Use Case** | Grouped data | Flat grid data |
| **Setup** | Single property | Separate control + grid child |
| **Navigation** | Built-in | Requires setup |
| **Grouping** | Native support | Not supported |
| **Grid Type** | GridGroupingControl only | GridControl, GridDataBoundGrid |

## Quick Reference

```csharp
// Enable navigation bar
gridGroupingControl1.ShowNavigationBar = true;

// Navigate to first record
gridGroupingControl1.RecordNavigationBar.MoveFirst();

// Navigate to last record
gridGroupingControl1.RecordNavigationBar.MoveLast();

// Navigate to next record
gridGroupingControl1.RecordNavigationBar.MoveNext();

// Navigate to previous record
gridGroupingControl1.RecordNavigationBar.MovePrevious();
```

## Next Steps

- Explore grouping configurations for different data structures
- Implement custom navigation UI alongside built-in navigation
- Apply visual styles to match your application theme
- Add event handlers for navigation state changes
