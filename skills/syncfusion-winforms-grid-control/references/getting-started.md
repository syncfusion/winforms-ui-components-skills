# Getting Started with GridControl

This guide covers the initial setup and configuration of Syncfusion Windows Forms GridControl, including installation, assembly references, and creating your first grid.

## Choosing the Right Grid Control

Syncfusion provides multiple grid controls for Windows Forms. Understanding which one to use is essential:

### GridControl (Cell-Oriented)
- **Best for:** Custom layouts, spreadsheet-like interfaces, virtual grids
- **Key features:** Cell-level customization, virtual mode, formula support
- **Data binding:** Not required (contains own data)
- **When to use:** Need complete control over individual cells

### GridDataBoundGrid (Column-Oriented, Classic)
- **Best for:** Simple data tables with ADO.NET sources
- **Key features:** Column-based styling, basic sorting and filtering
- **Data binding:** Required (ADO.NET DataSet/DataTable)
- **When to use:** Simple data-bound scenarios without grouping

### GridGroupingControl (Data-Bound with Grouping)
- **Best for:** Complex data views with grouping, filtering, sorting
- **Key features:** Grouping, hierarchical data, summaries, expressions
- **Data binding:** Required (IList, IBindingList sources)
- **When to use:** Need advanced data operations

### SfDataGrid (Modern Grid)
- **Best for:** New applications needing high performance
- **Key features:** Modern architecture, advanced features, best performance
- **Data binding:** Required
- **When to use:** Starting new projects, need latest features

**This guide focuses on GridControl** - the cell-oriented grid.

## Installation and Assembly Deployment

### Method 1: NuGet Package Manager

Install via Package Manager Console:

```powershell
Install-Package Syncfusion.Grid.Windows
```

Or use NuGet Package Manager UI:
1. Right-click project → **Manage NuGet Packages**
2. Search for **"Syncfusion.Grid.Windows"**
3. Click **Install**

### Method 2: Manual Assembly References

Add the following assemblies to your project:

**Required assemblies:**
- `Syncfusion.Grid.Windows.dll` - Main GridControl
- `Syncfusion.Grid.Base.dll` - Base grid functionality
- `Syncfusion.Shared.Base.dll` - Shared utilities
- `Syncfusion.Shared.Windows.dll` - Windows shared components

To add manually:
1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation folder
4. Select the required DLLs

### License Configuration

For licensed versions, register your license key in `Program.cs`:

```csharp
using Syncfusion.Licensing;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Register Syncfusion license
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

## Adding GridControl through Designer

The designer approach provides visual feedback and is great for beginners.

### Steps:

1. **Create a new Windows Forms Application** in Visual Studio

2. **Open Form1 in designer view**

3. **Open the Toolbox** (View → Toolbox or Ctrl+Alt+X)

4. **Locate GridControl** in the Syncfusion section

5. **Drag and drop** GridControl onto the form

   The control appears with default dimensions and automatically adds required assemblies to your project.

6. **Configure properties** in the Properties window:
   - Set `RowCount` and `ColCount`
   - Configure `Size` or `Dock` property
   - Set `Name` property (e.g., "gridControl1")

7. **Edit cells in designer:**
   - Select the grid
   - Click the **Edit** button in the smart tag or properties
   - The grid becomes editable in the designer
   - Enter values directly
   - Use Property Grid on the right to modify cell styles

### Designer Mode Features:

**Grid Properties Tab:**
- Modify grid-wide settings
- Change appearance for all cells
- Configure behavior properties

**Selected Range Tab:**
- Style specific cells or ranges
- Set cell values
- Configure cell types
- Apply formatting

## Adding GridControl through Code

For programmatic control and dynamic scenarios, add GridControl via code.

### Basic Setup:

```csharp
using Syncfusion.Windows.Forms.Grid;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private GridControl gridControl1;
    
    public Form1()
    {
        InitializeComponent();
        InitializeGrid();
    }
    
    private void InitializeGrid()
    {
        // Create GridControl instance
        gridControl1 = new GridControl();
        
        // Set size and position
        gridControl1.Size = new Size(600, 400);
        gridControl1.Location = new Point(10, 10);
        
        // Or use docking
        // gridControl1.Dock = DockStyle.Fill;
        
        // Add to form's controls
        this.Controls.Add(gridControl1);
    }
}
```

### VB.NET Version:

```vb
Imports Syncfusion.Windows.Forms.Grid

Public Class Form1
    Private gridControl1 As GridControl
    
    Public Sub New()
        InitializeComponent()
        InitializeGrid()
    End Sub
    
    Private Sub InitializeGrid()
        ' Create GridControl instance
        gridControl1 = New GridControl()
        
        ' Set size and position
        gridControl1.Size = New Size(600, 400)
        gridControl1.Location = New Point(10, 10)
        
        ' Add to form's controls
        Me.Controls.Add(gridControl1)
    End Sub
End Class
```

## Initial Configuration

### Setting Grid Dimensions:

```csharp
// Set number of rows and columns
gridControl1.RowCount = 50;
gridControl1.ColCount = 10;

