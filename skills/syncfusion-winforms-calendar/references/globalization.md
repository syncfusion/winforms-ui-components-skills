# Globalization and Localization

## When to Read This

Read this guide when you need to:
- Display calendar in different languages and cultures
- Change the first day of the week (Sunday vs Monday)
- Customize date formats for different regions
- Support right-to-left (RTL) languages like Arabic or Hebrew
- Localize month and day names
- Create multi-culture applications

## Overview

SfCalendar supports full globalization through the `Culture` property, allowing you to display calendar content in different languages, formats, and regional settings.

## Culture and Localization

### Setting Culture

**C#:**
```csharp
using System.Globalization;

SfCalendar calendar = new SfCalendar();

// English (United States)
calendar.Culture = new CultureInfo("en-US");

// French (France)
calendar.Culture = new CultureInfo("fr-FR");

// German (Germany)
calendar.Culture = new CultureInfo("de-DE");

// Japanese (Japan)
calendar.Culture = new CultureInfo("ja-JP");

// Arabic (Saudi Arabia)
calendar.Culture = new CultureInfo("ar-SA");
```

**VB.NET:**
```vb
Imports System.Globalization

Dim calendar As New SfCalendar()

' English (United States)
calendar.Culture = New CultureInfo("en-US")

' French (France)
calendar.Culture = New CultureInfo("fr-FR")
```

### Common Culture Codes

| Culture Code | Language/Region |
|-------------|-----------------|
| en-US | English (United States) |
| en-GB | English (United Kingdom) |
| fr-FR | French (France) |
| de-DE | German (Germany) |
| es-ES | Spanish (Spain) |
| it-IT | Italian (Italy) |
| ja-JP | Japanese (Japan) |
| zh-CN | Chinese (Simplified, China) |
| zh-TW | Chinese (Traditional, Taiwan) |
| ko-KR | Korean (Korea) |
| ar-SA | Arabic (Saudi Arabia) |
| he-IL | Hebrew (Israel) |
| ru-RU | Russian (Russia) |
| pt-BR | Portuguese (Brazil) |
| hi-IN | Hindi (India) |

### Culture Effects

When you set the `Culture` property, it automatically affects:
- Month names (January, Janvier, Januar, etc.)
- Day names (Monday, Lundi, Montag, etc.)
- First day of week (Sunday for US, Monday for Europe)
- Date format (MM/dd/yyyy vs dd/MM/yyyy)
- Calendar system (Gregorian, Hijri, etc.)

**Example:**
```csharp
// English (US): January, February, March...
calendar.Culture = new CultureInfo("en-US");

// French: Janvier, Février, Mars...
calendar.Culture = new CultureInfo("fr-FR");

// German: Januar, Februar, März...
calendar.Culture = new CultureInfo("de-DE");

// Japanese: 1月, 2月, 3月...
calendar.Culture = new CultureInfo("ja-JP");
```

## First Day of Week

### Setting First Day

**C#:**
```csharp
// Start week on Monday (common in Europe)
calendar.FirstDayOfWeek = DayOfWeek.Monday;

// Start week on Sunday (common in US)
calendar.FirstDayOfWeek = DayOfWeek.Sunday;

// Start week on Saturday
calendar.FirstDayOfWeek = DayOfWeek.Saturday;
```

**VB.NET:**
```vb
' Start week on Monday
calendar.FirstDayOfWeek = DayOfWeek.Monday
```

### Culture-Based First Day

**C#:**
```csharp
// Use first day from culture
CultureInfo culture = new CultureInfo("en-GB");  // UK uses Monday
calendar.Culture = culture;
calendar.FirstDayOfWeek = culture.DateTimeFormat.FirstDayOfWeek;

// US culture (Sunday)
CultureInfo usCulture = new CultureInfo("en-US");
calendar.FirstDayOfWeek = usCulture.DateTimeFormat.FirstDayOfWeek;  // Sunday
```

### First Day Examples

**C#:**
```csharp
// European calendar (Monday start)
calendar.Culture = new CultureInfo("de-DE");
calendar.FirstDayOfWeek = DayOfWeek.Monday;

// US calendar (Sunday start)
calendar.Culture = new CultureInfo("en-US");
calendar.FirstDayOfWeek = DayOfWeek.Sunday;

// Middle East calendar (Saturday start)
calendar.Culture = new CultureInfo("ar-SA");
calendar.FirstDayOfWeek = DayOfWeek.Saturday;
```

## Date Format Customization

