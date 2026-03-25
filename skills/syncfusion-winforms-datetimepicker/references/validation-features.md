# Validation and Advanced Features

Learn about validation, error handling, watermarks, globalization, right-to-left support, and readonly functionality in the SfDateTimeEdit control.

## Table of Contents

- [Validation Behavior](#validation-behavior)
- [Validation Events](#validation-events)
- [Validation Reset Options](#validation-reset-options)
- [Watermark for Null Values](#watermark-for-null-values)
- [Globalization Support](#globalization-support)
- [Right-to-Left Support](#right-to-left-support)
- [ReadOnly Property](#readonly-property)
- [Complete Examples](#complete-examples)
- [Next Steps](#next-steps)

## Validation Behavior

The SfDateTimeEdit control validates date-time values automatically when:
- The Enter key is pressed
- The control loses focus
- A date is selected from the drop-down calendar

Validation checks for:
- Invalid date-time format
- Values outside MinDateTime and MaxDateTime range
- Invalid dates (e.g., February 30)

## Validation Events

### Validating Event

The `Validating` event provides detailed information about validation failures through the `ValidatingEventArgs`.

**Event Properties:**
- **IsError**: Boolean indicating if validation failed
- **ErrorMessage**: Descriptive error message explaining the failure

**C#:**
```csharp
// Subscribe to the Validating event
dateTimeEdit.Validating += DateTimeEdit_Validating;

private void DateTimeEdit_Validating(object sender, ValidatingEventArgs e)
{
    if (e.IsError)
    {
        // Display error message
        MessageBox.Show(
            e.ErrorMessage,
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        );
        
        // Log the error
        Console.WriteLine($"Validation failed: {e.ErrorMessage}");
    }
    else
    {
        // Validation passed
        Console.WriteLine("Date-time value is valid");
    }
}
```

**VB.NET:**
```vb
' Subscribe to the Validating event
AddHandler dateTimeEdit.Validating, AddressOf DateTimeEdit_Validating

Private Sub DateTimeEdit_Validating(sender As Object, e As ValidatingEventArgs)
    If e.IsError Then
        ' Display error message
        MessageBox.Show(
            e.ErrorMessage,
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        )
        
        ' Log the error
        Console.WriteLine($"Validation failed: {e.ErrorMessage}")
    Else
        ' Validation passed
        Console.WriteLine("Date-time value is valid")
    End If
End Sub
```

## Validation Reset Options

The `ValidationOption` property determines how the control behaves when validation fails.

### ValidationOption Values

- **Reset**: Maintains the previous valid value
- **MinValue**: Resets to MinDateTime when validation fails
- **MaxValue**: Resets to MaxDateTime when validation fails

### Reset Option (Default)

**C#:**
```csharp
dateTimeEdit.ValidationOption = ValidationResetOption.Reset;
dateTimeEdit.Value = new DateTime(2018, 2, 15);

// If user enters invalid date, value reverts to 2018-02-15
```

**VB.NET:**
```vb
dateTimeEdit.ValidationOption = ValidationResetOption.Reset
dateTimeEdit.Value = New DateTime(2018, 2, 15)

' If user enters invalid date, value reverts to 2018-02-15
```

### MinValue Option

**C#:**
```csharp
dateTimeEdit.MinDateTime = new DateTime(2018, 1, 1);
dateTimeEdit.MaxDateTime = new DateTime(2018, 12, 31);
dateTimeEdit.ValidationOption = ValidationResetOption.MinValue;

// If validation fails, value resets to 2018-01-01
```

**VB.NET:**
```vb
dateTimeEdit.MinDateTime = New DateTime(2018, 1, 1)
dateTimeEdit.MaxDateTime = New DateTime(2018, 12, 31)
dateTimeEdit.ValidationOption = ValidationResetOption.MinValue

' If validation fails, value resets to 2018-01-01
```

### MaxValue Option

**C#:**
```csharp
dateTimeEdit.MinDateTime = new DateTime(2018, 1, 1);
dateTimeEdit.MaxDateTime = new DateTime(2018, 12, 31);
dateTimeEdit.ValidationOption = ValidationResetOption.MaxValue;

// If validation fails, value resets to 2018-12-31
```

**VB.NET:**
```vb
dateTimeEdit.MinDateTime = New DateTime(2018, 1, 1)
dateTimeEdit.MaxDateTime = New DateTime(2018, 12, 31)
dateTimeEdit.ValidationOption = ValidationResetOption.MaxValue

' If validation fails, value resets to 2018-12-31
```

## Watermark for Null Values

The Watermark property displays helpful text when the value is null. This feature works only when:
- `AllowNull` is set to `true`
- `DateTimeEditingMode` is set to `Mask`
- `Value` is `null`

### Setting Up Watermark

**C#:**
```csharp
// Enable null values and set watermark
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.AllowNull = true;
dateTimeEdit.Value = null;
dateTimeEdit.Watermark = "Choose a date";

// Customize watermark color
dateTimeEdit.Style.WatermarkForeColor = Color.Gray;
```

**VB.NET:**
```vb
' Enable null values and set watermark
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.AllowNull = True
dateTimeEdit.Value = Nothing
dateTimeEdit.Watermark = "Choose a date"

' Customize watermark color
dateTimeEdit.Style.WatermarkForeColor = Color.Gray
```

### Watermark Examples

**C#:**
```csharp
// Example 1: Optional date field
dateTimeEdit.Watermark = "No date selected";

// Example 2: Appointment scheduler
dateTimeEdit.Watermark = "Select appointment date";

// Example 3: Birthday field
dateTimeEdit.Watermark = "Enter date of birth (optional)";

// Example 4: Deadline field
dateTimeEdit.Watermark = "Set deadline (optional)";
```

## Globalization Support

The SfDateTimeEdit control supports internationalization through the `Culture` property, allowing date-time formatting based on different cultures.

### Setting Culture

**C#:**
```csharp
using System.Globalization;

// Set to US English culture
dateTimeEdit.Culture = new CultureInfo("en-US");

// Set to French culture
dateTimeEdit.Culture = new CultureInfo("fr-FR");

// Set to German culture
dateTimeEdit.Culture = new CultureInfo("de-DE");

// Set to Japanese culture
dateTimeEdit.Culture = new CultureInfo("ja-JP");
```

**VB.NET:**
```vb
Imports System.Globalization

' Set to US English culture
dateTimeEdit.Culture = New CultureInfo("en-US")

' Set to French culture
dateTimeEdit.Culture = New CultureInfo("fr-FR")

' Set to German culture
dateTimeEdit.Culture = New CultureInfo("de-DE")

' Set to Japanese culture
dateTimeEdit.Culture = New CultureInfo("ja-JP")
```

### Culture-Specific Formatting

Different cultures display dates in different formats:

**C#:**
```csharp
DateTime testDate = new DateTime(2018, 7, 5);

// US English (en-US)
dateTimeEdit.Culture = new CultureInfo("en-US");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
// Output: Thursday, July 05, 2018

// French (fr-FR)
dateTimeEdit.Culture = new CultureInfo("fr-FR");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
// Output: jeudi 5 juillet 2018

// German (de-DE)
dateTimeEdit.Culture = new CultureInfo("de-DE");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
// Output: Donnerstag, 5. Juli 2018

// Spanish (es-ES)
dateTimeEdit.Culture = new CultureInfo("es-ES");
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
// Output: jueves, 5 de julio de 2018
```

**VB.NET:**
```vb
Dim testDate As DateTime = New DateTime(2018, 7, 5)

' US English (en-US)
dateTimeEdit.Culture = New CultureInfo("en-US")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
' Output: Thursday, July 05, 2018

' French (fr-FR)
dateTimeEdit.Culture = New CultureInfo("fr-FR")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
' Output: jeudi 5 juillet 2018

' German (de-DE)
dateTimeEdit.Culture = New CultureInfo("de-DE")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
' Output: Donnerstag, 5. Juli 2018

' Spanish (es-ES)
dateTimeEdit.Culture = New CultureInfo("es-ES")
dateTimeEdit.DateTimePattern = DateTimePattern.LongDate
' Output: jueves, 5 de julio de 2018
```

### Calendar Week Rule

Configure how week numbers are calculated based on culture:

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
CultureInfo culture = new CultureInfo("en-US");

// Set calendar week rule
culture.DateTimeFormat.CalendarWeekRule = CalendarWeekRule.FirstFullWeek;
dateTimeEdit.Culture = culture;

// Show week numbers in calendar
dateTimeEdit.MonthCalendar.ShowWeekNumbers = true;
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
Dim culture As New CultureInfo("en-US")

' Set calendar week rule
culture.DateTimeFormat.CalendarWeekRule = CalendarWeekRule.FirstFullWeek
dateTimeEdit.Culture = culture

' Show week numbers in calendar
dateTimeEdit.MonthCalendar.ShowWeekNumbers = True
```

## Right-to-Left Support

The `RightToLeft` property enables right-to-left layout for languages like Arabic and Hebrew.

### Enabling RTL

**C#:**
```csharp
// Enable right-to-left layout
dateTimeEdit.RightToLeft = RightToLeft.Yes;

// Disable right-to-left layout
dateTimeEdit.RightToLeft = RightToLeft.No;

// Use parent's RTL setting
dateTimeEdit.RightToLeft = RightToLeft.Inherit;
```

**VB.NET:**
```vb
' Enable right-to-left layout
dateTimeEdit.RightToLeft = RightToLeft.Yes

' Disable right-to-left layout
dateTimeEdit.RightToLeft = RightToLeft.No

' Use parent's RTL setting
dateTimeEdit.RightToLeft = RightToLeft.Inherit
```

### RTL with Drop-Down Alignment

**C#:**
```csharp
// Configure for RTL layout
dateTimeEdit.RightToLeft = RightToLeft.Yes;

// Align drop-down to the right (natural for RTL)
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Right;

// Set Arabic culture
dateTimeEdit.Culture = new CultureInfo("ar-SA");
```

**VB.NET:**
```vb
' Configure for RTL layout
dateTimeEdit.RightToLeft = RightToLeft.Yes

' Align drop-down to the right (natural for RTL)
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Right

' Set Arabic culture
dateTimeEdit.Culture = New CultureInfo("ar-SA")
```

## ReadOnly Property

The `ReadOnly` property restricts text editing while still allowing value changes through UI elements.

### ReadOnly Behavior

When `ReadOnly` is `true`:
- Users cannot type in the text box
- Up-down buttons still work (if enabled)
- Drop-down calendar still works
- Programmatic value changes still work

**C#:**
```csharp
// Enable read-only mode
dateTimeEdit.ReadOnly = true;

// Users can still use:
// - Up-down buttons (if ShowUpDown = true)
// - Drop-down calendar
// - But cannot type directly
```

**VB.NET:**
```vb
' Enable read-only mode
dateTimeEdit.ReadOnly = True

' Users can still use:
' - Up-down buttons (if ShowUpDown = True)
' - Drop-down calendar
' - But cannot type directly
```

### ReadOnly Example

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
dateTimeEdit.Location = new System.Drawing.Point(50, 50);
dateTimeEdit.Size = new System.Drawing.Size(250, 30);

// Configure as read-only with button access
dateTimeEdit.ReadOnly = true;
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.ShowUpDown = true;
dateTimeEdit.Value = DateTime.Today;

// Users can click up-down or calendar, but not type
this.Controls.Add(dateTimeEdit);
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
dateTimeEdit.Location = New System.Drawing.Point(50, 50)
dateTimeEdit.Size = New System.Drawing.Size(250, 30)

' Configure as read-only with button access
dateTimeEdit.ReadOnly = True
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.ShowUpDown = True
dateTimeEdit.Value = DateTime.Today

' Users can click up-down or calendar, but not type
Me.Controls.Add(dateTimeEdit)
```

## Complete Examples

### Example 1: Comprehensive Validation Form

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class ValidationForm : Form
{
    private SfDateTimeEdit dateEdit;
    private Label lblStatus;
    private ComboBox cmbValidationOption;
    
    public ValidationForm()
    {
        this.Text = "Validation Example";
        this.Size = new Size(500, 300);
        
        // Validation option selector
        Label lblOption = new Label();
        lblOption.Text = "Validation Reset Option:";
        lblOption.Location = new Point(20, 20);
        this.Controls.Add(lblOption);
        
        cmbValidationOption = new ComboBox();
        cmbValidationOption.Location = new Point(170, 20);
        cmbValidationOption.Size = new Size(280, 25);
        cmbValidationOption.DropDownStyle = ComboBoxStyle.DropDownList;
        cmbValidationOption.Items.AddRange(new object[]
        {
            "Reset (to previous value)",
            "MinValue (to minimum date)",
            "MaxValue (to maximum date)"
        });
        cmbValidationOption.SelectedIndex = 0;
        cmbValidationOption.SelectedIndexChanged += ValidationOption_Changed;
        this.Controls.Add(cmbValidationOption);
        
        // Date editor
        Label lblDate = new Label();
        lblDate.Text = "Enter Date:";
        lblDate.Location = new Point(20, 60);
        this.Controls.Add(lblDate);
        
        dateEdit = new SfDateTimeEdit();
        dateEdit.Location = new Point(170, 60);
        dateEdit.Size = new Size(280, 30);
        dateEdit.DateTimeEditingMode = DateTimeEditingMode.Default;
        dateEdit.DateTimePattern = DateTimePattern.ShortDate;
        
        // Set date range: Current year only
        dateEdit.MinDateTime = new DateTime(DateTime.Today.Year, 1, 1);
        dateEdit.MaxDateTime = new DateTime(DateTime.Today.Year, 12, 31);
        dateEdit.Value = DateTime.Today;
        dateEdit.ValidationOption = ValidationResetOption.Reset;
        
        // Subscribe to validation event
        dateEdit.Validating += DateEdit_Validating;
        
        this.Controls.Add(dateEdit);
        
        // Range info
        Label lblRange = new Label();
        lblRange.Text = $"Valid Range: {dateEdit.MinDateTime:d} to {dateEdit.MaxDateTime:d}";
        lblRange.Location = new Point(170, 95);
        lblRange.Size = new Size(280, 20);
        lblRange.ForeColor = Color.Blue;
        this.Controls.Add(lblRange);
        
        // Status label
        lblStatus = new Label();
        lblStatus.Location = new Point(20, 130);
        lblStatus.Size = new Size(450, 120);
        lblStatus.BorderStyle = BorderStyle.FixedSingle;
        lblStatus.BackColor = Color.LightYellow;
        lblStatus.Text = "Status: Ready\n\n" +
                        "Try entering:\n" +
                        "• A date outside the current year (will fail)\n" +
                        "• An invalid date like '13/45/2018' (will fail)\n" +
                        "• A valid date within the current year (will succeed)";
        this.Controls.Add(lblStatus);
    }
    
    private void ValidationOption_Changed(object sender, EventArgs e)
    {
        switch (cmbValidationOption.SelectedIndex)
        {
            case 0:
                dateEdit.ValidationOption = ValidationResetOption.Reset;
                break;
            case 1:
                dateEdit.ValidationOption = ValidationResetOption.MinValue;
                break;
            case 2:
                dateEdit.ValidationOption = ValidationResetOption.MaxValue;
                break;
        }
    }
    
    private void DateEdit_Validating(object sender, ValidatingEventArgs e)
    {
        if (e.IsError)
        {
            lblStatus.Text = $"❌ Validation Failed\n\n" +
                           $"Error: {e.ErrorMessage}\n\n" +
                           $"Action: Value will be reset according to selected option";
            lblStatus.ForeColor = Color.Red;
        }
        else
        {
            lblStatus.Text = $"✓ Validation Succeeded\n\n" +
                           $"Date: {dateEdit.Value:D}\n\n" +
                           $"The entered date is valid and within the allowed range.";
            lblStatus.ForeColor = Color.Green;
        }
    }
}
```

### Example 2: Multi-Language Support

**C#:**
```csharp
using System;
using System.Drawing;
using System.Globalization;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class MultiLanguageForm : Form
{
    private SfDateTimeEdit dateEdit;
    private ComboBox cmbLanguage;
    private Label lblOutput;
    
    public MultiLanguageForm()
    {
        this.Text = "Multi-Language Date Editor";
        this.Size = new Size(500, 250);
        
        // Language selector
        Label lblLanguage = new Label();
        lblLanguage.Text = "Select Language:";
        lblLanguage.Location = new Point(20, 20);
        this.Controls.Add(lblLanguage);
        
        cmbLanguage = new ComboBox();
        cmbLanguage.Location = new Point(130, 20);
        cmbLanguage.Size = new Size(320, 25);
        cmbLanguage.DropDownStyle = ComboBoxStyle.DropDownList;
        cmbLanguage.Items.AddRange(new object[]
        {
            "English (United States) - en-US",
            "French (France) - fr-FR",
            "German (Germany) - de-DE",
            "Spanish (Spain) - es-ES",
            "Japanese (Japan) - ja-JP",
            "Arabic (Saudi Arabia) - ar-SA",
            "Chinese (China) - zh-CN"
        });
        cmbLanguage.SelectedIndex = 0;
        cmbLanguage.SelectedIndexChanged += Language_Changed;
        this.Controls.Add(cmbLanguage);
        
        // Date editor
        Label lblDate = new Label();
        lblDate.Text = "Select Date:";
        lblDate.Location = new Point(20, 70);
        this.Controls.Add(lblDate);
        
        dateEdit = new SfDateTimeEdit();
        dateEdit.Location = new Point(130, 70);
        dateEdit.Size = new Size(320, 30);
        dateEdit.DateTimePattern = DateTimePattern.LongDate;
        dateEdit.Value = DateTime.Today;
        dateEdit.Culture = new CultureInfo("en-US");
        dateEdit.ValueChanged += UpdateOutput;
        this.Controls.Add(dateEdit);
        
        // Output display
        lblOutput = new Label();
        lblOutput.Location = new Point(20, 120);
        lblOutput.Size = new Size(450, 80);
        lblOutput.BorderStyle = BorderStyle.FixedSingle;
        lblOutput.BackColor = Color.AliceBlue;
        this.Controls.Add(lblOutput);
        
        UpdateOutput(null, null);
    }
    
    private void Language_Changed(object sender, EventArgs e)
    {
        string selection = cmbLanguage.SelectedItem.ToString();
        string cultureCode = selection.Substring(selection.LastIndexOf('-') - 2).Trim();
        
        CultureInfo culture = new CultureInfo(cultureCode);
        dateEdit.Culture = culture;
        
        // Enable RTL for Arabic
        dateEdit.RightToLeft = cultureCode == "ar-SA" ? 
            RightToLeft.Yes : RightToLeft.No;
        
        UpdateOutput(null, null);
    }
    
    private void UpdateOutput(object sender, EventArgs e)
    {
        if (dateEdit.Value.HasValue)
        {
            DateTime date = dateEdit.Value.Value;
            string cultureName = dateEdit.Culture.DisplayName;
            
            lblOutput.Text = 
                $"Culture: {cultureName}\n" +
                $"Long Date: {date.ToString("D", dateEdit.Culture)}\n" +
                $"Short Date: {date.ToString("d", dateEdit.Culture)}\n" +
                $"ISO 8601: {date:yyyy-MM-dd}";
        }
    }
}
```

### Example 3: Optional Date Field with Watermark

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class OptionalDateForm : Form
{
    private SfDateTimeEdit birthDateEdit;
    private SfDateTimeEdit anniversaryEdit;
    private Button btnSave;
    
    public OptionalDateForm()
    {
        this.Text = "Optional Date Fields";
        this.Size = new Size(450, 250);
        
        // Birth date (required)
        Label lblBirth = new Label();
        lblBirth.Text = "Birth Date:*";
        lblBirth.Location = new Point(20, 20);
        this.Controls.Add(lblBirth);
        
        birthDateEdit = new SfDateTimeEdit();
        birthDateEdit.Location = new Point(150, 20);
        birthDateEdit.Size = new Size(250, 30);
        birthDateEdit.DateTimePattern = DateTimePattern.LongDate;
        birthDateEdit.MaxDateTime = DateTime.Today;
        birthDateEdit.Value = DateTime.Today.AddYears(-25);
        this.Controls.Add(birthDateEdit);
        
        // Anniversary date (optional)
        Label lblAnniversary = new Label();
        lblAnniversary.Text = "Anniversary Date:";
        lblAnniversary.Location = new Point(20, 70);
        this.Controls.Add(lblAnniversary);
        
        anniversaryEdit = new SfDateTimeEdit();
        anniversaryEdit.Location = new Point(150, 70);
        anniversaryEdit.Size = new Size(250, 30);
        anniversaryEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        anniversaryEdit.DateTimePattern = DateTimePattern.LongDate;
        anniversaryEdit.AllowNull = true;
        anniversaryEdit.Value = null;
        anniversaryEdit.Watermark = "Select anniversary date (optional)";
        anniversaryEdit.Style.WatermarkForeColor = Color.Gray;
        this.Controls.Add(anniversaryEdit);
        
        // Save button
        btnSave = new Button();
        btnSave.Text = "Save";
        btnSave.Location = new Point(150, 120);
        btnSave.Size = new Size(100, 30);
        btnSave.Click += Save_Click;
        this.Controls.Add(btnSave);
        
        // Info label
        Label lblInfo = new Label();
        lblInfo.Text = "* Required field\n" +
                      "Anniversary date is optional and can be left empty.";
        lblInfo.Location = new Point(20, 160);
        lblInfo.Size = new Size(400, 40);
        lblInfo.ForeColor = Color.Blue;
        this.Controls.Add(lblInfo);
    }
    
    private void Save_Click(object sender, EventArgs e)
    {
        if (!birthDateEdit.Value.HasValue)
        {
            MessageBox.Show("Birth date is required!", "Validation Error");
            return;
        }
        
        string message = $"Birth Date: {birthDateEdit.Value:D}\n";
        
        if (anniversaryEdit.Value.HasValue)
        {
            message += $"Anniversary Date: {anniversaryEdit.Value:D}";
        }
        else
        {
            message += "Anniversary Date: Not provided";
        }
        
        MessageBox.Show(message, "Saved Successfully");
    }
}
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and usage
- **[Date Range and Value](date-range-value.md)** - Manage date-time values
- **[Display Patterns](display-patterns.md)** - Format date-time display
- **[Editing Modes](editing-modes.md)** - Configure editing behavior
- **[Appearance Styling](appearance-styling.md)** - Customize visual appearance
