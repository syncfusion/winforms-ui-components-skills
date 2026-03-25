# Date Selection

## Table of Contents
- [Overview](#overview)
- [Single Date Selection](#single-date-selection)
- [Multiple Date Selection](#multiple-date-selection)
- [Date Range Restrictions](#date-range-restrictions)
- [Blackout Dates](#blackout-dates)
- [Special Dates](#special-dates)
- [Programmatic Selection](#programmatic-selection)

## When to Read This

Read this guide when you need to:
- Configure single or multiple date selection
- Enable date range selection for start/end dates
- Restrict selectable dates with MinDate and MaxDate
- Block specific dates from selection (blackout dates)
- Highlight special dates with icons and descriptions
- Programmatically select or clear dates
- Handle selection changed events

## Overview

SfCalendar provides flexible date selection modes including single date, multiple dates, and date range selection, along with powerful date restriction capabilities.

## Single Date Selection

Single date selection is the default mode where users can select one date at a time.

### Basic Single Selection

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Single selection is default - no configuration needed
// Set initial date
calendar.SelectedDate = DateTime.Today;

// Handle selection change
calendar.SelectedDateChanged += (s, e) =>
{
    if (calendar.SelectedDate.HasValue)
    {
        MessageBox.Show($"Selected: {calendar.SelectedDate.Value:D}");
    }
};
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Single selection is default
calendar.SelectedDate = DateTime.Today

' Handle selection change
AddHandler calendar.SelectedDateChanged, Sub(s, e)
    If calendar.SelectedDate.HasValue Then
        MessageBox.Show($"Selected: {calendar.SelectedDate.Value:D}")
    End If
End Sub
```

### Getting Selected Date

**C#:**
```csharp
// Check if date is selected
if (calendar.SelectedDate.HasValue)
{
    DateTime selected = calendar.SelectedDate.Value;
    
    // Use the date
    string dayName = selected.DayOfWeek.ToString();
    string formatted = selected.ToString("yyyy-MM-dd");
}
else
{
    // No date selected
    MessageBox.Show("Please select a date");
}
```

### Clearing Selection

**C#:**
```csharp
// Clear selection programmatically
calendar.SelectedDate = null;

// Or enable None button for user to clear
calendar.ShowNone = true;
```

## Multiple Date Selection

Allow users to select multiple dates simultaneously.

### Enable Multiple Selection

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Enable multiple selection
calendar.AllowMultipleSelection = true;

// Handle selection changes
calendar.SelectionChanged += (s, e) =>
{
    int count = calendar.SelectedDates.Count;
    MessageBox.Show($"Selected {count} dates");
};
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Enable multiple selection
calendar.AllowMultipleSelection = True

' Handle selection changes
AddHandler calendar.SelectionChanged, Sub(s, e)
    Dim count As Integer = calendar.SelectedDates.Count
    MessageBox.Show($"Selected {count} dates")
End Sub
```

### Working with Multiple Selected Dates

**C#:**
```csharp
// Get all selected dates
var selectedDates = calendar.SelectedDates;

// Iterate through selected dates
foreach (DateTime date in selectedDates)
{
    Console.WriteLine(date.ToString("D"));
}

// Get count
int dateCount = selectedDates.Count;

// Check if specific date is selected
DateTime checkDate = new DateTime(2026, 3, 25);
bool isSelected = selectedDates.Contains(checkDate);
```

**VB.NET:**
```vb
' Get all selected dates
Dim selectedDates = calendar.SelectedDates

' Iterate through selected dates
For Each [date] As DateTime In selectedDates
    Console.WriteLine([date].ToString("D"))
Next

' Get count
Dim dateCount As Integer = selectedDates.Count
```

### Adding/Removing Dates Programmatically

**C#:**
```csharp
// Add dates to selection
calendar.SelectedDates.Add(new DateTime(2026, 3, 25));
calendar.SelectedDates.Add(new DateTime(2026, 3, 28));

// Remove date from selection
calendar.SelectedDates.Remove(new DateTime(2026, 3, 25));

// Clear all selections
calendar.SelectedDates.Clear();
```

### Multiple Selection Example

**C#:**
```csharp
public class MultiSelectCalendarForm : Form
{
    private SfCalendar calendar;
    private ListBox lstSelected;
    private Button btnClearAll;
    
    public MultiSelectCalendarForm()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 20),
            Size = new Size(350, 320),
            AllowMultipleSelection = true
        };
        calendar.SelectionChanged += Calendar_SelectionChanged;
        
        lstSelected = new ListBox
        {
            Location = new Point(380, 20),
            Size = new Size(200, 320)
        };
        
        btnClearAll = new Button
        {
            Location = new Point(380, 350),
            Size = new Size(200, 30),
            Text = "Clear All Selections"
        };
        btnClearAll.Click += (s, e) =>
        {
            calendar.SelectedDates.Clear();
            lstSelected.Items.Clear();
        };
        
        this.Controls.Add(calendar);
        this.Controls.Add(lstSelected);
        this.Controls.Add(btnClearAll);
        this.Size = new Size(620, 430);
        this.Text = "Multiple Date Selection";
    }
    
    private void Calendar_SelectionChanged(object sender, EventArgs e)
    {
        lstSelected.Items.Clear();
        foreach (DateTime date in calendar.SelectedDates)
        {
            lstSelected.Items.Add(date.ToString("D"));
        }
    }
}
```

## Date Range Restrictions

Limit selectable dates to a specific range using MinDate and MaxDate.

### Set Minimum and Maximum Dates

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Restrict to next 30 days only
calendar.MinDate = DateTime.Today;
calendar.MaxDate = DateTime.Today.AddDays(30);

// Or restrict to past dates
calendar.MinDate = DateTime.Today.AddMonths(-6);
calendar.MaxDate = DateTime.Today;
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Restrict to next 30 days only
calendar.MinDate = DateTime.Today
calendar.MaxDate = DateTime.Today.AddDays(30)
```

### Common Date Range Patterns

**C#:**
```csharp
// Future dates only (no past dates)
calendar.MinDate = DateTime.Today;
calendar.MaxDate = DateTime.MaxValue;

// Past dates only (no future dates)
calendar.MinDate = DateTime.MinValue;
calendar.MaxDate = DateTime.Today;

// Current month only
calendar.MinDate = new DateTime(DateTime.Today.Year, DateTime.Today.Month, 1);
calendar.MaxDate = calendar.MinDate.AddMonths(1).AddDays(-1);

// Current year only
calendar.MinDate = new DateTime(DateTime.Today.Year, 1, 1);
calendar.MaxDate = new DateTime(DateTime.Today.Year, 12, 31);

// Next 6 months
calendar.MinDate = DateTime.Today;
calendar.MaxDate = DateTime.Today.AddMonths(6);
```

### Date Range Example

**C#:**
```csharp
public class DateRangeForm : Form
{
    private SfCalendar calendar;
    private DateTimePicker dtpMin;
    private DateTimePicker dtpMax;
    private Button btnApply;
    
    public DateRangeForm()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 100),
            Size = new Size(350, 300),
            MinDate = DateTime.Today,
            MaxDate = DateTime.Today.AddMonths(3)
        };
        
        Label lblMin = new Label
        {
            Location = new Point(20, 20),
            Size = new Size(100, 25),
            Text = "Min Date:"
        };
        
        dtpMin = new DateTimePicker
        {
            Location = new Point(120, 20),
            Size = new Size(200, 25),
            Value = DateTime.Today
        };
        
        Label lblMax = new Label
        {
            Location = new Point(20, 55),
            Size = new Size(100, 25),
            Text = "Max Date:"
        };
        
        dtpMax = new DateTimePicker
        {
            Location = new Point(120, 55),
            Size = new Size(200, 25),
            Value = DateTime.Today.AddMonths(3)
        };
        
        btnApply = new Button
        {
            Location = new Point(330, 37),
            Size = new Size(80, 30),
            Text = "Apply"
        };
        btnApply.Click += BtnApply_Click;
        
        this.Controls.AddRange(new Control[] { 
            calendar, lblMin, dtpMin, lblMax, dtpMax, btnApply 
        });
        this.Size = new Size(450, 470);
        this.Text = "Date Range Restriction";
    }
    
    private void BtnApply_Click(object sender, EventArgs e)
    {
        if (dtpMin.Value > dtpMax.Value)
        {
            MessageBox.Show("Min date must be before max date!", "Invalid Range");
            return;
        }
        
        calendar.MinDate = dtpMin.Value;
        calendar.MaxDate = dtpMax.Value;
        calendar.SelectedDate = null;  // Clear selection
    }
}
```

## Blackout Dates

Block specific dates from selection. Blackout dates appear with distinct styling and cannot be selected.

### Add Blackout Dates

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Add single blackout date
calendar.BlackoutDates.Add(new DateTime(2026, 3, 25));

// Add multiple blackout dates
calendar.BlackoutDates.Add(new DateTime(2026, 3, 28));
calendar.BlackoutDates.Add(new DateTime(2026, 4, 1));

// Add date range as blackout (e.g., company closure)
DateTime start = new DateTime(2026, 12, 24);
DateTime end = new DateTime(2026, 12, 31);
for (DateTime date = start; date <= end; date = date.AddDays(1))
{
    calendar.BlackoutDates.Add(date);
}
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Add blackout dates
calendar.BlackoutDates.Add(New DateTime(2026, 3, 25))
calendar.BlackoutDates.Add(New DateTime(2026, 3, 28))

' Add date range
Dim startDate = New DateTime(2026, 12, 24)
Dim endDate = New DateTime(2026, 12, 31)
Dim currentDate = startDate
While currentDate <= endDate
    calendar.BlackoutDates.Add(currentDate)
    currentDate = currentDate.AddDays(1)
End While
```

### Block Weekends

**C#:**
```csharp
// Block all weekends in current month
DateTime firstDay = new DateTime(DateTime.Today.Year, DateTime.Today.Month, 1);
DateTime lastDay = firstDay.AddMonths(1).AddDays(-1);

for (DateTime date = firstDay; date <= lastDay; date = date.AddDays(1))
{
    if (date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday)
    {
        calendar.BlackoutDates.Add(date);
    }
}
```

### Remove Blackout Dates

**C#:**
```csharp
// Remove specific blackout date
calendar.BlackoutDates.Remove(new DateTime(2026, 3, 25));

// Clear all blackout dates
calendar.BlackoutDates.Clear();

// Check if date is blacked out
DateTime checkDate = new DateTime(2026, 3, 25);
bool isBlocked = calendar.BlackoutDates.Contains(checkDate);
```

## Special Dates

Highlight important dates with custom icons and descriptions.

### Add Special Dates

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using Syncfusion.WinForms.Input.Events;

SfCalendar calendar = new SfCalendar();

// Add special date with icon
calendar.SpecialDates.Add(new SpecialDate
{
    Date = new DateTime(2026, 3, 25),
    Description = "Team Meeting at 2 PM",
    IconImage = Properties.Resources.MeetingIcon  // 16x16 icon recommended
});

// Add holiday
calendar.SpecialDates.Add(new SpecialDate
{
    Date = new DateTime(2026, 12, 25),
    Description = "Christmas Holiday",
    IconImage = Properties.Resources.HolidayIcon
});

// Add birthday
calendar.SpecialDates.Add(new SpecialDate
{
    Date = new DateTime(2026, 4, 15),
    Description = "John's Birthday",
    IconImage = Properties.Resources.BirthdayIcon
});
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Add special date
calendar.SpecialDates.Add(New SpecialDate With {
    .[Date] = New DateTime(2026, 3, 25),
    .Description = "Team Meeting at 2 PM",
    .IconImage = My.Resources.MeetingIcon
})
```

### Load Icons from Resources

**C#:**
```csharp
// Assuming you have icons in project resources
Image meetingIcon = Image.FromFile("Icons/meeting.png");
Image deadlineIcon = Image.FromFile("Icons/deadline.png");

calendar.SpecialDates.Add(new SpecialDate
{
    Date = DateTime.Today.AddDays(5),
    Description = "Project Deadline",
    IconImage = deadlineIcon
});
```

### Special Dates Example

**C#:**
```csharp
public class SpecialDatesForm : Form
{
    private SfCalendar calendar;
    private TextBox txtDescription;
    private Button btnAddSpecial;
    
    public SpecialDatesForm()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 50),
            Size = new Size(350, 320)
        };
        
        // Show description when hovering over special dates
        calendar.CellToolTipOpening += Calendar_CellToolTipOpening;
        
        Label lblDesc = new Label
        {
            Location = new Point(20, 380),
            Size = new Size(100, 25),
            Text = "Description:"
        };
        
        txtDescription = new TextBox
        {
            Location = new Point(120, 380),
            Size = new Size(250, 25),
            PlaceholderText = "Enter event description"
        };
        
        btnAddSpecial = new Button
        {
            Location = new Point(380, 380),
            Size = new Size(100, 25),
            Text = "Add Special"
        };
        btnAddSpecial.Click += BtnAddSpecial_Click;
        
        this.Controls.AddRange(new Control[] {
            calendar, lblDesc, txtDescription, btnAddSpecial
        });
        this.Size = new Size(520, 480);
        this.Text = "Special Dates Example";
    }
    
    private void BtnAddSpecial_Click(object sender, EventArgs e)
    {
        if (calendar.SelectedDate.HasValue && !string.IsNullOrWhiteSpace(txtDescription.Text))
        {
            calendar.SpecialDates.Add(new SpecialDate
            {
                Date = calendar.SelectedDate.Value,
                Description = txtDescription.Text,
                IconImage = SystemIcons.Information.ToBitmap()  // Using system icon
            });
            
            txtDescription.Clear();
            MessageBox.Show("Special date added!", "Success");
        }
        else
        {
            MessageBox.Show("Please select a date and enter description.", "Error");
        }
    }
    
    private void Calendar_CellToolTipOpening(object sender, CellToolTipOpeningEventArgs e)
    {
        // Show special date description in tooltip
        var specialDate = calendar.SpecialDates.FirstOrDefault(
            sd => sd.Date.Date == e.Date.Date);
        
        if (specialDate != null)
        {
            e.ToolTipInfo.Text = specialDate.Description;
        }
    }
}
```

## Programmatic Selection

### Select Date Programmatically

**C#:**
```csharp
// Select specific date
calendar.SelectedDate = new DateTime(2026, 3, 25);

// Select today
calendar.SelectedDate = DateTime.Today;

// Select first day of month
calendar.SelectedDate = new DateTime(DateTime.Today.Year, DateTime.Today.Month, 1);

// Select last day of month
DateTime firstDay = new DateTime(DateTime.Today.Year, DateTime.Today.Month, 1);
calendar.SelectedDate = firstDay.AddMonths(1).AddDays(-1);
```

### Navigate to Specific Date

**C#:**
```csharp
// Navigate to date without selecting
calendar.DisplayDate = new DateTime(2026, 12, 1);
```

### Validate Date Before Selection

**C#:**
```csharp
DateTime dateToSelect = new DateTime(2026, 3, 25);

// Check if date is within valid range
if (dateToSelect >= calendar.MinDate && dateToSelect <= calendar.MaxDate)
{
    // Check if date is not blacked out
    if (!calendar.BlackoutDates.Contains(dateToSelect))
    {
        calendar.SelectedDate = dateToSelect;
    }
    else
    {
        MessageBox.Show("This date is not available.", "Blackout Date");
    }
}
else
{
    MessageBox.Show("Date is outside allowed range.", "Invalid Date");
}
```

## Next Steps

- **[Views and Navigation](views-and-navigation.md)** - Learn about Month, Year, Decade, and Century views
- **[Appearance Customization](appearance-customization.md)** - Customize colors, fonts, and visual styles
- **[Globalization](globalization.md)** - Localize calendar for different cultures and languages