### Header Date Format

**C#:**
```csharp
// Default: "MMMM yyyy" (e.g., "March 2024")
calendar.HeaderText = "{0:MMMM yyyy}";

// Short month: "MMM yyyy" (e.g., "Mar 2024")
calendar.HeaderText = "{0:MMM yyyy}";

// Full date: "MMMM dd, yyyy" (e.g., "March 15, 2024")
calendar.HeaderText = "{0:MMMM dd, yyyy}";

// Numeric: "MM/yyyy" (e.g., "03/2024")
calendar.HeaderText = "{0:MM/yyyy}";
```

**VB.NET:**
```vb
' Default format
calendar.HeaderText = "{0:MMMM yyyy}"
```

### Date Format Examples

**C#:**
```csharp
// US format (MM/dd/yyyy)
calendar.Culture = new CultureInfo("en-US");
string formatted = calendar.SelectedDate.ToString("d", calendar.Culture);
// Result: "3/15/2024"

// European format (dd/MM/yyyy)
calendar.Culture = new CultureInfo("en-GB");
formatted = calendar.SelectedDate.ToString("d", calendar.Culture);
// Result: "15/03/2024"

// ISO format (yyyy-MM-dd)
formatted = calendar.SelectedDate.ToString("yyyy-MM-dd");
// Result: "2024-03-15"

// Long date
calendar.Culture = new CultureInfo("fr-FR");
formatted = calendar.SelectedDate.ToString("D", calendar.Culture);
// Result: "vendredi 15 mars 2024"
```

## Month and Day Name Localization

### Automatic Localization

Month and day names are automatically localized based on the `Culture` property:

**C#:**
```csharp
// English months
calendar.Culture = new CultureInfo("en-US");
// Displays: January, February, March...
// Days: Sunday, Monday, Tuesday...

// French months
calendar.Culture = new CultureInfo("fr-FR");
// Displays: Janvier, Février, Mars...
// Days: Dimanche, Lundi, Mardi...

// German months
calendar.Culture = new CultureInfo("de-DE");
// Displays: Januar, Februar, März...
// Days: Sonntag, Montag, Dienstag...

// Spanish months
calendar.Culture = new CultureInfo("es-ES");
// Displays: Enero, Febrero, Marzo...
// Days: Domingo, Lunes, Martes...
```

### Abbreviated vs Full Names

**C#:**
```csharp
CultureInfo culture = new CultureInfo("en-US");

// Abbreviated day names
string[] shortDays = culture.DateTimeFormat.AbbreviatedDayNames;
// Result: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]

// Full day names
string[] fullDays = culture.DateTimeFormat.DayNames;
// Result: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

// Abbreviated month names
string[] shortMonths = culture.DateTimeFormat.AbbreviatedMonthNames;
// Result: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec", ""]

// Full month names
string[] fullMonths = culture.DateTimeFormat.MonthNames;
// Result: ["January", "February", "March", ...]
```

## RTL (Right-to-Left) Support

### Enabling RTL

**C#:**
```csharp
// Enable RTL for Arabic
calendar.Culture = new CultureInfo("ar-SA");
calendar.RightToLeft = RightToLeft.Yes;

// Enable RTL for Hebrew
calendar.Culture = new CultureInfo("he-IL");
calendar.RightToLeft = RightToLeft.Yes;
```

**VB.NET:**
```vb
' Enable RTL for Arabic
calendar.Culture = New CultureInfo("ar-SA")
calendar.RightToLeft = RightToLeft.Yes
```

### RTL Effects

When `RightToLeft = Yes`:
- Calendar layout mirrors (week starts from right)
- Navigation buttons swap (previous on right, next on left)
- Text alignment adjusts
- Day columns reverse order

**Example:**
```csharp
// Arabic calendar with RTL
calendar.Culture = new CultureInfo("ar-SA");
calendar.RightToLeft = RightToLeft.Yes;
calendar.FirstDayOfWeek = DayOfWeek.Saturday;
// Displays: Arabic month names, RTL layout, Saturday as first day
```

### RTL Options

**C#:**
```csharp
// Explicit RTL
calendar.RightToLeft = RightToLeft.Yes;

// Explicit LTR
calendar.RightToLeft = RightToLeft.No;

// Inherit from parent
calendar.RightToLeft = RightToLeft.Inherit;
```

## Complete Globalization Example

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using System;
using System.Drawing;
using System.Globalization;
using System.Windows.Forms;