// Default is 10 rows x 10 columns if not specified
```

### Basic Appearance:

```csharp
// Grid appearance
gridControl1.BackColor = Color.White;
gridControl1.GridLineColor = Color.LightGray;
gridControl1.ThemesEnabled = true;

// Row and column headers
gridControl1.Properties.RowHeaders = true;
gridControl1.Properties.ColHeaders = true;

// Borders
gridControl1.BorderStyle = BorderStyle.FixedSingle;
```

### Common Initial Settings:

```csharp
// Enable Excel-like features
gridControl1.ExcelLikeSelectionFrame = true;
gridControl1.ExcelLikeCurrentCell = true;

// Selection mode
gridControl1.AllowSelection = GridSelectionFlags.Any;

// Editing
gridControl1.ReadOnly = false;  // Allow editing (default)

// Scrolling
gridControl1.HScrollBehavior = GridScrollbarMode.Shared;
gridControl1.VScrollBehavior = GridScrollbarMode.Shared;
```

## Creating Your First Grid

### Complete Example:

```csharp
using Syncfusion.Windows.Forms.Grid;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private GridControl gridControl1;
    
    public Form1()
    {
        InitializeComponent();
        SetupGrid();
        PopulateGrid();
        StyleGrid();
    }
    
    private void SetupGrid()
    {
        // Create and configure grid
        gridControl1 = new GridControl
        {
            Dock = DockStyle.Fill,
            RowCount = 20,
            ColCount = 5,
            ExcelLikeSelectionFrame = true,
            ExcelLikeCurrentCell = true,
            AllowSelection = GridSelectionFlags.Any
        };
        
        this.Controls.Add(gridControl1);
    }
    
    private void PopulateGrid()
    {
        // Set header row
        gridControl1[0, 1].CellValue = "Product";
        gridControl1[0, 2].CellValue = "Quantity";
        gridControl1[0, 3].CellValue = "Price";
        gridControl1[0, 4].CellValue = "Total";
        
        // Populate data rows
        for (int row = 1; row <= 20; row++)
        {
            gridControl1[row, 1].CellValue = $"Product {row}";
            gridControl1[row, 2].CellValue = row * 10;
            gridControl1[row, 3].CellValue = row * 5.99;
            gridControl1[row, 4].CellValue = (row * 10) * (row * 5.99);
        }
    }
    
    private void StyleGrid()
    {
        // Style header row
        GridStyleInfo headerStyle = new GridStyleInfo
        {
            BackColor = Color.SteelBlue,
            TextColor = Color.White,
            Font = { Bold = true, Size = 10f }
        };
        
        gridControl1.ChangeCells(
            GridRangeInfo.Row(0), 
            headerStyle
        );
        
        // Alternate row colors
        for (int row = 1; row <= gridControl1.RowCount; row++)
        {
            if (row % 2 == 0)
            {
                GridStyleInfo altRowStyle = new GridStyleInfo
                {
                    BackColor = Color.WhiteSmoke
                };
                gridControl1.ChangeCells(
                    GridRangeInfo.Row(row), 
                    altRowStyle
                );
            }
        }
    }
}
```

## Namespace Imports

Always include these namespaces when working with GridControl:

```csharp
using Syncfusion.Windows.Forms.Grid;          // Main grid classes
using Syncfusion.Windows.Forms.Grid.Grouping; // Grouping features (optional)
using System.Drawing;                         // Colors, fonts
using System.Windows.Forms;                   // WinForms basics
```

## Quick Reference: Essential Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `RowCount` | Number of rows | `gridControl1.RowCount = 100;` |
| `ColCount` | Number of columns | `gridControl1.ColCount = 20;` |
| `Dock` | Docking style | `gridControl1.Dock = DockStyle.Fill;` |
| `Model[r,c]` | Access cell | `gridControl1[1, 1].CellValue = "Test";` |
| `ExcelLikeSelectionFrame` | Excel selection | `gridControl1.ExcelLikeSelectionFrame = true;` |
| `AllowSelection` | Enable selection | `gridControl1.AllowSelection = GridSelectionFlags.Any;` |
| `ReadOnly` | Prevent editing | `gridControl1.ReadOnly = true;` |

## Common Gotchas

### Row/Column Indexing
- GridControl uses **1-based indexing** for data cells
- Row 0 and Column 0 are **headers**
- Valid data cells start at `[1, 1]`

```csharp
// Header cells
gridControl1[0, 1].CellValue = "Column Header";
gridControl1[1, 0].CellValue = "Row Header";

// First data cell
gridControl1[1, 1].CellValue = "Data";
```

### Designer vs Code
- Designer sets properties on the control instance
- Code can override designer settings
- Use `InitializeComponent()` before your custom initialization

### Assembly Loading
- Ensure all required DLLs are referenced
- Check for version mismatches
- Verify license key is registered before control creation

## Next Steps

Now that you have a basic GridControl set up:
1. Learn about data population methods
2. Explore cell styling architecture
3. Configure cell types for different data
4. Implement selection and editing
5. Add Excel-like features
6. Enable formula support

Refer to the other reference documents for detailed coverage of these topics.
