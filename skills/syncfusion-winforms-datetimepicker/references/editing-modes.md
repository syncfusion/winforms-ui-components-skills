# Editing Modes and Navigation

The SfDateTimeEdit control supports two distinct editing modes: Default text editing and Mask editing. Learn how to configure editing behavior, keyboard navigation, and field-by-field editing.

## Table of Contents

- [DateTimeEditingMode Property](#datetimeeditingmode-property)
- [Default Editing Mode](#default-editing-mode)
- [Mask Editing Mode](#mask-editing-mode)
- [ShowUpDown Property](#showupdown-property)
- [Keyboard Navigation](#keyboard-navigation)
- [Field Selection and Editing](#field-selection-and-editing)
- [Complete Examples](#complete-examples)
- [Next Steps](#next-steps)

## DateTimeEditingMode Property

The `DateTimeEditingMode` property controls how users can input and edit date-time values. It accepts two values:
- **Default**: Free-form text editing
- **Mask**: Field-by-field structured editing

**C#:**
```csharp
// Set to Default mode
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Default;

// Set to Mask mode
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
```

**VB.NET:**
```vb
' Set to Default mode
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Default

' Set to Mask mode
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
```

## Default Editing Mode

In Default mode, users can type date-time values freely in any valid format. The control automatically parses and validates the input.

### Characteristics

- **Free-form input**: Users can type dates in various formats
- **Auto-correction**: Text is automatically formatted when focus is lost or Enter is pressed
- **Flexible parsing**: Accepts multiple date formats
- **Format independence**: Input doesn't need to match display pattern exactly

### Example Usage

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Default;
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
dateTimeEdit.Value = new DateTime(2018, 6, 27);

// User can type: "Mar 28 2018" or "3/28/2018" or "28-03-2018"
// All will be converted to: "Wednesday, June 27, 2018"
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Default
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
dateTimeEdit.Value = New DateTime(2018, 6, 27)

' User can type: "Mar 28 2018" or "3/28/2018" or "28-03-2018"
' All will be converted to: "Wednesday, June 27, 2018"
```

### Advantages of Default Mode

- Quick data entry for experienced users
- Supports copy-paste operations
- Flexible input formats
- Natural typing experience

### Best Use Cases

- Data import scenarios
- Power user interfaces
- Quick date entry forms
- When input format varies

## Mask Editing Mode

In Mask mode, the date-time value is divided into separate fields (day, month, year, hour, minute, second). Users navigate and edit each field individually.

### Characteristics

- **Field-by-field editing**: Each component is a separate editable field
- **Structured input**: Users edit within the defined format
- **Arrow key navigation**: Use arrow keys to move between fields
- **Increment/decrement**: Use up/down arrows to change values
- **Visual separation**: Clear field boundaries

### Example Usage

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;
dateTimeEdit.Value = new DateTime(2018, 2, 1);

// Display: [02]/[01]/[2018]
// User navigates with Tab or arrow keys between fields
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate
dateTimeEdit.Value = New DateTime(2018, 2, 1)

' Display: [02]/[01]/[2018]
' User navigates with Tab or arrow keys between fields
```

### Advantages of Mask Mode

- Prevents invalid input
- Clearer field structure
- Better for novice users
- Supports null values (with AllowNull property)
- Works with up-down buttons

### Best Use Cases

- Forms requiring precise date entry
- Applications for non-technical users
- When null values are needed
- Time-critical data entry

## ShowUpDown Property

The `ShowUpDown` property displays increment/decrement buttons for changing field values. These buttons only appear in **Mask mode**.

**C#:**
```csharp
// Enable up-down buttons (only works in Mask mode)
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.ShowUpDown = true;
```

**VB.NET:**
```vb
' Enable up-down buttons (only works in Mask mode)
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.ShowUpDown = True
```

### Up-Down Button Behavior

- **Up button**: Increments the selected field value
- **Down button**: Decrements the selected field value
- **Auto-wrapping**: Values wrap around (e.g., 12 → 1 for months)
- **Range validation**: Respects MinDateTime and MaxDateTime

### Example with Up-Down

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
dateTimeEdit.Location = new System.Drawing.Point(50, 50);
dateTimeEdit.Size = new System.Drawing.Size(200, 30);

// Configure for mask mode with up-down buttons
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.ShowUpDown = true;
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;
dateTimeEdit.Value = DateTime.Today;

this.Controls.Add(dateTimeEdit);
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
dateTimeEdit.Location = New System.Drawing.Point(50, 50)
dateTimeEdit.Size = New System.Drawing.Size(200, 30)

' Configure for mask mode with up-down buttons
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.ShowUpDown = True
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate
dateTimeEdit.Value = DateTime.Today

Me.Controls.Add(dateTimeEdit)
```

## Keyboard Navigation

The SfDateTimeEdit provides comprehensive keyboard support, especially in Mask mode.

### Navigation Keys (Mask Mode)

| Key | Action |
|-----|--------|
| **Tab** | Move to next field |
| **Shift+Tab** | Move to previous field |
| **Right Arrow** | Move to next field |
| **Left Arrow** | Move to previous field |
| **Home** | Move to first field |
| **End** | Move to last field |
| **Up Arrow** | Increment current field value |
| **Down Arrow** | Decrement current field value |
| **Alt+Down** | Open drop-down calendar |
| **Alt+Up** | Close drop-down calendar |
| **Enter** | Apply changes and validate |
| **Escape** | Cancel changes |

### InterceptArrowKeys Property

Control whether arrow keys change values or move focus:

**C#:**
```csharp
// Enable arrow key value changes (default)
dateTimeEdit.InterceptArrowKeys = true;

// Disable arrow key value changes
dateTimeEdit.InterceptArrowKeys = false;
```

**VB.NET:**
```vb
' Enable arrow key value changes (default)
dateTimeEdit.InterceptArrowKeys = True

' Disable arrow key value changes
dateTimeEdit.InterceptArrowKeys = False
```

### Mouse Wheel Support

In Mask mode, you can change values using the mouse wheel:

**C#:**
```csharp
// Enable mouse wheel value changes
dateTimeEdit.AllowValueChangeOnMouseWheel = true;

// Disable mouse wheel value changes
dateTimeEdit.AllowValueChangeOnMouseWheel = false;
```

**VB.NET:**
```vb
' Enable mouse wheel value changes
dateTimeEdit.AllowValueChangeOnMouseWheel = True

' Disable mouse wheel value changes
dateTimeEdit.AllowValueChangeOnMouseWheel = False
```

## Field Selection and Editing

In Mask mode, you can programmatically access and control the selected field.

### SelectedField Property

The `SelectedField` property provides information about the currently selected field:

**C#:**
```csharp
// Get selected field information
if (dateTimeEdit.SelectedField != null)
{
    string fieldType = dateTimeEdit.SelectedField.FieldType.ToString();
    string fieldValue = dateTimeEdit.SelectedField.FieldValue;
    
    MessageBox.Show($"Field Type: {fieldType}\nField Value: {fieldValue}");
}
```

**VB.NET:**
```vb
' Get selected field information
If dateTimeEdit.SelectedField IsNot Nothing Then
    Dim fieldType As String = dateTimeEdit.SelectedField.FieldType.ToString()
    Dim fieldValue As String = dateTimeEdit.SelectedField.FieldValue
    
    MessageBox.Show($"Field Type: {fieldType}" & vbCrLf & $"Field Value: {fieldValue}")
End If
```

### Field Types

- **Day**: Day of the month (1-31)
- **Month**: Month of the year (1-12)
- **Year**: Year value
- **Hour**: Hour value (0-23 or 1-12)
- **Minute**: Minute value (0-59)
- **Second**: Second value (0-59)
- **Meridiem**: AM/PM designator

## Complete Examples

### Example 1: Mode Comparison Form

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class ModeComparisonForm : Form
{
    private SfDateTimeEdit defaultModeEdit;
    private SfDateTimeEdit maskModeEdit;
    
    public ModeComparisonForm()
    {
        this.Text = "Editing Mode Comparison";
        this.Size = new Size(450, 200);
        
        // Default mode control
        Label lblDefault = new Label();
        lblDefault.Text = "Default Mode (Free-form editing):";
        lblDefault.Location = new Point(20, 20);
        lblDefault.Size = new Size(250, 20);
        this.Controls.Add(lblDefault);
        
        defaultModeEdit = new SfDateTimeEdit();
        defaultModeEdit.Location = new Point(20, 45);
        defaultModeEdit.Size = new Size(380, 30);
        defaultModeEdit.DateTimeEditingMode = DateTimeEditingMode.Default;
        defaultModeEdit.DateTimePattern = DateTimePattern.LongDate;
        defaultModeEdit.Value = DateTime.Today;
        this.Controls.Add(defaultModeEdit);
        
        // Mask mode control
        Label lblMask = new Label();
        lblMask.Text = "Mask Mode (Field-by-field editing):";
        lblMask.Location = new Point(20, 90);
        lblMask.Size = new Size(250, 20);
        this.Controls.Add(lblMask);
        
        maskModeEdit = new SfDateTimeEdit();
        maskModeEdit.Location = new Point(20, 115);
        maskModeEdit.Size = new Size(380, 30);
        maskModeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        maskModeEdit.DateTimePattern = DateTimePattern.ShortDate;
        maskModeEdit.ShowUpDown = true;
        maskModeEdit.Value = DateTime.Today;
        this.Controls.Add(maskModeEdit);
        
        // Synchronize values
        defaultModeEdit.ValueChanged += (s, e) =>
        {
            if (defaultModeEdit.Value.HasValue)
                maskModeEdit.Value = defaultModeEdit.Value.Value;
        };
        
        maskModeEdit.ValueChanged += (s, e) =>
        {
            if (maskModeEdit.Value.HasValue)
                defaultModeEdit.Value = maskModeEdit.Value.Value;
        };
    }
}
```

### Example 2: Time Entry with Up-Down Buttons

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class TimeEntryForm : Form
{
    private SfDateTimeEdit timeEdit;
    private Label lblSelectedField;
    
    public TimeEntryForm()
    {
        this.Text = "Time Entry with Navigation";
        this.Size = new Size(400, 180);
        
        Label lblPrompt = new Label();
        lblPrompt.Text = "Enter Time (use arrow keys or up-down buttons):";
        lblPrompt.Location = new Point(20, 20);
        lblPrompt.Size = new Size(350, 20);
        this.Controls.Add(lblPrompt);
        
        timeEdit = new SfDateTimeEdit();
        timeEdit.Location = new Point(20, 50);
        timeEdit.Size = new Size(300, 30);
        timeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        timeEdit.DateTimePattern = DateTimePattern.LongTime;
        timeEdit.ShowUpDown = true;
        timeEdit.Value = DateTime.Now;
        
        // Monitor field selection
        timeEdit.Click += (s, e) => UpdateFieldInfo();
        timeEdit.KeyUp += (s, e) => UpdateFieldInfo();
        
        this.Controls.Add(timeEdit);
        
        // Field information display
        lblSelectedField = new Label();
        lblSelectedField.Location = new Point(20, 90);
        lblSelectedField.Size = new Size(350, 40);
        lblSelectedField.Font = new Font(lblSelectedField.Font.FontFamily, 9, FontStyle.Italic);
        this.Controls.Add(lblSelectedField);
        
        UpdateFieldInfo();
    }
    
    private void UpdateFieldInfo()
    {
        if (timeEdit.SelectedField != null)
        {
            lblSelectedField.Text = 
                $"Selected Field: {timeEdit.SelectedField.FieldType}\n" +
                $"Current Value: {timeEdit.SelectedField.FieldValue}\n" +
                $"Tip: Use ↑↓ arrows or mouse wheel to change, ←→ to navigate";
        }
    }
}
```

### Example 3: Advanced Navigation Control

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class NavigationControlForm : Form
{
    private SfDateTimeEdit dateTimeEdit;
    private CheckBox chkInterceptArrows;
    private CheckBox chkMouseWheel;
    private CheckBox chkShowUpDown;
    
    public NavigationControlForm()
    {
        this.Text = "Navigation Control Settings";
        this.Size = new Size(450, 250);
        
        // DateTime control
        Label lblEdit = new Label();
        lblEdit.Text = "Date/Time:";
        lblEdit.Location = new Point(20, 20);
        this.Controls.Add(lblEdit);
        
        dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(100, 20);
        dateTimeEdit.Size = new Size(300, 30);
        dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
        dateTimeEdit.Format = "MM/dd/yyyy hh:mm tt";
        dateTimeEdit.Value = DateTime.Now;
        this.Controls.Add(dateTimeEdit);
        
        // Navigation options
        Label lblOptions = new Label();
        lblOptions.Text = "Navigation Options:";
        lblOptions.Location = new Point(20, 70);
        lblOptions.Font = new Font(lblOptions.Font, FontStyle.Bold);
        this.Controls.Add(lblOptions);
        
        // Intercept arrow keys
        chkInterceptArrows = new CheckBox();
        chkInterceptArrows.Text = "Intercept Arrow Keys (enable ↑↓ value changes)";
        chkInterceptArrows.Location = new Point(40, 100);
        chkInterceptArrows.Size = new Size(350, 20);
        chkInterceptArrows.Checked = true;
        chkInterceptArrows.CheckedChanged += (s, e) =>
        {
            dateTimeEdit.InterceptArrowKeys = chkInterceptArrows.Checked;
        };
        this.Controls.Add(chkInterceptArrows);
        
        // Mouse wheel
        chkMouseWheel = new CheckBox();
        chkMouseWheel.Text = "Allow Mouse Wheel Value Changes";
        chkMouseWheel.Location = new Point(40, 130);
        chkMouseWheel.Size = new Size(350, 20);
        chkMouseWheel.Checked = true;
        chkMouseWheel.CheckedChanged += (s, e) =>
        {
            dateTimeEdit.AllowValueChangeOnMouseWheel = chkMouseWheel.Checked;
        };
        this.Controls.Add(chkMouseWheel);
        
        // Show up-down
        chkShowUpDown = new CheckBox();
        chkShowUpDown.Text = "Show Up-Down Buttons";
        chkShowUpDown.Location = new Point(40, 160);
        chkShowUpDown.Size = new Size(350, 20);
        chkShowUpDown.Checked = false;
        chkShowUpDown.CheckedChanged += (s, e) =>
        {
            dateTimeEdit.ShowUpDown = chkShowUpDown.Checked;
        };
        this.Controls.Add(chkShowUpDown);
    }
}
```

## ReadOnly Mode

Restrict editing while still allowing value changes via buttons or calendar:

**C#:**
```csharp
// Set to read-only
dateTimeEdit.ReadOnly = true;

// Users can still:
// - Click up-down buttons (if ShowUpDown = true)
// - Select from drop-down calendar
// But cannot type in the text box
```

**VB.NET:**
```vb
' Set to read-only
dateTimeEdit.ReadOnly = True

' Users can still:
' - Click up-down buttons (if ShowUpDown = True)
' - Select from drop-down calendar
' But cannot type in the text box
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and usage
- **[Date Range and Value](date-range-value.md)** - Manage date-time values
- **[Display Patterns](display-patterns.md)** - Format date-time display
- **[Appearance Styling](appearance-styling.md)** - Customize visual appearance
- **[Validation Features](validation-features.md)** - Implement validation and error handling
