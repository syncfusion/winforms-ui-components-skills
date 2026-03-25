# Date Range and Value Management

Learn how to manage date-time values, set minimum and maximum date ranges, and handle value changes in the SfDateTimeEdit control.

## Table of Contents

- [Value Property](#value-property)
- [MinDateTime and MaxDateTime](#mindatetime-and-maxdatetime)
- [Date Range Validation](#date-range-validation)
- [AllowNull Property](#allownull-property)
- [DateTimeText Property](#datetimetext-property)
- [ValueChanged Event](#valuechanged-event)
- [Complete Examples](#complete-examples)
- [Next Steps](#next-steps)

## Value Property

The `Value` property is the primary property for setting and retrieving the current date and time in the SfDateTimeEdit control. It's a nullable `DateTime?` type, allowing null values when configured appropriately.

### Setting the Value

**C#:**
```csharp
// Set a specific date and time
dateTimeEdit.Value = new DateTime(2018, 2, 16, 10, 30, 0);

// Set to current date and time
dateTimeEdit.Value = DateTime.Now;

// Set to today's date (time at midnight)
dateTimeEdit.Value = DateTime.Today;

// Set to null (when AllowNull is true)
dateTimeEdit.AllowNull = true;
dateTimeEdit.Value = null;
```

**VB.NET:**
```vb
' Set a specific date and time
dateTimeEdit.Value = New DateTime(2018, 2, 16, 10, 30, 0)

' Set to current date and time
dateTimeEdit.Value = DateTime.Now

' Set to today's date (time at midnight)
dateTimeEdit.Value = DateTime.Today

' Set to null (when AllowNull is true)
dateTimeEdit.AllowNull = True
dateTimeEdit.Value = Nothing
```

## MinDateTime and MaxDateTime

The `MinDateTime` and `MaxDateTime` properties restrict the date-time values that users can select, creating a valid date range.

### MinDateTime Property

Restricts users from selecting dates earlier than the specified minimum date:

**C#:**
```csharp
// Set minimum date to February 1, 2018
dateTimeEdit.MinDateTime = new DateTime(2018, 2, 1);

// Set minimum date to 30 days ago
dateTimeEdit.MinDateTime = DateTime.Now.AddDays(-30);
```

**VB.NET:**
```vb
' Set minimum date to February 1, 2018
dateTimeEdit.MinDateTime = New DateTime(2018, 2, 1)

' Set minimum date to 30 days ago
dateTimeEdit.MinDateTime = DateTime.Now.AddDays(-30)
```

### MaxDateTime Property

Restricts users from selecting dates later than the specified maximum date:

**C#:**
```csharp
// Set maximum date to February 28, 2018
dateTimeEdit.MaxDateTime = new DateTime(2018, 2, 28);

// Set maximum date to 30 days from now
dateTimeEdit.MaxDateTime = DateTime.Now.AddDays(30);
```

**VB.NET:**
```vb
' Set maximum date to February 28, 2018
dateTimeEdit.MaxDateTime = New DateTime(2018, 2, 28)

' Set maximum date to 30 days from now
dateTimeEdit.MaxDateTime = DateTime.Now.AddDays(30)
```

## Date Range Validation

When `Value` is set outside the MinDateTime and MaxDateTime range, it automatically resets:

- If `Value < MinDateTime`, then `Value` is reset to `MinDateTime`
- If `Value > MaxDateTime`, then `Value` is reset to `MaxDateTime`
- When setting `MinDateTime`, if new `MinDateTime > MaxDateTime`, then `MaxDateTime` is reset to `MinDateTime`
- When setting `MaxDateTime`, if new `MaxDateTime < MinDateTime`, then `MinDateTime` is reset to `MaxDateTime`

### Example: Date Range for Appointment Booking

**C#:**
```csharp
// Appointment booking system: Allow bookings for next 30 days
SfDateTimeEdit appointmentDate = new SfDateTimeEdit();

// Set current date as the starting value
appointmentDate.Value = DateTime.Today;

// Set minimum to today (no past bookings)
appointmentDate.MinDateTime = DateTime.Today;

// Set maximum to 30 days from now
appointmentDate.MaxDateTime = DateTime.Today.AddDays(30);

this.Controls.Add(appointmentDate);
```

**VB.NET:**
```vb
' Appointment booking system: Allow bookings for next 30 days
Dim appointmentDate As New SfDateTimeEdit()

' Set current date as the starting value
appointmentDate.Value = DateTime.Today

' Set minimum to today (no past bookings)
appointmentDate.MinDateTime = DateTime.Today

' Set maximum to 30 days from now
appointmentDate.MaxDateTime = DateTime.Today.AddDays(30)

Me.Controls.Add(appointmentDate)
```

### Example: Hotel Reservation System

**C#:**
```csharp
// Check-in date control
SfDateTimeEdit checkInDate = new SfDateTimeEdit();
checkInDate.Value = DateTime.Today;
checkInDate.MinDateTime = DateTime.Today;
checkInDate.MaxDateTime = DateTime.Today.AddYears(1);

// Check-out date control (minimum is check-in date)
SfDateTimeEdit checkOutDate = new SfDateTimeEdit();
checkOutDate.Value = DateTime.Today.AddDays(1);
checkOutDate.MinDateTime = DateTime.Today.AddDays(1);
checkOutDate.MaxDateTime = DateTime.Today.AddYears(1);

// Update check-out minimum when check-in changes
checkInDate.ValueChanged += (s, e) =>
{
    if (checkInDate.Value.HasValue)
    {
        checkOutDate.MinDateTime = checkInDate.Value.Value.AddDays(1);
        if (!checkOutDate.Value.HasValue || 
            checkOutDate.Value.Value <= checkInDate.Value.Value)
        {
            checkOutDate.Value = checkInDate.Value.Value.AddDays(1);
        }
    }
};
```

**VB.NET:**
```vb
' Check-in date control
Dim checkInDate As New SfDateTimeEdit()
checkInDate.Value = DateTime.Today
checkInDate.MinDateTime = DateTime.Today
checkInDate.MaxDateTime = DateTime.Today.AddYears(1)

' Check-out date control (minimum is check-in date)
Dim checkOutDate As New SfDateTimeEdit()
checkOutDate.Value = DateTime.Today.AddDays(1)
checkOutDate.MinDateTime = DateTime.Today.AddDays(1)
checkOutDate.MaxDateTime = DateTime.Today.AddYears(1)

' Update check-out minimum when check-in changes
AddHandler checkInDate.ValueChanged, Sub(s, e)
    If checkInDate.Value.HasValue Then
        checkOutDate.MinDateTime = checkInDate.Value.Value.AddDays(1)
        If Not checkOutDate.Value.HasValue OrElse _
           checkOutDate.Value.Value <= checkInDate.Value.Value Then
            checkOutDate.Value = checkInDate.Value.Value.AddDays(1)
        End If
    End If
End Sub
```

## AllowNull Property

The `AllowNull` property enables the control to accept null values. This works only in **Mask editing mode**.

**C#:**
```csharp
// Enable null values
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
dateTimeEdit.AllowNull = true;
dateTimeEdit.Value = null;
dateTimeEdit.Watermark = "Choose a date";
```

**VB.NET:**
```vb
' Enable null values
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask
dateTimeEdit.AllowNull = True
dateTimeEdit.Value = Nothing
dateTimeEdit.Watermark = "Choose a date"
```

**Important Notes:**
- AllowNull only works when `DateTimeEditingMode` is set to `Mask`
- When Value is null, the Watermark text is displayed
- In Default editing mode, AllowNull is not supported

## DateTimeText Property

The `DateTimeText` property allows you to set the date-time value using a text string. The text must match the current DateTimePattern format.

**C#:**
```csharp
// Set pattern first
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;

// Set value using DateTimeText (must match pattern format)
dateTimeEdit.DateTimeText = "02/16/2018";

// For custom pattern
dateTimeEdit.DateTimePattern = DateTimePattern.Custom;
dateTimeEdit.Format = "MM/dd/yyyy hh:mm tt";
dateTimeEdit.DateTimeText = "02/16/2018 10:30 AM";
```

**VB.NET:**
```vb
' Set pattern first
dateTimeEdit.DateTimePattern = DateTimePattern.ShortDate

' Set value using DateTimeText (must match pattern format)
dateTimeEdit.DateTimeText = "02/16/2018"

' For custom pattern
dateTimeEdit.DateTimePattern = DateTimePattern.Custom
dateTimeEdit.Format = "MM/dd/yyyy hh:mm tt"
dateTimeEdit.DateTimeText = "02/16/2018 10:30 AM"
```

## ValueChanged Event

The `ValueChanged` event fires whenever the Value property changes, allowing you to respond to user selections.

**C#:**
```csharp
// Subscribe to the event
dateTimeEdit.ValueChanged += DateTimeEdit_ValueChanged;

private void DateTimeEdit_ValueChanged(object sender, EventArgs e)
{
    SfDateTimeEdit editor = sender as SfDateTimeEdit;
    
    if (editor.Value.HasValue)
    {
        MessageBox.Show($"Date changed to: {editor.Value.Value:D}");
    }
    else
    {
        MessageBox.Show("Date cleared (null value)");
    }
}
```

**VB.NET:**
```vb
' Subscribe to the event
AddHandler dateTimeEdit.ValueChanged, AddressOf DateTimeEdit_ValueChanged

Private Sub DateTimeEdit_ValueChanged(sender As Object, e As EventArgs)
    Dim editor As SfDateTimeEdit = TryCast(sender, SfDateTimeEdit)
    
    If editor.Value.HasValue Then
        MessageBox.Show($"Date changed to: {editor.Value.Value:D}")
    Else
        MessageBox.Show("Date cleared (null value)")
    End If
End Sub
```

## Complete Examples

### Example 1: Project Deadline Selector

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class ProjectDeadlineForm : Form
{
    private SfDateTimeEdit startDate;
    private SfDateTimeEdit endDate;
    private Label lblStatus;
    
    public ProjectDeadlineForm()
    {
        this.Text = "Project Timeline";
        this.Size = new Size(400, 200);
        
        // Start date
        Label lblStart = new Label();
        lblStart.Text = "Start Date:";
        lblStart.Location = new Point(20, 20);
        this.Controls.Add(lblStart);
        
        startDate = new SfDateTimeEdit();
        startDate.Location = new Point(120, 20);
        startDate.Size = new Size(200, 25);
        startDate.Value = DateTime.Today;
        startDate.MinDateTime = DateTime.Today;
        startDate.MaxDateTime = DateTime.Today.AddYears(2);
        startDate.ValueChanged += UpdateEndDateConstraints;
        this.Controls.Add(startDate);
        
        // End date
        Label lblEnd = new Label();
        lblEnd.Text = "End Date:";
        lblEnd.Location = new Point(20, 60);
        this.Controls.Add(lblEnd);
        
        endDate = new SfDateTimeEdit();
        endDate.Location = new Point(120, 60);
        endDate.Size = new Size(200, 25);
        endDate.Value = DateTime.Today.AddMonths(1);
        endDate.MinDateTime = DateTime.Today;
        endDate.MaxDateTime = DateTime.Today.AddYears(2);
        endDate.ValueChanged += UpdateStatus;
        this.Controls.Add(endDate);
        
        // Status label
        lblStatus = new Label();
        lblStatus.Location = new Point(20, 100);
        lblStatus.Size = new Size(350, 40);
        this.Controls.Add(lblStatus);
        
        UpdateStatus(null, null);
    }
    
    private void UpdateEndDateConstraints(object sender, EventArgs e)
    {
        if (startDate.Value.HasValue)
        {
            endDate.MinDateTime = startDate.Value.Value;
            if (!endDate.Value.HasValue || endDate.Value.Value < startDate.Value.Value)
            {
                endDate.Value = startDate.Value.Value.AddDays(1);
            }
        }
        UpdateStatus(null, null);
    }
    
    private void UpdateStatus(object sender, EventArgs e)
    {
        if (startDate.Value.HasValue && endDate.Value.HasValue)
        {
            TimeSpan duration = endDate.Value.Value - startDate.Value.Value;
            lblStatus.Text = $"Project Duration: {duration.Days} days";
        }
    }
}
```

### Example 2: Age Verification Control

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class AgeVerificationForm : Form
{
    private SfDateTimeEdit birthDate;
    private Label lblAge;
    
    public AgeVerificationForm()
    {
        this.Text = "Age Verification";
        this.Size = new Size(400, 150);
        
        Label lblPrompt = new Label();
        lblPrompt.Text = "Enter Birth Date:";
        lblPrompt.Location = new Point(20, 20);
        this.Controls.Add(lblPrompt);
        
        birthDate = new SfDateTimeEdit();
        birthDate.Location = new Point(120, 20);
        birthDate.Size = new Size(200, 25);
        birthDate.DateTimePattern = DateTimePattern.LongDate;
        
        // Must be at least 18 years old
        DateTime eighteenYearsAgo = DateTime.Today.AddYears(-18);
        birthDate.MaxDateTime = eighteenYearsAgo;
        
        // Reasonable minimum (no one over 120 years old)
        birthDate.MinDateTime = DateTime.Today.AddYears(-120);
        
        birthDate.Value = eighteenYearsAgo;
        birthDate.ValueChanged += CalculateAge;
        this.Controls.Add(birthDate);
        
        lblAge = new Label();
        lblAge.Location = new Point(20, 60);
        lblAge.Size = new Size(350, 20);
        this.Controls.Add(lblAge);
        
        CalculateAge(null, null);
    }
    
    private void CalculateAge(object sender, EventArgs e)
    {
        if (birthDate.Value.HasValue)
        {
            DateTime birth = birthDate.Value.Value;
            int age = DateTime.Today.Year - birth.Year;
            if (birth.Date > DateTime.Today.AddYears(-age))
                age--;
                
            lblAge.Text = $"Age: {age} years old";
            lblAge.ForeColor = age >= 18 ? Color.Green : Color.Red;
        }
    }
}
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and usage
- **[Display Patterns](display-patterns.md)** - Format date-time display
- **[Editing Modes](editing-modes.md)** - Configure editing behavior
- **[Appearance Styling](appearance-styling.md)** - Customize visual appearance
- **[Validation Features](validation-features.md)** - Implement validation and error handling
