# Cell Types

## Table of Contents
- [Overview](#overview)
- [Cell Type Architecture](#cell-type-architecture)
- [Built-in Cell Types](#built-in-cell-types)
- [Enhanced Cell Types](#enhanced-cell-types)
- [Setting Cell Types](#setting-cell-types)
- [Creating Custom Cell Types](#creating-custom-cell-types)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl uses a cell type architecture that separates data representation (model) from visual rendering (renderer). This allows flexible customization of how cells display and interact with data.

### Key Components

- **GridCellModelBase** - Defines cell behavior and data handling
- **GridCellRendererBase** - Controls visual rendering and user interaction
- **CellType Property** - Assigns cell type to columns or individual cells
- **CellRenderers Collection** - Manages registered cell types

## Cell Type Architecture

### Model-Renderer Pattern

Each cell type consists of two parts:

```
GridCellModelBase (Data Layer)
├── Manages cell data and state
├── Serialization support
└── Creates renderer instances

GridCellRendererBase (View Layer)
├── Handles drawing/rendering
├── Processes user input (mouse, keyboard)
└── Manages editing controls
```

### Built-in Architecture

```csharp
// Model defines behavior
public class TextBoxCellModel : GridCellModelBase
{
    public override GridCellRendererBase CreateRenderer(GridControlBase control)
    {
        return new TextBoxCellRenderer(control, this);
    }
}

// Renderer handles visuals
public class TextBoxCellRenderer : GridCellRendererBase
{
    protected override void OnDraw(Graphics g, Rectangle clientRectangle, 
        int rowIndex, int colIndex, GridStyleInfo style)
    {
        // Drawing logic
    }
}
```

## Built-in Cell Types

GridGroupingControl includes standard cell types for common scenarios.

### TextBox

Default cell type for text input.

```csharp
// Set column to TextBox type (explicit)
gridGroupingControl1.TableDescriptor.Columns["FirstName"].Appearance.AnyRecordFieldCell.CellType = "TextBox";
```

### CheckBox

Boolean values displayed as checkboxes.

```csharp
// Set column to CheckBox
gridGroupingControl1.TableDescriptor.Columns["IsActive"].Appearance.AnyRecordFieldCell.CellType = "CheckBox";

// Customize appearance
gridGroupingControl1.TableDescriptor.Columns["IsActive"].Appearance.AnyRecordFieldCell.CellAppearance = GridCellAppearance.Raised;
gridGroupingControl1.TableDescriptor.Columns["IsActive"].Appearance.AnyRecordFieldCell.CheckBoxOptions = new GridCheckBoxCellInfo("Yes", "No", "", true);
```

### ComboBox

Dropdown list for selection from predefined values.

```csharp
// Set column to ComboBox
gridGroupingControl1.TableDescriptor.Columns["Status"].Appearance.AnyRecordFieldCell.CellType = "ComboBox";

// Set dropdown items
gridGroupingControl1.TableDescriptor.Columns["Status"].Appearance.AnyRecordFieldCell.ChoiceList = new string[] 
{ 
    "Active", 
    "Inactive", 
    "Pending", 
    "Suspended" 
};

// Dropdown style
gridGroupingControl1.TableDescriptor.Columns["Status"].Appearance.AnyRecordFieldCell.DropDownStyle = GridDropDownStyle.Exclusive; // No manual entry
```

### DateTimePicker

Date/time selection with calendar popup.

```csharp
// Set column to DateTimePicker
gridGroupingControl1.TableDescriptor.Columns["BirthDate"].Appearance.AnyRecordFieldCell.CellType = GridCellTypeName.DateTime;

// Configure format
gridGroupingControl1.TableDescriptor.Columns["BirthDate"].Appearance.AnyRecordFieldCell.Format = "MM/dd/yyyy";
gridGroupingControl1.TableDescriptor.Columns["BirthDate"].Appearance.AnyRecordFieldCell.DateTimeEdit = GridDateTimeEdit.ShowDatePicker;
```

### Currency

Numeric values formatted as currency.

```csharp
// Set column to Currency
gridGroupingControl1.TableDescriptor.Columns["Salary"].Appearance.AnyRecordFieldCell.CellType = GridCellTypeName.Currency;

// Format options
gridGroupingControl1.TableDescriptor.Columns["Salary"].Appearance.AnyRecordFieldCell.Format = "C2"; // Two decimal places
gridGroupingControl1.TableDescriptor.Columns["Salary"].Appearance.AnyRecordFieldCell.CurrencyEdit = new GridCurrencyEditStyleInfo();
```

### Static

Read-only display (no editing).

```csharp
// Set column to Static (read-only)
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Appearance.AnyRecordFieldCell.CellType = "Static";
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Appearance.AnyRecordFieldCell.ReadOnly = true;
```

## Enhanced Cell Types

Syncfusion provides additional cell types through the `GridHelperClasses` assembly.

### Registering Enhanced Cell Types

```csharp
using Syncfusion.GridHelperClasses;

// Register all enhanced cell types
RegisterCellModel.GridGroupingCellType(gridGroupingControl1);
```

### Available Enhanced Types

#### MaskedEdit

Input with format mask (phone, SSN, etc.).

```csharp
// After registering helper cell types
gridGroupingControl1.TableDescriptor.Columns["Phone"].Appearance.AnyRecordFieldCell.CellType = CustomCellTypes.MaskedEdit;
gridGroupingControl1.TableDescriptor.Columns["Phone"].Appearance.AnyRecordFieldCell.MaskEdit = new GridMaskEditInfo
{
    Mask = "(999) 000-0000"
};
```

#### PercentEdit

Percentage values with % symbol.

```csharp
gridGroupingControl1.TableDescriptor.Columns["Discount"].Appearance.AnyRecordFieldCell.CellType = CustomCellTypes.PercentEdit;
```

#### IntegerEdit

Integer-only input with increment buttons.

```csharp
gridGroupingControl1.TableDescriptor.Columns["Quantity"].Appearance.AnyRecordFieldCell.CellType = CustomCellTypes.IntegerEdit;
```

#### NumericUpDown

Numeric input with up/down buttons.

```csharp
gridGroupingControl1.TableDescriptor.Columns["Rating"].Appearance.AnyRecordFieldCell.CellType = CustomCellTypes.NumericUpDown;
gridGroupingControl1.TableDescriptor.Columns["Rating"].Appearance.AnyRecordFieldCell.NumericUpDown = new GridNumericUpDownCellInfo(1, 10, 1); // Min, Max, Step
```

## Setting Cell Types

### Column-Level Cell Type

Apply to entire column:

```csharp
// Set cell type for all cells in column
gridGroupingControl1.TableDescriptor.Columns["Status"].Appearance.AnyRecordFieldCell.CellType = "ComboBox";
gridGroupingControl1.TableDescriptor.Columns["Status"].Appearance.AnyRecordFieldCell.ChoiceList = new string[] { "Open", "Closed", "Pending" };
```

### Cell-Level Cell Type

Apply to specific cells using `QueryCellStyleInfo`:

```csharp
gridGroupingControl1.QueryCellStyleInfo += GridGroupingControl1_QueryCellStyleInfo;

void GridGroupingControl1_QueryCellStyleInfo(object sender, GridTableCellStyleInfoEventArgs e)
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell)
    {
        if (e.TableCellIdentity.Column != null && e.TableCellIdentity.Column.Name == "Priority")
        {
            Record record = e.TableCellIdentity.DisplayElement.GetRecord();
            int priorityValue = Convert.ToInt32(record.GetValue("PriorityValue"));
            
            // High priority: CheckBox, Low priority: ComboBox
            if (priorityValue > 5)
            {
                e.Style.CellType = "CheckBox";
                e.Style.CellValue = priorityValue > 7;
            }
            else
            {
                e.Style.CellType = "ComboBox";
                e.Style.ChoiceList = new string[] { "Low", "Medium", "Normal" };
            }
        }
    }
}
```

## Creating Custom Cell Types

### Step 1: Create Cell Model

```csharp
using Syncfusion.Windows.Forms.Grid;
using Syncfusion.Windows.Forms.Grid.Grouping;

public class LinkLabelCellModel : GridCellModelBase
{
    protected LinkLabelCellModel(SerializationInfo info, StreamingContext context)
        : base(info, context)
    {
    }

    public LinkLabelCellModel(GridModel grid)
        : base(grid)
    {
    }

    public override GridCellRendererBase CreateRenderer(GridControlBase control)
    {
        return new LinkLabelCellRenderer(control, this);
    }
}
```

### Step 2: Create Cell Renderer

```csharp
public class LinkLabelCellRenderer : GridCellRendererBase
{
    private LinkLabel linkLabel;

    public LinkLabelCellRenderer(GridControlBase grid, GridCellModelBase cellModel)
        : base(grid, cellModel)
    {
    }

    protected override void OnInitialize(int rowIndex, int colIndex)
    {
        base.OnInitialize(rowIndex, colIndex);
        
        // Create LinkLabel control
        if (linkLabel == null)
        {
            linkLabel = new LinkLabel();
            linkLabel.LinkClicked += LinkLabel_LinkClicked;
        }
        
        // Set link text
        linkLabel.Text = ControlText;
    }

    private void LinkLabel_LinkClicked(object sender, LinkLabelLinkClickedEventArgs e)
    {
        // Handle link click
        string url = linkLabel.Text;
        if (Uri.IsWellFormedUriString(url, UriKind.Absolute))
        {
            System.Diagnostics.Process.Start(url);
        }
    }

    protected override void OnDraw(Graphics g, Rectangle clientRectangle, 
        int rowIndex, int colIndex, GridStyleInfo style)
    {
        // Draw link label
        linkLabel.Bounds = clientRectangle;
        
        using (Bitmap bmp = new Bitmap(clientRectangle.Width, clientRectangle.Height))
        {
            linkLabel.DrawToBitmap(bmp, new Rectangle(0, 0, bmp.Width, bmp.Height));
            g.DrawImage(bmp, clientRectangle);
        }
    }

    protected override Rectangle OnGetCellContainerBounds(int rowIndex, int colIndex, GridStyleInfo style)
    {
        Rectangle r = base.OnGetCellContainerBounds(rowIndex, colIndex, style);
        if (linkLabel != null)
        {
            linkLabel.Bounds = r;
        }
        return r;
    }
}
```

### Step 3: Register Custom Cell Type

```csharp
// Register in grid initialization
LinkLabelCellModel linkLabelModel = new LinkLabelCellModel(gridGroupingControl1.TableModel);
gridGroupingControl1.TableModel.CellModels.Add("LinkLabel", linkLabelModel);
```

### Step 4: Use Custom Cell Type

```csharp
// Assign to column
gridGroupingControl1.TableDescriptor.Columns["Website"].Appearance.AnyRecordFieldCell.CellType = "LinkLabel";

// Or assign in QueryCellStyleInfo
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.Column?.Name == "Website")
    {
        e.Style.CellType = "LinkLabel";
    }
};
```

## Common Scenarios

### Scenario 1: Employee Form with Mixed Cell Types

```csharp
var columns = gridGroupingControl1.TableDescriptor.Columns;

// ID: Read-only
columns["EmployeeID"].Appearance.AnyRecordFieldCell.CellType = "Static";
columns["EmployeeID"].Appearance.AnyRecordFieldCell.ReadOnly = true;

// Name: TextBox (default)
columns["FirstName"].Appearance.AnyRecordFieldCell.CellType = "TextBox";
columns["LastName"].Appearance.AnyRecordFieldCell.CellType = "TextBox";

// Birth Date: DatePicker
columns["BirthDate"].Appearance.AnyRecordFieldCell.CellType = GridCellTypeName.DateTime;
columns["BirthDate"].Appearance.AnyRecordFieldCell.Format = "MM/dd/yyyy";

// Active: CheckBox
columns["IsActive"].Appearance.AnyRecordFieldCell.CellType = "CheckBox";

// Department: ComboBox
columns["Department"].Appearance.AnyRecordFieldCell.CellType = "ComboBox";
columns["Department"].Appearance.AnyRecordFieldCell.ChoiceList = new string[] 
{ 
    "Sales", "Marketing", "Engineering", "HR", "Finance" 
};

// Salary: Currency
columns["Salary"].Appearance.AnyRecordFieldCell.CellType = GridCellTypeName.Currency;
columns["Salary"].Appearance.AnyRecordFieldCell.Format = "C0";
```

### Scenario 2: Dynamic Cell Types Based on Data

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.Column?.Name == "Value")
    {
        Record record = e.TableCellIdentity.DisplayElement.GetRecord();
        string dataType = record.GetValue("DataType").ToString();
        
        switch (dataType)
        {
            case "Boolean":
                e.Style.CellType = "CheckBox";
                break;
            case "Date":
                e.Style.CellType = GridCellTypeName.DateTime;
                e.Style.Format = "MM/dd/yyyy";
                break;
            case "Currency":
                e.Style.CellType = GridCellTypeName.Currency;
                e.Style.Format = "C2";
                break;
            case "Choice":
                e.Style.CellType = "ComboBox";
                e.Style.ChoiceList = record.GetValue("Choices").ToString().Split(',');
                break;
            default:
                e.Style.CellType = "TextBox";
                break;
        }
    }
};
```

### Scenario 3: Rating Stars Custom Cell Type

```csharp
// Custom cell type showing 1-5 star rating
public class StarRatingCellRenderer : GridCellRendererBase
{
    public StarRatingCellRenderer(GridControlBase grid, GridCellModelBase cellModel)
        : base(grid, cellModel)
    {
    }

    protected override void OnDraw(Graphics g, Rectangle clientRectangle, 
        int rowIndex, int colIndex, GridStyleInfo style)
    {
        // Get rating value (1-5)
        int rating = 0;
        if (style.CellValue != null)
            int.TryParse(style.CellValue.ToString(), out rating);
        
        // Draw stars
        int starSize = 16;
        int spacing = 2;
        int x = clientRectangle.X + 5;
        int y = clientRectangle.Y + (clientRectangle.Height - starSize) / 2;
        
        for (int i = 0; i < 5; i++)
        {
            bool filled = i < rating;
            DrawStar(g, new Rectangle(x, y, starSize, starSize), filled);
            x += starSize + spacing;
        }
    }

    private void DrawStar(Graphics g, Rectangle rect, bool filled)
    {
        // Star drawing logic (simplified)
        using (Brush brush = new SolidBrush(filled ? Color.Gold : Color.LightGray))
        {
            g.FillEllipse(brush, rect); // Simplified as circle
        }
    }
}

// Register and use
StarRatingCellModel starModel = new StarRatingCellModel(gridGroupingControl1.TableModel);
gridGroupingControl1.TableModel.CellModels.Add("StarRating", starModel);

gridGroupingControl1.TableDescriptor.Columns["Rating"].Appearance.AnyRecordFieldCell.CellType = "StarRating";
```

## Best Practices

### Choosing Cell Types

1. **Match Data Type**: Use cell types that match underlying data:
   - Boolean → CheckBox
   - DateTime → DateTimePicker
   - Decimal → Currency/NumericEdit
   - Enum → ComboBox

2. **User Experience**: Choose cell types that simplify data entry:
   - ComboBox for predefined values (prevents typos)
   - MaskedEdit for formatted input (phone, SSN)
   - CheckBox for binary choices (clearer than Yes/No text)

3. **Performance**: Simple cell types (TextBox, Static) perform better than complex types (ComboBox, DateTimePicker) for large datasets.

### Custom Cell Types

1. **Reusability**: Create custom cell types for repeated patterns across application.

2. **Separation of Concerns**: Keep model (data logic) separate from renderer (visual logic).

3. **Event Handling**: In custom renderers, properly handle mouse/keyboard events to match grid behavior.

4. **Cleanup**: Dispose controls and resources in renderer's `Dispose()` method.

### Registration

1. **Register Once**: Register custom cell types during grid initialization, not repeatedly.

2. **Unique Names**: Use descriptive, unique names for custom cell types: "EmailLink", "ProgressBar", "ImageButton".

3. **Helper Classes**: Use `RegisterCellModel.GridGroupingCellType()` to register all enhanced types at once.

### Column Configuration

1. **Set at Column Level**: When all cells in a column use the same type, set at column level for performance.

2. **QueryCellStyleInfo for Variability**: Use `QueryCellStyleInfo` when cell types vary by row/condition.

3. **Test Editing**: Verify cell types work correctly in edit mode (Enter key, Tab, Escape).

4. **Accessibility**: Ensure custom cell types support keyboard navigation and screen readers.
