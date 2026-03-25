# Cell Types

## Table of Contents
- [Overview](#overview)
- [Setting Cell Types](#setting-cell-types)
- [Built-in Cell Types](#built-in-cell-types)
  - [TextBox](#textbox-default)
  - [Static](#static)
  - [Header](#header)
  - [CheckBox](#checkbox)
  - [ComboBox](#combobox)
  - [PushButton](#pushbutton)
  - [DateTimePicker](#datetimepicker)
  - [NumericUpDown](#numericupdown)
  - [Image](#image)
  - [ProgressBar](#progressbar)
  - [Other Types](#other-types)
- [Configuring Cell Types](#configuring-cell-types)
- [Custom Cell Types](#custom-cell-types)

## Overview

GridControl supports 15+ built-in cell types that transform cells into specialized controls like checkboxes, dropdowns, date pickers, and more. Each cell type has its own renderer and model that controls appearance and behavior.

**Architecture:**
- `GridCellModelBase` - Data/model part of cell type
- `GridCellRendererBase` - Rendering/visual part of cell type

**Default cell type:** TextBox

## Setting Cell Types

### Using GridCellTypeName:

```csharp
// Recommended approach using static class
gridControl1[2, 2].CellType = GridCellTypeName.CheckBox;
gridControl1[3, 2].CellType = GridCellTypeName.ComboBox;
gridControl1[4, 2].CellType = GridCellTypeName.PushButton;
```

### Using String:

```csharp
// Alternative using string literals
gridControl1[2, 2].CellType = "CheckBox";
gridControl1[3, 2].CellType = "ComboBox";
```

**Best practice:** Use `GridCellTypeName` for intellisense and compile-time checking.

## Built-in Cell Types

### TextBox (Default)

Displays editable text with optional images.

```csharp
// Simple text
gridControl1[2, 2].CellType = GridCellTypeName.TextBox;
gridControl1[2, 2].Text = "Editable Text";

// Text with image
gridControl1[3, 2].CellType = GridCellTypeName.TextBox;
gridControl1[3, 2].Text = "Text with Icon";
gridControl1[3, 2].ImageIndex = 0;
```

**Features:**
- Editable text input
- Supports images
- Multiline support
- Validation

### Static

Non-editable text display with optional images.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.Static;
gridControl1[2, 2].Text = "Read-Only Text";

// With image
gridControl1[3, 2].CellType = GridCellTypeName.Static;
gridControl1[3, 2].Text = "Static with Icon";
gridControl1[3, 2].ImageIndex = 2;
```

**Use when:**
- Display-only information
- Labels or titles
- Calculated values
- Status displays

**Note:** Can be selected but not edited (can be deleted with DELETE key).

### Header

Button-like header cells with depressed state.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.Header;
gridControl1[2, 2].Text = "Column Header";
```

**Use for:**
- Custom column headers
- Custom row headers
- Section dividers
- Clickable headers

### CheckBox

Boolean checkbox control.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.CheckBox;
gridControl1[2, 2].CellValue = true;
gridControl1[2, 2].Description = "Enable Feature";

// Checked state
gridControl1[3, 2].CellType = GridCellTypeName.CheckBox;
gridControl1[3, 2].CellValue = "True";  // or boolean true

// Unchecked state
gridControl1[4, 2].CellType = GridCellTypeName.CheckBox;
gridControl1[4, 2].CellValue = "False";  // or boolean false
```

**Properties:**
- `CellValue` - "True"/"False" or boolean
- `Description` - Label text next to checkbox
- `CheckBoxOptions` - Checkbox appearance settings

### ComboBox

Dropdown selection list.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.ComboBox;
gridControl1[2, 2].CellValue = "Option 1";
gridControl1[2, 2].ChoiceList = new string[] { "Option 1", "Option 2", "Option 3" };

// Dropdown-only (no text entry)
gridControl1[3, 2].CellType = GridCellTypeName.ComboBox;
gridControl1[3, 2].ChoiceList = new string[] { "Small", "Medium", "Large" };
gridControl1[3, 2].CellValue = "Medium";
gridControl1[3, 2].DropDownStyle = GridDropDownStyle.Exclusive;  // No typing allowed
```

**Properties:**
- `ChoiceList` - Items to display
- `CellValue` - Selected item
- `DropDownStyle` - Editable or dropdown-only
- `DropDownLines` - Number of visible items

### PushButton

Clickable button cell.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.PushButton;
gridControl1[2, 2].CellValue = "Click Me";
gridControl1[2, 2].Description = "Button1";  // Used to identify button

// Handle button click
gridControl1.CellButtonClicked += (sender, e) =>
{
    if (e.ColIndex == 2 && e.RowIndex == 2)
    {
        MessageBox.Show("Button clicked!");
    }
};
```

**Use for:**
- Action triggers
- Dialog launchers
- Custom commands
- Row operations (delete, edit, etc.)

### DateTimePicker

Date and time selection.

```csharp
// Date picker
gridControl1[2, 2].CellType = GridCellTypeName.MonthCalendar;
gridControl1[2, 2].CellValue = DateTime.Now;

// DateTime picker with time
gridControl1[3, 2].CellType = GridCellTypeName.DateTimePicker;
gridControl1[3, 2].CellValue = DateTime.Now;
gridControl1[3, 2].Format = "MM/dd/yyyy HH:mm";
```

**Cell type names:**
- `MonthCalendar` - Calendar dropdown
- `DateTimePicker` - Date and time picker

### NumericUpDown

Numeric input with up/down arrows.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.NumericUpDown;
gridControl1[2, 2].CellValue = 50;

// Configure range
gridControl1[2, 2].NumericUpDownProps.MinValue = 0;
gridControl1[2, 2].NumericUpDownProps.MaxValue = 100;
gridControl1[2, 2].NumericUpDownProps.Increment = 5;
```

**Use for:**
- Quantity input
- Age selection
- Numeric parameters
- Spin button controls

### Image

Display images in cells.

```csharp
// From ImageList
gridControl1[2, 2].CellType = GridCellTypeName.Image;
gridControl1[2, 2].ImageIndex = 0;

// From file
gridControl1[3, 2].CellType = GridCellTypeName.Image;
gridControl1[3, 2].CellValue = Image.FromFile("icon.png");
```

### ProgressBar

Visual progress indicator.

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.ProgressBar;
gridControl1[2, 2].CellValue = 75;  // 75% complete

// Configure appearance
gridControl1[2, 2].ProgressBarProps.Minimum = 0;
gridControl1[2, 2].ProgressBarProps.Maximum = 100;
gridControl1[2, 2].ProgressBarProps.ProgressColor = Color.Green;
```

**Use for:**
- Task completion status
- Loading indicators
- Percentage displays
- Data visualization

### Other Types

**CurrencyEdit:**
```csharp
gridControl1[2, 2].CellType = GridCellTypeName.Currency;
gridControl1[2, 2].CellValue = 1234.56;
gridControl1[2, 2].Format = "C";
```

**MaskEdit:**
```csharp
gridControl1[2, 2].CellType = GridCellTypeName.MaskEdit;
gridControl1[2, 2].Text = "___-__-____";  // SSN format
```

**Link:**
```csharp
gridControl1[2, 2].CellType = GridCellTypeName.Link;
gridControl1[2, 2].Text = "Click Here";
gridControl1[2, 2].Tag = "https://example.com";
```

## Configuring Cell Types

### ComboBox Configuration:

```csharp
// Populate from data source
List<string> items = GetItemList();
gridControl1[2, 2].CellType = GridCellTypeName.ComboBox;
gridControl1[2, 2].ChoiceList = items.ToArray();

// Dropdown style
gridControl1[2, 2].DropDownStyle = GridDropDownStyle.Exclusive;  // No typing
gridControl1[3, 2].DropDownStyle = GridDropDownStyle.AutoComplete;  // With typing

// Dropdown appearance
gridControl1[2, 2].DropDownLines = 10;  // Show 10 items
```

### CheckBox Configuration:

```csharp
// Appearance
gridControl1[2, 2].CheckBoxOptions.CheckedValue = "Yes";
gridControl1[2, 2].CheckBoxOptions.UncheckedValue = "No";

// Three-state checkbox
gridControl1[3, 2].CheckBoxOptions.ThreeState = true;
gridControl1[3, 2].CellValue = "Indeterminate";
```

### DateTimePicker Configuration:

```csharp
gridControl1[2, 2].CellType = GridCellTypeName.DateTimePicker;
gridControl1[2, 2].CellValue = DateTime.Now;

// Format
gridControl1[2, 2].Format = "MMMM dd, yyyy";  // December 31, 2023

// Min/Max dates
gridControl1[2, 2].DateTimePickerProps.MinDateTime = new DateTime(2020, 1, 1);
gridControl1[2, 2].DateTimePickerProps.MaxDateTime = new DateTime(2030, 12, 31);
```

### Button Configuration:

```csharp
// Button appearance
gridControl1[2, 2].CellType = GridCellTypeName.PushButton;
gridControl1[2, 2].CellValue = "Delete";
gridControl1[2, 2].BackColor = Color.Red;
gridControl1[2, 2].TextColor = Color.White;

// Handle clicks
gridControl1.CellButtonClicked += HandleButtonClick;

private void HandleButtonClick(object sender, GridCellButtonClickedEventArgs e)
{
    string buttonText = gridControl1[e.RowIndex, e.ColIndex].CellValue.ToString();
    MessageBox.Show($"Button '{buttonText}' clicked at {e.RowIndex},{e.ColIndex}");
}
```

## Custom Cell Types

Create custom cell types for specialized needs.

### Basic Custom Cell Type:

```csharp
// Create custom cell model
public class CustomCellModel : GridCellModelBase
{
    public CustomCellModel(GridModel grid) : base(grid)
    {
    }
    
    public override GridCellRendererBase CreateRenderer(GridControlBase control)
    {
        return new CustomCellRenderer(control, this);
    }
}

// Create custom cell renderer
public class CustomCellRenderer : GridCellRendererBase
{
    public CustomCellRenderer(GridControlBase grid, GridCellModelBase cellModel) 
        : base(grid, cellModel)
    {
    }
    
    protected override void OnDraw(Graphics g, Rectangle clientRectangle, 
        int rowIndex, int colIndex, GridStyleInfo style)
    {
        // Custom drawing logic
        base.OnDraw(g, clientRectangle, rowIndex, colIndex, style);
    }
}

// Register custom cell type
GridCellModelBase customModel = new CustomCellModel(gridControl1.Model);
gridControl1.CellModels.Add("CustomCell", customModel);

// Use custom cell type
gridControl1[2, 2].CellType = "CustomCell";
```

## Common Patterns

### Data Entry Form:

```csharp
// Name (TextBox)
gridControl1[1, 1].Text = "Name:";
gridControl1[1, 2].CellType = GridCellTypeName.TextBox;

// Age (NumericUpDown)
gridControl1[2, 1].Text = "Age:";
gridControl1[2, 2].CellType = GridCellTypeName.NumericUpDown;
gridControl1[2, 2].NumericUpDownProps.MinValue = 0;
gridControl1[2, 2].NumericUpDownProps.MaxValue = 120;

// Gender (ComboBox)
gridControl1[3, 1].Text = "Gender:";
gridControl1[3, 2].CellType = GridCellTypeName.ComboBox;
gridControl1[3, 2].ChoiceList = new string[] { "Male", "Female", "Other" };

// Birth Date (DateTimePicker)
gridControl1[4, 1].Text = "Birth Date:";
gridControl1[4, 2].CellType = GridCellTypeName.MonthCalendar;

// Active (CheckBox)
gridControl1[5, 1].Text = "Active:";
gridControl1[5, 2].CellType = GridCellTypeName.CheckBox;

// Submit (Button)
gridControl1[6, 2].CellType = GridCellTypeName.PushButton;
gridControl1[6, 2].CellValue = "Submit";
```

### Dynamic Cell Types:

```csharp
// Set cell type based on data
private void SetCellType(int row, int col, object value)
{
    if (value is bool)
    {
        gridControl1[row, col].CellType = GridCellTypeName.CheckBox;
    }
    else if (value is DateTime)
    {
        gridControl1[row, col].CellType = GridCellTypeName.MonthCalendar;
    }
    else if (value is int || value is double)
    {
        gridControl1[row, col].CellType = GridCellTypeName.NumericUpDown;
    }
    else
    {
        gridControl1[row, col].CellType = GridCellTypeName.TextBox;
    }
    
    gridControl1[row, col].CellValue = value;
}
```

## Best Practices

1. **Choose appropriate cell types** for data types
2. **Configure constraints** (min/max, choice lists)
3. **Handle events** for buttons and validation
4. **Use Static cells** for read-only display
5. **Set ChoiceList** before setting ComboBox values
6. **Provide user feedback** for invalid inputs
7. **Consider performance** with complex custom types

## Troubleshooting

### ComboBox not showing items
- Ensure `ChoiceList` is set before `CellValue`
- Verify items are strings
- Check `DropDownLines` property

### CheckBox showing text instead of checkbox
- Verify `CellType` is set to `GridCellTypeName.CheckBox`
- Check `CellValue` is boolean or "True"/"False" string

### Button not responding to clicks
- Ensure `CellButtonClicked` event is subscribed
- Verify `CellType` is `PushButton`
- Check event handler parameters

### DateTimePicker not appearing
- Use `MonthCalendar` or `DateTimePicker` cell type
- Set `CellValue` to DateTime object
- Check date is within min/max range

## Next Steps

- Implement data validation for cell types
- Create custom cell renderers
- Handle cell type events
- Configure cell type properties
- Build complex data entry forms