public class GlobalizationForm : Form
{
    private SfCalendar calendar;
    private ComboBox cmbCulture;
    private ComboBox cmbFirstDay;
    private TextBox txtDateFormat;
    private CheckBox chkRTL;
    private Button btnApply;
    private Label lblSelectedDate;
    
    private readonly CultureInfo[] cultures = new CultureInfo[]
    {
        new CultureInfo("en-US"),  // English (US)
        new CultureInfo("en-GB"),  // English (UK)
        new CultureInfo("fr-FR"),  // French
        new CultureInfo("de-DE"),  // German
        new CultureInfo("es-ES"),  // Spanish
        new CultureInfo("it-IT"),  // Italian
        new CultureInfo("ja-JP"),  // Japanese
        new CultureInfo("zh-CN"),  // Chinese (Simplified)
        new CultureInfo("ar-SA"),  // Arabic
        new CultureInfo("he-IL"),  // Hebrew
        new CultureInfo("ru-RU"),  // Russian
        new CultureInfo("pt-BR"),  // Portuguese (Brazil)
        new CultureInfo("hi-IN")   // Hindi
    };
    
    public GlobalizationForm()
    {
        SetupCalendar();
        SetupControls();
        
        this.Text = "Calendar Globalization";
        this.Size = new Size(640, 560);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void SetupCalendar()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 20),
            Size = new Size(380, 400),
            SelectedDate = DateTime.Today
        };
        
        calendar.SelectionChanged += Calendar_SelectionChanged;
        
        this.Controls.Add(calendar);
    }
    
    private void SetupControls()
    {
        // Culture selector
        Label lblCulture = new Label
        {
            Location = new Point(420, 20),
            Size = new Size(180, 20),
            Text = "Culture:"
        };
        
        cmbCulture = new ComboBox
        {
            Location = new Point(420, 45),
            Size = new Size(180, 25),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        foreach (CultureInfo culture in cultures)
        {
            cmbCulture.Items.Add($"{culture.DisplayName} ({culture.Name})");
        }
        cmbCulture.SelectedIndex = 0;  // Default to en-US
        
        // First day of week
        Label lblFirstDay = new Label
        {
            Location = new Point(420, 85),
            Size = new Size(180, 20),
            Text = "First Day of Week:"
        };
        
        cmbFirstDay = new ComboBox
        {
            Location = new Point(420, 110),
            Size = new Size(180, 25),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        foreach (DayOfWeek day in Enum.GetValues(typeof(DayOfWeek)))
        {
            cmbFirstDay.Items.Add(day.ToString());
        }
        cmbFirstDay.SelectedIndex = 0;  // Default to Sunday
        
        // Date format
        Label lblDateFormat = new Label
        {
            Location = new Point(420, 150),
            Size = new Size(180, 20),
            Text = "Header Format:"
        };
        
        txtDateFormat = new TextBox
        {
            Location = new Point(420, 175),
            Size = new Size(180, 25),
            Text = "{0:MMMM yyyy}"
        };
        
        // RTL support
        chkRTL = new CheckBox
        {
            Location = new Point(420, 215),
            Size = new Size(180, 25),
            Text = "Right-to-Left (RTL)"
        };
        
        // Apply button
        btnApply = new Button
        {
            Location = new Point(420, 255),
            Size = new Size(180, 35),
            Text = "Apply Settings"
        };
        btnApply.Click += BtnApply_Click;
        
        // Selected date display
        lblSelectedDate = new Label
        {
            Location = new Point(20, 435),
            Size = new Size(580, 60),
            Text = "Selected: " + DateTime.Today.ToString("D", CultureInfo.CurrentCulture),
            BorderStyle = BorderStyle.FixedSingle,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Segoe UI", 10F)
        };
        
        this.Controls.AddRange(new Control[] {
            lblCulture, cmbCulture,
            lblFirstDay, cmbFirstDay,
            lblDateFormat, txtDateFormat,
            chkRTL, btnApply, lblSelectedDate
        });
    }
    
    private void BtnApply_Click(object sender, EventArgs e)
    {
        // Apply culture
        CultureInfo selectedCulture = cultures[cmbCulture.SelectedIndex];
        calendar.Culture = selectedCulture;
        
        // Apply first day of week
        calendar.FirstDayOfWeek = (DayOfWeek)cmbFirstDay.SelectedIndex;
        
        // Apply date format
        calendar.HeaderText = txtDateFormat.Text;
        
        // Apply RTL
        calendar.RightToLeft = chkRTL.Checked ? RightToLeft.Yes : RightToLeft.No;
        
        // Update selected date display
        UpdateSelectedDateDisplay();
    }
    
    private void Calendar_SelectionChanged(object sender, Syncfusion.WinForms.Input.Events.SelectionChangedEventArgs e)
    {
        UpdateSelectedDateDisplay();
    }
    
    private void UpdateSelectedDateDisplay()
    {
        if (calendar.SelectedDate.HasValue)
        {
            CultureInfo culture = calendar.Culture ?? CultureInfo.CurrentCulture;
            
            string shortDate = calendar.SelectedDate.Value.ToString("d", culture);
            string longDate = calendar.SelectedDate.Value.ToString("D", culture);
            
            lblSelectedDate.Text = $"Selected:\n{shortDate}\n{longDate}";
        }
        else
        {
            lblSelectedDate.Text = "No date selected";
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Input
Imports System.Globalization

Public Class GlobalizationForm
    Inherits Form
    
    Private calendar As SfCalendar
    Private cmbCulture As ComboBox
    Private cmbFirstDay As ComboBox
    Private chkRTL As CheckBox
    
    Public Sub New()
        SetupCalendar()
        SetupControls()
        
        Me.Text = "Calendar Globalization"
        Me.Size = New Size(640, 560)
    End Sub
    
    Private Sub SetupCalendar()
        calendar = New SfCalendar With {
            .Location = New Point(20, 20),
            .Size = New Size(380, 400),
            .SelectedDate = DateTime.Today
        }
        
        Me.Controls.Add(calendar)
    End Sub
    
    Private Sub SetupControls()
        ' Culture selector
        cmbCulture = New ComboBox With {
            .Location = New Point(420, 45),
            .Size = New Size(180, 25),
            .DropDownStyle = ComboBoxStyle.DropDownList
        }
        
        cmbCulture.Items.AddRange(New String() {
            "English (US) - en-US",
            "English (UK) - en-GB",
            "French - fr-FR",
            "German - de-DE",
            "Arabic - ar-SA"
        })
        cmbCulture.SelectedIndex = 0
        
        ' First day of week
        cmbFirstDay = New ComboBox With {
            .Location = New Point(420, 110),
            .Size = New Size(180, 25),
            .DropDownStyle = ComboBoxStyle.DropDownList
        }
        
        For Each day As DayOfWeek In [Enum].GetValues(GetType(DayOfWeek))
            cmbFirstDay.Items.Add(day.ToString())
        Next
        cmbFirstDay.SelectedIndex = 0
        
        ' RTL checkbox
        chkRTL = New CheckBox With {
            .Location = New Point(420, 215),
            .Size = New Size(180, 25),
            .Text = "Right-to-Left (RTL)"
        }
        
        Me.Controls.AddRange(New Control() {
            cmbCulture, cmbFirstDay, chkRTL
        })
    End Sub
End Class
```

## Tips for Multi-Culture Applications

### Culture Detection

**C#:**
```csharp
// Use system culture
calendar.Culture = CultureInfo.CurrentCulture;

// Use UI culture (for display)
calendar.Culture = CultureInfo.CurrentUICulture;

// Detect from user settings
string userCultureCode = Properties.Settings.Default.UserCulture;
if (!string.IsNullOrEmpty(userCultureCode))
{
    calendar.Culture = new CultureInfo(userCultureCode);
}
```

### Culture Fallback

**C#:**
```csharp
public void SetCalendarCulture(SfCalendar calendar, string cultureCode)
{
    try
    {
        calendar.Culture = new CultureInfo(cultureCode);
    }
    catch (CultureNotFoundException)
    {
        // Fallback to English if culture not found
        calendar.Culture = new CultureInfo("en-US");
    }
}
```

### Saving Culture Preference

**C#:**
```csharp
// Save user's culture preference
Properties.Settings.Default.UserCulture = calendar.Culture.Name;
Properties.Settings.Default.FirstDayOfWeek = (int)calendar.FirstDayOfWeek;
Properties.Settings.Default.Save();

// Load on startup
calendar.Culture = new CultureInfo(Properties.Settings.Default.UserCulture);
calendar.FirstDayOfWeek = (DayOfWeek)Properties.Settings.Default.FirstDayOfWeek;
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and installation
- **[Appearance Customization](appearance-customization.md)** - Customize visual appearance
- **[Date Selection](date-selection.md)** - Configure selection modes and restrictions
