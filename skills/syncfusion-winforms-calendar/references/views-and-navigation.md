# Views and Navigation

## Table of Contents
- [Overview](#overview)
- [Month View](#month-view)
- [Year View](#year-view)
- [Decade View](#decade-view)
- [Century View](#century-view)
- [Navigation Between Views](#navigation-between-views)
- [Today and None Buttons](#today-and-none-buttons)
- [Header Customization](#header-customization)

## When to Read This

Read this guide when you need to:
- Understand different calendar views (Month, Year, Decade, Century)
- Navigate between calendar views programmatically
- Customize view navigation behavior
- Configure Today and None buttons
- Customize header appearance and text
- Control which views are accessible to users

## Overview

SfCalendar provides four hierarchical views that allow users to quickly navigate through time periods:
- **Month View** - Shows days of a specific month (default view)
- **Year View** - Shows 12 months for selecting a month
- **Decade View** - Shows 10 years for selecting a year
- **Century View** - Shows 10 decades for selecting a decade

Users can click on the header to navigate upward through views, then select an item to navigate back down.

## Month View

The default view that displays days of a month in a calendar grid.

### Basic Month View

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Month view is default
calendar.View = CalendarView.Month;

// Set the displayed month
calendar.DisplayDate = new DateTime(2026, 3, 1);
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Month view is default
calendar.View = CalendarView.Month

' Set the displayed month
calendar.DisplayDate = New DateTime(2026, 3, 1)
```

### Month View Features

**C#:**
```csharp
// Show week numbers
calendar.ShowWeekNumbers = true;

// Set first day of week
calendar.FirstDayOfWeek = DayOfWeek.Monday;

// Navigate to specific month
calendar.DisplayDate = new DateTime(2026, 6, 1);  // June 2026
```

### Month Navigation

**C#:**
```csharp
// Go to next month
DateTime nextMonth = calendar.DisplayDate.AddMonths(1);
calendar.DisplayDate = nextMonth;

// Go to previous month
DateTime prevMonth = calendar.DisplayDate.AddMonths(-1);
calendar.DisplayDate = prevMonth;

// Go to current month
calendar.DisplayDate = DateTime.Today;
```

## Year View

Displays 12 months of a year for quick month selection.

### Enable Year View

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Set to year view
calendar.View = CalendarView.Year;

// Or allow user to click header in month view to navigate to year view
// (This is enabled by default)
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Set to year view
calendar.View = CalendarView.Year
```

### Year View Behavior

When in Year View:
- Displays 12 months in a 4x3 grid (Jan, Feb, Mar...)
- Clicking a month navigates to Month View for that month
- Header shows the year (e.g., "2026")
- Clicking header navigates to Decade View

**C#:**
```csharp
// Navigate to specific year
calendar.DisplayDate = new DateTime(2025, 1, 1);
calendar.View = CalendarView.Year;
```

### Year Navigation

**C#:**
```csharp
// Go to next year
DateTime nextYear = calendar.DisplayDate.AddYears(1);
calendar.DisplayDate = nextYear;

// Go to previous year
DateTime prevYear = calendar.DisplayDate.AddYears(-1);
calendar.DisplayDate = prevYear;
```

## Decade View

Displays 10 years of a decade for quick year selection.

### Enable Decade View

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Set to decade view
calendar.View = CalendarView.Decade;

// Display decade starting from 2020
calendar.DisplayDate = new DateTime(2020, 1, 1);
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Set to decade view
calendar.View = CalendarView.Decade
calendar.DisplayDate = New DateTime(2020, 1, 1)
```

### Decade View Behavior

When in Decade View:
- Displays 10 years in a grid (e.g., 2020-2029)
- Clicking a year navigates to Year View for that year
- Header shows the decade range (e.g., "2020 - 2029")
- Clicking header navigates to Century View

**C#:**
```csharp
// Navigate to specific decade
calendar.DisplayDate = new DateTime(2030, 1, 1);  // 2030-2039
calendar.View = CalendarView.Decade;
```

## Century View

Displays 10 decades of a century for broad time navigation.

### Enable Century View

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Set to century view
calendar.View = CalendarView.Century;

// Display century starting from 2000
calendar.DisplayDate = new DateTime(2000, 1, 1);
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Set to century view
calendar.View = CalendarView.Century
calendar.DisplayDate = New DateTime(2000, 1, 1)
```

### Century View Behavior

When in Century View:
- Displays 10 decades in a grid (e.g., 2000-2009, 2010-2019, etc.)
- Clicking a decade navigates to Decade View for that decade
- Header shows the century range (e.g., "2000 - 2099")
- This is the highest level view (clicking header has no effect)

## Navigation Between Views

### User Navigation (Default Behavior)

By default, users can navigate between views by:
1. Clicking the header to navigate **upward** (Month → Year → Decade → Century)
2. Clicking a cell to navigate **downward** (Century → Decade → Year → Month)

### Programmatic View Changes

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Set view programmatically
calendar.View = CalendarView.Month;    // Month view
calendar.View = CalendarView.Year;     // Year view
calendar.View = CalendarView.Decade;   // Decade view
calendar.View = CalendarView.Century;  // Century view
```

### Handle View Changed Event

**C#:**
```csharp
calendar.ViewChanged += (sender, e) =>
{
    string currentView = calendar.View.ToString();
    MessageBox.Show($"View changed to: {currentView}");
};
```

**VB.NET:**
```vb
AddHandler calendar.ViewChanged, Sub(sender, e)
    Dim currentView As String = calendar.View.ToString()
    MessageBox.Show($"View changed to: {currentView}")
End Sub
```

### Restrict View Navigation

**C#:**
```csharp
// Allow only Month and Year views
// (Note: This requires handling ViewChanging event to prevent navigation)
calendar.ViewChanging += (sender, e) =>
{
    if (e.NewValue == CalendarView.Decade || e.NewValue == CalendarView.Century)
    {
        e.Cancel = true;  // Prevent navigation to Decade/Century
    }
};
```

### Complete Navigation Example

**C#:**
```csharp
public class ViewNavigationForm : Form
{
    private SfCalendar calendar;
    private Label lblCurrentView;
    private Button btnMonth;
    private Button btnYear;
    private Button btnDecade;
    private Button btnCentury;
    
    public ViewNavigationForm()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 60),
            Size = new Size(350, 320)
        };
        calendar.ViewChanged += Calendar_ViewChanged;
        
        lblCurrentView = new Label
        {
            Location = new Point(20, 20),
            Size = new Size(350, 30),
            Text = "Current View: Month",
            Font = new Font("Segoe UI", 10F, FontStyle.Bold)
        };
        
        btnMonth = new Button
        {
            Location = new Point(20, 390),
            Size = new Size(80, 30),
            Text = "Month"
        };
        btnMonth.Click += (s, e) => calendar.View = CalendarView.Month;
        
        btnYear = new Button
        {
            Location = new Point(110, 390),
            Size = new Size(80, 30),
            Text = "Year"
        };
        btnYear.Click += (s, e) => calendar.View = CalendarView.Year;
        
        btnDecade = new Button
        {
            Location = new Point(200, 390),
            Size = new Size(80, 30),
            Text = "Decade"
        };
        btnDecade.Click += (s, e) => calendar.View = CalendarView.Decade;
        
        btnCentury = new Button
        {
            Location = new Point(290, 390),
            Size = new Size(80, 30),
            Text = "Century"
        };
        btnCentury.Click += (s, e) => calendar.View = CalendarView.Century;
        
        this.Controls.AddRange(new Control[] {
            calendar, lblCurrentView, btnMonth, btnYear, btnDecade, btnCentury
        });
        this.Size = new Size(410, 480);
        this.Text = "View Navigation Demo";
    }
    
    private void Calendar_ViewChanged(object sender, EventArgs e)
    {
        lblCurrentView.Text = $"Current View: {calendar.View}";
    }
}
```

## Today and None Buttons

### Show Today Button

The Today button navigates to and selects today's date.

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Enable Today button (shown at bottom of calendar)
calendar.ShowToday = true;

// Customize Today button text (if supported by theme)
// The button appears at the bottom and navigates to today's date
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Enable Today button
calendar.ShowToday = True
```

### Show None Button

The None button clears the current selection.

**C#:**
```csharp
SfCalendar calendar = new SfCalendar();

// Enable None button (shown at bottom of calendar)
calendar.ShowNone = true;

// None button clears selection
// After clicking None, SelectedDate will be null
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Enable None button
calendar.ShowNone = True
```

### Both Buttons Example

**C#:**
```csharp
SfCalendar calendar = new SfCalendar
{
    Location = new Point(20, 20),
    Size = new Size(350, 350),  // Slightly larger to accommodate buttons
    ShowToday = true,
    ShowNone = true,
    SelectedDate = DateTime.Today
};

// Handle selection changes
calendar.SelectedDateChanged += (s, e) =>
{
    if (calendar.SelectedDate.HasValue)
    {
        Console.WriteLine($"Selected: {calendar.SelectedDate.Value:D}");
    }
    else
    {
        Console.WriteLine("Selection cleared (None clicked)");
    }
};
```

## Header Customization

### Header Behavior

The header displays:
- **Month View**: Month and Year (e.g., "March 2026")
- **Year View**: Year (e.g., "2026")
- **Decade View**: Decade range (e.g., "2020 - 2029")
- **Century View**: Century range (e.g., "2000 - 2099")

Clicking the header navigates to the next higher view level.

### Customize Header Appearance

**C#:**
```csharp
// Access header style through calendar style
calendar.Style.Header.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
calendar.Style.Header.ForeColor = Color.DarkBlue;
calendar.Style.Header.BackColor = Color.LightGray;
```

### Header Format

The header format is culture-dependent. To customize:

**C#:**
```csharp
using System.Globalization;

// Set culture for date formatting
calendar.Culture = new CultureInfo("en-US");  // "March 2026"
// calendar.Culture = new CultureInfo("fr-FR");  // "mars 2026"
// calendar.Culture = new CultureInfo("de-DE");  // "März 2026"
```

### Disable Header Click Navigation

**C#:**
```csharp
// Prevent navigation via header click
calendar.AllowViewNavigation = false;

// Or handle ViewChanging event to control navigation
calendar.ViewChanging += (sender, e) =>
{
    // Allow only certain view changes
    if (e.NewValue == CalendarView.Century)
    {
        e.Cancel = true;  // Don't allow navigation to Century view
    }
};
```

## Complete Example: Multi-View Calendar

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using System;
using System.Drawing;
using System.Windows.Forms;

public class MultiViewCalendarForm : Form
{
    private SfCalendar calendar;
    private Label lblInfo;
    private GroupBox grpNavigation;
    private RadioButton rbMonth;
    private RadioButton rbYear;
    private RadioButton rbDecade;
    private RadioButton rbCentury;
    
    public MultiViewCalendarForm()
    {
        SetupCalendar();
        SetupNavigationControls();
        SetupInfoLabel();
        
        this.Text = "Multi-View Calendar";
        this.Size = new Size(600, 480);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void SetupCalendar()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 20),
            Size = new Size(350, 360),
            ShowToday = true,
            ShowNone = true,
            SelectedDate = DateTime.Today
        };
        
        calendar.ViewChanged += Calendar_ViewChanged;
        calendar.SelectedDateChanged += Calendar_SelectedDateChanged;
        
        this.Controls.Add(calendar);
    }
    
    private void SetupNavigationControls()
    {
        grpNavigation = new GroupBox
        {
            Location = new Point(390, 20),
            Size = new Size(180, 150),
            Text = "View Navigation"
        };
        
        rbMonth = new RadioButton
        {
            Location = new Point(15, 25),
            Size = new Size(150, 25),
            Text = "Month View",
            Checked = true
        };
        rbMonth.CheckedChanged += (s, e) => 
        {
            if (rbMonth.Checked) calendar.View = CalendarView.Month;
        };
        
        rbYear = new RadioButton
        {
            Location = new Point(15, 55),
            Size = new Size(150, 25),
            Text = "Year View"
        };
        rbYear.CheckedChanged += (s, e) => 
        {
            if (rbYear.Checked) calendar.View = CalendarView.Year;
        };
        
        rbDecade = new RadioButton
        {
            Location = new Point(15, 85),
            Size = new Size(150, 25),
            Text = "Decade View"
        };
        rbDecade.CheckedChanged += (s, e) => 
        {
            if (rbDecade.Checked) calendar.View = CalendarView.Decade;
        };
        
        rbCentury = new RadioButton
        {
            Location = new Point(15, 115),
            Size = new Size(150, 25),
            Text = "Century View"
        };
        rbCentury.CheckedChanged += (s, e) => 
        {
            if (rbCentury.Checked) calendar.View = CalendarView.Century;
        };
        
        grpNavigation.Controls.AddRange(new Control[] {
            rbMonth, rbYear, rbDecade, rbCentury
        });
        
        this.Controls.Add(grpNavigation);
    }
    
    private void SetupInfoLabel()
    {
        lblInfo = new Label
        {
            Location = new Point(20, 390),
            Size = new Size(550, 50),
            Text = GetInfoText(),
            BorderStyle = BorderStyle.FixedSingle,
            Padding = new Padding(5)
        };
        
        this.Controls.Add(lblInfo);
    }
    
    private void Calendar_ViewChanged(object sender, EventArgs e)
    {
        lblInfo.Text = GetInfoText();
        
        // Update radio button selection
        switch (calendar.View)
        {
            case CalendarView.Month:
                rbMonth.Checked = true;
                break;
            case CalendarView.Year:
                rbYear.Checked = true;
                break;
            case CalendarView.Decade:
                rbDecade.Checked = true;
                break;
            case CalendarView.Century:
                rbCentury.Checked = true;
                break;
        }
    }
    
    private void Calendar_SelectedDateChanged(object sender, EventArgs e)
    {
        lblInfo.Text = GetInfoText();
    }
    
    private string GetInfoText()
    {
        string view = $"View: {calendar.View}";
        string selected = calendar.SelectedDate.HasValue
            ? $"Selected: {calendar.SelectedDate.Value:D}"
            : "Selected: None";
        
        return $"{view} | {selected}";
    }
    
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MultiViewCalendarForm());
    }
}
```

## Next Steps

- **[Appearance Customization](appearance-customization.md)** - Learn how to customize colors, fonts, themes, and cell styling
- **[Date Selection](date-selection.md)** - Explore date selection modes, restrictions, and special dates
- **[Globalization](globalization.md)** - Localize calendar for different cultures
