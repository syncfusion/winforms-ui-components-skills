# Grid Layout

## Table of Contents
- [Overview](#overview)
- [Stacked Headers](#stacked-headers)
- [Field Chooser](#field-chooser)
- [Multi-Row Records](#multi-row-records)
- [Freezing Columns](#freezing-columns)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides flexible layout customization features including stacked headers for column grouping, field chooser for column visibility, multi-row record display, and frozen columns. These features enhance data organization and navigation.

### Key Components

- **StackedHeaderRows** - Multi-level column headers spanning multiple columns
- **FieldChooser** - Interactive column visibility dialog
- **RecordPreviewCell** - Multi-row record display
- **FrozenCount** - Lock columns from horizontal scrolling

## Stacked Headers

Stacked headers create additional unbound header rows that span across multiple columns, allowing logical grouping of related columns.

### Architecture

```
GridTableDescriptor.StackedHeaderRows (Collection)
├── GridStackedHeaderRowDescriptor (Row)
    ├── GridStackedHeaderDescriptor (Header 1)
    │   └── GridStackedHeaderVisibleColumnDescriptor (Columns)
    ├── GridStackedHeaderDescriptor (Header 2)
    │   └── GridStackedHeaderVisibleColumnDescriptor (Columns)
```

### Add Stacked Headers Programmatically

```csharp
// Step 1: Create stacked header descriptor
GridStackedHeaderDescriptor header1 = new GridStackedHeaderDescriptor("header1", "Personal Info");

// Step 2: Add columns to header
header1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("EmployeeID"));
header1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("FirstName"));
header1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("LastName"));

// Step 3: Create second header
GridStackedHeaderDescriptor header2 = new GridStackedHeaderDescriptor("header2", "Job Information");
header2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Title"));
header2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Department"));
header2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("HireDate"));

// Step 4: Add to row descriptor
GridStackedHeaderRowDescriptor row = new GridStackedHeaderRowDescriptor("Row1",
    new GridStackedHeaderDescriptor[] { header1, header2 });

// Step 5: Add to grid
gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(row);

// Step 6: Display stacked headers
gridGroupingControl1.TopLevelGroupOptions.ShowStackedHeaders = true;
```

### Multi-Level Stacked Headers

Create multiple rows of stacked headers for deeper hierarchies:

```csharp
// First level: Department groups
GridStackedHeaderDescriptor deptHeader1 = new GridStackedHeaderDescriptor("dept1", "Sales Department");
deptHeader1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("SalesRep"));
deptHeader1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Revenue"));
deptHeader1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Target"));

GridStackedHeaderRowDescriptor row1 = new GridStackedHeaderRowDescriptor("DeptRow",
    new GridStackedHeaderDescriptor[] { deptHeader1 });

// Second level: Metric groups within department
GridStackedHeaderDescriptor metricHeader1 = new GridStackedHeaderDescriptor("metric1", "Q1 Metrics");
metricHeader1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Revenue"));
metricHeader1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Target"));

GridStackedHeaderRowDescriptor row2 = new GridStackedHeaderRowDescriptor("MetricRow",
    new GridStackedHeaderDescriptor[] { metricHeader1 });

// Add both rows
gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(row1);
gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(row2);
gridGroupingControl1.TopLevelGroupOptions.ShowStackedHeaders = true;
```

### Stacked Headers in Nested Tables

Apply stacked headers to child tables in hierarchical grids:

```csharp
// Get child table descriptor
GridTableDescriptor childDescriptor = gridGroupingControl1.GetTableDescriptor("Orders");

// Create stacked headers for child table
GridStackedHeaderDescriptor childHeader = new GridStackedHeaderDescriptor("orderHeader", "Order Details");
childHeader.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("OrderID"));
childHeader.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("OrderDate"));
childHeader.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("ShipDate"));

GridStackedHeaderRowDescriptor childRow = new GridStackedHeaderRowDescriptor("OrderRow",
    new GridStackedHeaderDescriptor[] { childHeader });

childDescriptor.StackedHeaderRows.Add(childRow);

// Show in child groups
gridGroupingControl1.ChildGroupOptions.ShowStackedHeaders = true;

// For nested children
gridGroupingControl1.NestedTableGroupOptions.ShowStackedHeaders = true;
```

### Stacked Header Appearance

Customize appearance for all stacked headers:

```csharp
// Customize all stacked headers
gridGroupingControl1.TableDescriptor.Appearance.StackedHeaderCell.TextColor = Color.White;
gridGroupingControl1.TableDescriptor.Appearance.StackedHeaderCell.BackColor = Color.DarkBlue;
gridGroupingControl1.TableDescriptor.Appearance.StackedHeaderCell.Font.Bold = true;
```

Customize individual stacked headers:

```csharp
GridStackedHeaderDescriptor header1 = new GridStackedHeaderDescriptor("header1", "Critical Metrics");

// Set custom appearance for this header
header1.Appearance.StackedHeaderCell.BackColor = Color.DarkRed;
header1.Appearance.StackedHeaderCell.TextColor = Color.White;
header1.Appearance.StackedHeaderCell.Font.Bold = true;

header1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Revenue"));
header1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Profit"));

// Other headers with different appearance
GridStackedHeaderDescriptor header2 = new GridStackedHeaderDescriptor("header2", "Standard Metrics");
header2.Appearance.StackedHeaderCell.BackColor = Color.LightGray;
header2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Orders"));
header2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Units"));

GridStackedHeaderRowDescriptor row = new GridStackedHeaderRowDescriptor("Row1",
    new GridStackedHeaderDescriptor[] { header1, header2 });
gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(row);
```

### Hide Stacked Headers

```csharp
// Hide for parent table
gridGroupingControl1.TopLevelGroupOptions.ShowStackedHeaders = false;

// Hide for child tables
gridGroupingControl1.ChildGroupOptions.ShowStackedHeaders = false;

// Hide for nested children
gridGroupingControl1.NestedTableGroupOptions.ShowStackedHeaders = false;
```

## Field Chooser

Field Chooser provides an interactive dialog for users to show/hide columns including stacked header columns.

### Enable Field Chooser

```csharp
using Syncfusion.GridHelperClasses;

// Create field chooser
FieldChooser fieldChooser = new FieldChooser(gridGroupingControl1);

// Show dialog
fieldChooser.ShowDialog();
```

### Show Only Stacked Headers

By default, Field Chooser shows all inner columns. To display only stacked headers:

```csharp
FieldChooser fieldChooser = new FieldChooser(gridGroupingControl1);

// Show only stacked headers, not inner columns
fieldChooser.EnableColumnsInView = false;

fieldChooser.ShowDialog();
```

### Context Menu Integration

```csharp
// Add field chooser to right-click context menu
gridGroupingControl1.TableControl.MouseDown += (s, e) =>
{
    if (e.Button == MouseButtons.Right)
    {
        ContextMenuStrip menu = new ContextMenuStrip();
        ToolStripMenuItem item = new ToolStripMenuItem("Column Chooser");
        item.Click += (sender, args) =>
        {
            FieldChooser chooser = new FieldChooser(gridGroupingControl1);
            chooser.ShowDialog();
        };
        menu.Items.Add(item);
        menu.Show(gridGroupingControl1.TableControl, e.Location);
    }
};
```

## Multi-Row Records

Display record data across multiple rows using preview rows.

### Enable Preview Rows

```csharp
// Show preview row for each record
gridGroupingControl1.TableDescriptor.ChildGroupOptions.ShowRecordPreviewRow = true;

// Set preview row height
gridGroupingControl1.TopLevelGroupOptions.RecordPreviewRowHeight = 50;
```

### Map Column to Preview Row

```csharp
// Display "Notes" column in preview row instead of main row
gridGroupingControl1.TableDescriptor.Columns["Notes"].Appearance.RecordPreviewCell.CellType = "TextBox";
gridGroupingControl1.TableDescriptor.Columns["Notes"].Appearance.RecordPreviewCell.WrapText = true;

// Hide from main row
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("Notes");
```

### Multi-Column Preview

```csharp
// Show multiple columns in preview area
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordPreviewCell)
    {
        Record record = e.TableCellIdentity.DisplayElement.GetRecord();
        
        // Combine multiple fields in preview
        string notes = record.GetValue("Notes")?.ToString() ?? "";
        string comments = record.GetValue("Comments")?.ToString() ?? "";
        string tags = record.GetValue("Tags")?.ToString() ?? "";
        
        e.Style.Text = $"Notes: {notes}\nComments: {comments}\nTags: {tags}";
        e.Style.WrapText = true;
    }
};
```

## Freezing Columns

Frozen columns remain visible while scrolling horizontally.

### Freeze Leading Columns

```csharp
// Freeze first 2 columns (EmployeeID, FirstName)
gridGroupingControl1.TableModel.Options.FrozenCount = 2;

// Ensure columns are in correct order
if (gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf("EmployeeID") != 0)
{
    gridGroupingControl1.TableDescriptor.VisibleColumns.Move(
        gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf("EmployeeID"), 0);
}
```

### Freeze Trailing Columns

```csharp
// Freeze last 2 columns (Actions, Total)
gridGroupingControl1.TableModel.Options.TrailingFrozenCount = 2;
```

### Freeze Rows and Columns

```csharp
// Freeze columns
gridGroupingControl1.TableModel.Options.FrozenCount = 2;

// Freeze rows (header rows)
gridGroupingControl1.TableModel.Options.FrozenRows = 1;
```

### Visual Separator for Frozen Columns

```csharp
// Add visual separator line
gridGroupingControl1.TableModel.Options.FrozenBorderStyle = GridFrozenBorderStyle.DoubleLine;
gridGroupingControl1.TableModel.Options.FrozenBorderColor = Color.DarkBlue;
```

## Common Scenarios

### Scenario 1: Employee Grid with Departmental Stacked Headers

```csharp
// Create stacked headers for employee departments
GridStackedHeaderDescriptor personalInfo = new GridStackedHeaderDescriptor("personal", "Personal Information");
personalInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("EmployeeID"));
personalInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("FirstName"));
personalInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("LastName"));
personalInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("BirthDate"));
personalInfo.Appearance.StackedHeaderCell.BackColor = Color.LightBlue;

GridStackedHeaderDescriptor jobInfo = new GridStackedHeaderDescriptor("job", "Job Information");
jobInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Title"));
jobInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Department"));
jobInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("HireDate"));
jobInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Salary"));
jobInfo.Appearance.StackedHeaderCell.BackColor = Color.LightGreen;

GridStackedHeaderDescriptor contactInfo = new GridStackedHeaderDescriptor("contact", "Contact Information");
contactInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Email"));
contactInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Phone"));
contactInfo.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Address"));
contactInfo.Appearance.StackedHeaderCell.BackColor = Color.LightYellow;

GridStackedHeaderRowDescriptor row = new GridStackedHeaderRowDescriptor("InfoRow",
    new GridStackedHeaderDescriptor[] { personalInfo, jobInfo, contactInfo });

gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(row);
gridGroupingControl1.TopLevelGroupOptions.ShowStackedHeaders = true;

// Freeze personal info columns
gridGroupingControl1.TableModel.Options.FrozenCount = 4;
```

### Scenario 2: Sales Dashboard with Quarterly Stacked Headers

```csharp
// Top-level: Year headers
GridStackedHeaderDescriptor year2023 = new GridStackedHeaderDescriptor("2023", "2023");
year2023.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q1_2023"));
year2023.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q2_2023"));
year2023.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q3_2023"));
year2023.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q4_2023"));

GridStackedHeaderRowDescriptor yearRow = new GridStackedHeaderRowDescriptor("YearRow",
    new GridStackedHeaderDescriptor[] { year2023 });

// Second-level: Quarter headers
GridStackedHeaderDescriptor q1 = new GridStackedHeaderDescriptor("Q1", "Q1");
q1.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q1_2023"));

GridStackedHeaderDescriptor q2 = new GridStackedHeaderDescriptor("Q2", "Q2");
q2.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q2_2023"));

GridStackedHeaderDescriptor q3 = new GridStackedHeaderDescriptor("Q3", "Q3");
q3.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q3_2023"));

GridStackedHeaderDescriptor q4 = new GridStackedHeaderDescriptor("Q4", "Q4");
q4.VisibleColumns.Add(new GridStackedHeaderVisibleColumnDescriptor("Q4_2023"));

GridStackedHeaderRowDescriptor quarterRow = new GridStackedHeaderRowDescriptor("QuarterRow",
    new GridStackedHeaderDescriptor[] { q1, q2, q3, q4 });

gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(yearRow);
gridGroupingControl1.TableDescriptor.StackedHeaderRows.Add(quarterRow);
gridGroupingControl1.TopLevelGroupOptions.ShowStackedHeaders = true;
```

### Scenario 3: Product Catalog with Preview Images

```csharp
// Enable preview rows
gridGroupingControl1.TopLevelGroupOptions.ShowRecordPreviewRow = true;
gridGroupingControl1.TopLevelGroupOptions.RecordPreviewRowHeight = 100;

// Display image in preview row
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordPreviewCell)
    {
        Record record = e.TableCellIdentity.DisplayElement.GetRecord();
        string imagePath = record.GetValue("ImagePath")?.ToString();
        
        if (!string.IsNullOrEmpty(imagePath) && File.Exists(imagePath))
        {
            e.Style.CellType = "Image";
            e.Style.CellValue = Image.FromFile(imagePath);
            e.Style.ImageSizeMode = GridImageSizeMode.CenterImage;
        }
        
        // Add description below image
        string description = record.GetValue("Description")?.ToString();
        e.Style.Text = description;
    }
};

// Hide image column from main row
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("ImagePath");
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("Description");
```

### Scenario 4: Field Chooser with Persistence

```csharp
// Save column visibility to settings
void SaveColumnVisibility()
{
    string[] visibleColumns = gridGroupingControl1.TableDescriptor.VisibleColumns
        .Select(vc => vc.MappingName).ToArray();
    Properties.Settings.Default.VisibleColumns = string.Join(",", visibleColumns);
    Properties.Settings.Default.Save();
}

// Restore column visibility from settings
void RestoreColumnVisibility()
{
    string savedColumns = Properties.Settings.Default.VisibleColumns;
    if (!string.IsNullOrEmpty(savedColumns))
    {
        string[] columns = savedColumns.Split(',');
        gridGroupingControl1.TableDescriptor.VisibleColumns.Clear();
        foreach (string col in columns)
        {
            if (gridGroupingControl1.TableDescriptor.Columns.Contains(col))
            {
                gridGroupingControl1.TableDescriptor.VisibleColumns.Add(col);
            }
        }
    }
}

// Show field chooser and save on close
FieldChooser fieldChooser = new FieldChooser(gridGroupingControl1);
if (fieldChooser.ShowDialog() == DialogResult.OK)
{
    SaveColumnVisibility();
}
```

## Best Practices

### Stacked Headers

1. **Logical Grouping**: Group related columns under stacked headers (Personal Info, Job Info, Financials).

2. **Color Coding**: Use different background colors for different header groups to improve visual clarity:
   ```csharp
   header1.Appearance.StackedHeaderCell.BackColor = Color.LightBlue;
   header2.Appearance.StackedHeaderCell.BackColor = Color.LightGreen;
   ```

3. **Depth Limit**: Avoid more than 2-3 levels of stacked headers. Too many levels create confusion.

4. **Column Dependencies**: Stacked headers depend on `Columns` collection. Verify columns exist before creating headers.

5. **Drag-Drop**: Columns within stacked headers can be reordered. Entire header groups move together.

### Field Chooser

1. **User Control**: Provide Field Chooser for power users who want custom column layouts.

2. **Default Visibility**: Set sensible defaults. Show commonly-used columns by default.

3. **Persistence**: Save and restore user's column visibility preferences across sessions.

4. **Access Point**: Add Field Chooser to:
   - Context menu (right-click on headers)
   - Toolbar button
   - View menu

### Preview Rows

1. **Long Content**: Use preview rows for long text fields (Notes, Comments, Descriptions) that don't fit in regular cells.

2. **Height Sizing**: Set appropriate `RecordPreviewRowHeight` based on content. Test with typical data.

3. **Performance**: Preview rows increase grid height. Use sparingly with large datasets (>1000 records).

4. **Multi-Field Display**: Combine multiple fields in preview for comprehensive record summaries.

### Frozen Columns

1. **Freeze Key Columns**: Freeze ID, Name, or other key identifier columns for easy reference while scrolling.

2. **Balance**: Don't freeze too many columns. Leave adequate scrollable area (freeze 1-3 columns typically).

3. **Visual Feedback**: Use `FrozenBorderStyle` to clearly indicate frozen column boundary:
   ```csharp
   gridGroupingControl1.TableModel.Options.FrozenBorderStyle = GridFrozenBorderStyle.DoubleLine;
   ```

4. **Trailing Frozen**: Use trailing frozen columns for action columns (Edit, Delete, View) that should always be visible.

### General Layout

- Test layouts with actual data volumes and screen resolutions
- Provide user customization options (Field Chooser, resizing, reordering)
- Document special features (stacked headers, preview rows) in user guides
- Consider accessibility (keyboard navigation through stacked headers, screen reader support)
