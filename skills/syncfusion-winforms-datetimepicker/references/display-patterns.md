# Display Patterns and Formatting

The SfDateTimeEdit control provides flexible options to display date-time values in various formats using the DateTimePattern property and custom format strings.

## Table of Contents

- [DateTimePattern Property](#datetimepattern-property)
- [Built-in Patterns](#built-in-patterns)
- [Custom Patterns](#custom-patterns)
- [Custom Format Specifiers](#custom-format-specifiers)
- [Culture-based Formatting](#culture-based-formatting)
- [Complete Examples](#complete-examples)
- [Next Steps](#next-steps)

## DateTimePattern Property

The `DateTimePattern` property specifies how the date and time should be displayed in the SfDateTimeEdit control. It accepts values from the `DateTimePattern` enumeration.

**C#:**
```csharp
// Set display pattern
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
```

**VB.NET:**
```vb
' Set display pattern
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
```

## Built-in Patterns

The SfDateTimeEdit supports the following built-in date-time patterns:

### LongDate

Displays the full date with day of week, month name, day, and year.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5);
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
// Output: Thursday, July 05, 2018
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5)
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
' Output: Thursday, July 05, 2018
```

### ShortDate

Displays the date in numeric format (based on culture).

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5);
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;
// Output: 7/5/2018 (US culture)
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5)
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate
' Output: 7/5/2018 (US culture)
```

### LongTime

Displays the time with hours, minutes, seconds, and AM/PM designation.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.LongTime;
// Output: 2:30:45 PM
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.LongTime
' Output: 2:30:45 PM
```

### ShortTime

Displays the time with hours and minutes only.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.ShortTime;
// Output: 2:30 PM
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.ShortTime
' Output: 2:30 PM
```

### FullDateTime

Displays both the full date and time.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.FullDateTime;
// Output: Thursday, July 05, 2018 2:30:45 PM
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.FullDateTime
' Output: Thursday, July 05, 2018 2:30:45 PM
```

### MonthDay

Displays the month name and day.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5);
dateTimeEdit.DateTimePattern = DateTimePattern.MonthDay;
// Output: July 05
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5)
dateTimeEdit.DateTimePattern = DateTimePattern.MonthDay
' Output: July 05
```

### YearMonth

Displays the month name and year.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5);
dateTimeEdit.DateTimePattern = DateTimePattern.YearMonth;
// Output: July, 2018
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5)
dateTimeEdit.DateTimePattern = DateTimePattern.YearMonth
' Output: July, 2018
```

### ShortableDateTime

Displays date and time in sortable format (ISO 8601).

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.ShortableDateTime;
// Output: 2018-07-05T14:30:45
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.ShortableDateTime
' Output: 2018-07-05T14:30:45
```

### UniversalShortableDateTime

Displays date and time in universal sortable format.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.UniversalShortableDateTime;
// Output: 2018-07-05 14:30:45Z
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.UniversalShortableDateTime
' Output: 2018-07-05 14:30:45Z
```

### RFC1123

Displays date and time in RFC 1123 format.

**C#:**
```csharp
dateTimeEdit.Value = new DateTime(2018, 7, 5, 14, 30, 45);
dateTimeEdit.DateTimePattern = DateTimePattern.RFC1123;
// Output: Thu, 05 Jul 2018 14:30:45 GMT
```

**VB.NET:**
```vb
dateTimeEdit.Value = New DateTime(2018, 7, 5, 14, 30, 45)
dateTimeEdit.DateTimePattern = DateTimePattern.RFC1123
' Output: Thu, 05 Jul 2018 14:30:45 GMT
```

## Custom Patterns

Create your own display format using the `Custom` pattern type and the `Format` property.

**C#:**
```csharp
// Set to custom pattern mode
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;

// Define custom format
dateTimeEdit.Format = "MM/dd/yy hh:mm:ss";
dateTimeEdit.Value = new DateTime(2018, 2, 5, 14, 30, 45);
// Output: 02/05/18 02:30:45
```

**VB.NET:**
```vb
' Set to custom pattern mode
dateTimeEdit.DateTimePattern = DateTimePattern.Custom

' Define custom format
dateTimeEdit.Format = "MM/dd/yy hh:mm:ss"
dateTimeEdit.Value = New DateTime(2018, 2, 5, 14, 30, 45)
' Output: 02/05/18 02:30:45
```

## Custom Format Specifiers

Use these specifiers to create custom date-time formats:

| Specifier | Description | Example (for July 5, 2018, 2:30:45 PM) |
|-----------|-------------|----------------------------------------|
| **d** | Day of month (1-31) | 5 |
| **dd** | Day of month (01-31) | 05 |
| **ddd** | Abbreviated day name | Thu |
| **dddd** | Full day name | Thursday |
| **M** | Month (1-12) | 7 |
| **MM** | Month (01-12) | 07 |
| **MMM** | Abbreviated month name | Jul |
| **MMMM** | Full month name | July |
| **yy** | Year (last two digits) | 18 |
| **yyyy** | Year (four digits) | 2018 |
| **h** | Hour in 12-hour format (1-12) | 2 |
| **hh** | Hour in 12-hour format (01-12) | 02 |
| **H** | Hour in 24-hour format (0-23) | 14 |
| **HH** | Hour in 24-hour format (00-23) | 14 |
| **m** | Minute (0-59) | 30 |
| **mm** | Minute (00-59) | 30 |
| **s** | Second (0-59) | 45 |
| **ss** | Second (00-59) | 45 |
| **tt** | AM/PM designator | PM |

### Custom Pattern Examples

**Example 1: European Date Format**
```csharp
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "dd.MM.yyyy";
// Output: 05.07.2018
```

**Example 2: Date with Text**
```csharp
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "dddd, MMMM d, yyyy";
// Output: Thursday, July 5, 2018
```

**Example 3: Time with Date**
```csharp
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "MM/dd/yyyy hh:mm tt";
// Output: 07/05/2018 02:30 PM
```

**Example 4: 24-hour Time Format**
```csharp
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "dd-MMM-yyyy HH:mm:ss";
// Output: 05-Jul-2018 14:30:45
```

**Example 5: ISO 8601 Format**
```csharp
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "yyyy-MM-ddTHH:mm:ss";
// Output: 2018-07-05T14:30:45
```

## Culture-based Formatting

The display format automatically adjusts based on the Culture property setting.

**C#:**
```csharp
using System.Globalization;

// US English culture
dateTimeEdit.Culture = new CultureInfo("en-US");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
dateTimeEdit.Value = new DateTime(2018, 7, 5);
// Output: Thursday, July 05, 2018

// French culture
dateTimeEdit.Culture = new CultureInfo("fr-FR");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
dateTimeEdit.Value = new DateTime(2018, 7, 5);
// Output: jeudi 5 juillet 2018

// German culture
dateTimeEdit.Culture = new CultureInfo("de-DE");
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;
dateTimeEdit.Value = new DateTime(2018, 7, 5);
// Output: 05.07.2018
```

**VB.NET:**
```vb
Imports System.Globalization

' US English culture
dateTimeEdit.Culture = New CultureInfo("en-US")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
dateTimeEdit.Value = New DateTime(2018, 7, 5)
' Output: Thursday, July 05, 2018

' French culture
dateTimeEdit.Culture = New CultureInfo("fr-FR")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
dateTimeEdit.Value = New DateTime(2018, 7, 5)
' Output: jeudi 5 juillet 2018

' German culture
dateTimeEdit.Culture = New CultureInfo("de-DE")
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate
dateTimeEdit.Value = New DateTime(2018, 7, 5)
' Output: 05.07.2018
```

## Complete Examples

### Example 1: Pattern Selector Application

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class PatternSelectorForm : Form
{
    private SfDateTimeEdit dateTimeEdit;
    private ComboBox cmbPattern;
    
    public PatternSelectorForm()
    {
        this.Text = "DateTime Pattern Selector";
        this.Size = new Size(500, 200);
        
        // Pattern selector
        Label lblPattern = new Label();
        lblPattern.Text = "Select Pattern:";
        lblPattern.Location = new Point(20, 20);
        this.Controls.Add(lblPattern);
        
        cmbPattern = new ComboBox();
        cmbPattern.Location = new Point(120, 20);
        cmbPattern.Size = new Size(300, 25);
        cmbPattern.DropDownStyle = ComboBoxStyle.DropDownList;
        cmbPattern.Items.AddRange(new object[]
        {
            "LongDate",
            "ShortDate",
            "LongTime",
            "ShortTime",
            "FullDateTime",
            "MonthDay",
            "YearMonth",
            "Custom: MM/dd/yyyy hh:mm tt",
            "Custom: dd-MMM-yyyy",
            "Custom: yyyy-MM-dd HH:mm:ss"
        });
        cmbPattern.SelectedIndex = 0;
        cmbPattern.SelectedIndexChanged += Pattern_Changed;
        this.Controls.Add(cmbPattern);
        
        // DateTime editor
        Label lblEdit = new Label();
        lblEdit.Text = "Date/Time:";
        lblEdit.Location = new Point(20, 60);
        this.Controls.Add(lblEdit);
        
        dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(120, 60);
        dateTimeEdit.Size = new Size(300, 25);
        dateTimeEdit.Value = DateTime.Now;
        dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
        this.Controls.Add(dateTimeEdit);
    }
    
    private void Pattern_Changed(object sender, EventArgs e)
    {
        string selection = cmbPattern.SelectedItem.ToString();
        
        if (selection.StartsWith("Custom:"))
        {
            dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
            dateTimeEdit.Format = selection.Substring(8).Trim();
        }
        else
        {
            DateTimePattern pattern = (DateTimePattern)Enum.Parse(
                typeof(DateTimePattern), selection);
            dateTimeEdit.DateTimePattern = pattern;
        }
    }
}
```

### Example 2: Multi-Culture Date Display

**C#:**
```csharp
using System;
using System.Drawing;
using System.Globalization;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class MultiCultureForm : Form
{
    private SfDateTimeEdit usDateEdit;
    private SfDateTimeEdit ukDateEdit;
    private SfDateTimeEdit frDateEdit;
    private SfDateTimeEdit deDateEdit;
    
    public MultiCultureForm()
    {
        this.Text = "Multi-Culture Date Display";
        this.Size = new Size(400, 250);
        
        DateTime currentDate = DateTime.Now;
        int yPos = 20;
        
        // US format
        AddDateControl("US Format:", "en-US", ref usDateEdit, yPos);
        yPos += 40;
        
        // UK format
        AddDateControl("UK Format:", "en-GB", ref ukDateEdit, yPos);
        yPos += 40;
        
        // French format
        AddDateControl("French Format:", "fr-FR", ref frDateEdit, yPos);
        yPos += 40;
        
        // German format
        AddDateControl("German Format:", "de-DE", ref deDateEdit, yPos);
        
        // Sync all controls
        usDateEdit.ValueChanged += (s, e) => SyncDates(usDateEdit);
        ukDateEdit.ValueChanged += (s, e) => SyncDates(ukDateEdit);
        frDateEdit.ValueChanged += (s, e) => SyncDates(frDateEdit);
        deDateEdit.ValueChanged += (s, e) => SyncDates(deDateEdit);
    }
    
    private void AddDateControl(string label, string culture, 
                                ref SfDateTimeEdit control, int yPos)
    {
        Label lbl = new Label();
        lbl.Text = label;
        lbl.Location = new Point(20, yPos);
        lbl.Size = new Size(100, 20);
        this.Controls.Add(lbl);
        
        control = new SfDateTimeEdit();
        control.Location = new Point(130, yPos);
        control.Size = new Size(220, 25);
        control.Value = DateTime.Now;
        control.DateTimePattern = DateTimePattern.LongDate;
        control.Culture = new CultureInfo(culture);
        this.Controls.Add(control);
    }
    
    private void SyncDates(SfDateTimeEdit source)
    {
        if (source.Value.HasValue)
        {
            DateTime value = source.Value.Value;
            
            if (source != usDateEdit) usDateEdit.Value = value;
            if (source != ukDateEdit) ukDateEdit.Value = value;
            if (source != frDateEdit) frDateEdit.Value = value;
            if (source != deDateEdit) deDateEdit.Value = value;
        }
    }
}
```

### Example 3: Custom Format Builder

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class FormatBuilderForm : Form
{
    private SfDateTimeEdit dateTimeEdit;
    private TextBox txtFormat;
    private Label lblPreview;
    
    public FormatBuilderForm()
    {
        this.Text = "Custom Format Builder";
        this.Size = new Size(450, 250);
        
        // Format input
        Label lblFormat = new Label();
        lblFormat.Text = "Format String:";
        lblFormat.Location = new Point(20, 20);
        this.Controls.Add(lblFormat);
        
        txtFormat = new TextBox();
        txtFormat.Location = new Point(20, 45);
        txtFormat.Size = new Size(380, 25);
        txtFormat.Text = "MM/dd/yyyy hh:mm:ss tt";
        txtFormat.TextChanged += Format_Changed;
        this.Controls.Add(txtFormat);
        
        // Format help
        Label lblHelp = new Label();
        lblHelp.Text = "Format specifiers: d, dd, ddd, dddd, M, MM, MMM, MMMM, " +
                      "yy, yyyy, h, hh, H, HH, m, mm, s, ss, tt";
        lblHelp.Location = new Point(20, 75);
        lblHelp.Size = new Size(380, 40);
        this.Controls.Add(lblHelp);
        
        // DateTime editor
        Label lblEdit = new Label();
        lblEdit.Text = "Date/Time:";
        lblEdit.Location = new Point(20, 125);
        this.Controls.Add(lblEdit);
        
        dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(20, 150);
        dateTimeEdit.Size = new Size(380, 25);
        dateTimeEdit.Value = DateTime.Now;
        dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
        dateTimeEdit.Format = txtFormat.Text;
        this.Controls.Add(dateTimeEdit);
        
        // Preview
        lblPreview = new Label();
        lblPreview.Location = new Point(20, 185);
        lblPreview.Size = new Size(380, 20);
        lblPreview.Font = new Font(lblPreview.Font, FontStyle.Bold);
        this.Controls.Add(lblPreview);
        
        UpdatePreview();
    }
    
    private void Format_Changed(object sender, EventArgs e)
    {
        try
        {
            dateTimeEdit.Format = txtFormat.Text;
            UpdatePreview();
        }
        catch
        {
            lblPreview.Text = "Invalid format string";
            lblPreview.ForeColor = Color.Red;
        }
    }
    
    private void UpdatePreview()
    {
        if (dateTimeEdit.Value.HasValue)
        {
            lblPreview.Text = "Result: " + dateTimeEdit.DateTimeText;
            lblPreview.ForeColor = Color.Green;
        }
    }
}
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and usage
- **[Date Range and Value](date-range-value.md)** - Manage date-time values
- **[Editing Modes](editing-modes.md)** - Configure editing behavior
- **[Appearance Styling](appearance-styling.md)** - Customize visual appearance
- **[Validation Features](validation-features.md)** - Implement validation and globalization
