# Customizing Appearance

The Windows Forms Scheduler provides extensive appearance customization through the `Appearance` property and the `ScheduleAppearance` object.

## Table of Contents
- [Accessing Appearance Properties](#accessing-appearance-properties)
- [Border Customization](#border-customization)
- [Caption Customization](#caption-customization)
- [Display Item Formats](#display-item-formats)
- [Header Customization](#header-customization)
- [Navigation Calendar Appearance](#navigation-calendar-appearance)
- [Prime Time Configuration](#prime-time-configuration)
- [Time Column Customization](#time-column-customization)
- [Visual Styles and Themes](#visual-styles-and-themes)
- [Complete Appearance Examples](#complete-appearance-examples)

## Accessing Appearance Properties

Access the `ScheduleAppearance` object through the control's `Appearance` property:

```csharp
// Get appearance object
ScheduleAppearance appearance = scheduleControl1.Appearance;

// Modify properties
appearance.CaptionBackColor = Color.Blue;
appearance.PrimeTimeCellColor = Color.LightYellow;
```

## Border Customization

### Border Colors

```csharp
// Border color of clicked/selected appointment
appearance.ClickItemBorderColor = Color.Red;

// Color of appointment being dragged
appearance.DragColor = Color.Blue;

// Color of solid grid lines in calendar
appearance.SolidBorderColor = Color.Gray;
```

**Use Cases:**
- **ClickItemBorderColor:** Highlight selected appointments with distinct color
- **DragColor:** Visual feedback during drag operations
- **SolidBorderColor:** Adjust grid visibility for different themes

## Caption Customization

The caption panel appears above the schedule with navigation buttons.

### Caption Properties

```csharp
// Show/hide caption panel
appearance.ShowCaption = true;

// Show/hide navigation buttons (forward/backward)
appearance.ShowCaptionButtons = true;

// Caption background color
appearance.CaptionBackColor = Color.FromArgb(0, 114, 198); // Blue

// Or use standard colors
appearance.CaptionBackColor = Color.DarkBlue;
appearance.CaptionBackColor = Color.FromArgb(240, 240, 240); // Light gray
```

**Example - Professional Blue Header:**
```csharp
appearance.ShowCaption = true;
appearance.ShowCaptionButtons = true;
appearance.CaptionBackColor = Color.FromArgb(0, 114, 198);
```

## Display Item Formats

Control how appointments and dates are formatted using format strings.

### Date and Time Formats

```csharp
// Date-only format (e.g., "03/24/2026")
appearance.DateFormat = "MM/dd/yyyy";

// Time-only format (e.g., "9:00 AM")
appearance.TimeFormat = "h:mm tt";

// Combined date/time format (e.g., "03/24/2026 9:00 AM")
appearance.DateTimeFormat = "MM/dd/yyyy h:mm tt";
```

### View-Specific Formats

**Day View:**
```csharp
// Appointment display in day view (includes time and subject)
appearance.DayItemFormat = "{StartTime} - {EndTime}: {Subject}";

// Long header format (e.g., "Monday, March 24, 2026")
appearance.LongHeaderFormat = "dddd, MMMM dd, yyyy";
```

**Week View:**
```csharp
// Header format in week view (e.g., "Mon 3/24")
appearance.FullWeekHeaderFormat = "ddd M/d";

// Header format in workweek view (e.g., "Monday, 3/24")
appearance.WorkWeekHeaderFormat = "dddd, M/d";

// Appointment display in week view
appearance.WeekMonthItemFormat = "{Subject}";
```

**Month View:**
```csharp
// Header label in workweek view
appearance.WeekHeaderFormat = "ddd M/d";

// Appointment display in month view (subject only or with time)
appearance.WeekMonthItemFormat = "{Subject}";
```

**All-Day Appointments:**
```csharp
// Format for all-day appointments
appearance.AllDayItemFormat = "{Subject} (All Day)";
```

### Multi-Day Span Formats

For appointments spanning multiple days:

```csharp
// Left side of span (shows start date)
appearance.SpanItemFormatLeftText = "{StartDate}: {Subject}";

// Middle of span (shows subject)
appearance.SpanItemFormatMiddleText = "{Subject}";

// Right side of span (shows end date)
appearance.SpanItemFormatRightText = "{Subject} - {EndDate}";

// Terminal left (span continues from previous view)
appearance.SpanItemFormatTerminalLeftText = "... {Subject}";

// Terminal right (span continues into next view)
appearance.SpanItemFormatTerminalRightText = "{Subject} ...";
```

### Available Format Tokens

- **{Subject}** - Appointment subject
- **{Content}** - Appointment content/description
- **{StartTime}** - Start time
- **{EndTime}** - End time
- **{StartDate}** - Start date
- **{EndDate}** - End date
- **{Location}** - Location name
- **{Duration}** - Appointment duration

## Header Customization

### All-Day Row

```csharp
// Background color of all-day row
appearance.AllDayBackColor = Color.LightGray;
```

### Month and Week View Headers

```csharp
// Month/Week view header background
appearance.MonthWeekHeaderBackColor = Color.FromArgb(230, 230, 230);

// Month/Week view header text color
appearance.MonthWeekHeaderForeColor = Color.Black;
```

### WorkWeek View Headers

```csharp
// WorkWeek view header background
appearance.WorkWeekHeaderBackColor = Color.FromArgb(200, 220, 240);

// WorkWeek view header text color
appearance.WorkWeekHeaderForeColor = Color.DarkBlue;
```

**Example - Consistent Header Styling:**
```csharp
Color headerBg = Color.FromArgb(245, 245, 245);
Color headerFg = Color.FromArgb(50, 50, 50);

appearance.MonthWeekHeaderBackColor = headerBg;
appearance.MonthWeekHeaderForeColor = headerFg;
appearance.WorkWeekHeaderBackColor = headerBg;
appearance.WorkWeekHeaderForeColor = headerFg;
```

## Navigation Calendar Appearance

Customize the navigation calendar in the sidebar.

### Colors

```csharp
// Background color
appearance.NavigationCalendarBackColor = Color.White;

// Header color (month/year label)
appearance.NavigationCalendarHeaderColor = Color.FromArgb(0, 114, 198);

// Regular text color
appearance.NavigationCalendarTextColor = Color.Black;

// Disabled date text color
appearance.NavigationCalendarDisabledTextColor = Color.LightGray;

// Today's date color
appearance.NavigationCalendarTodayColor = Color.Red;

// Selected date background color
appearance.NavigationCalendarSelectionColor = Color.FromArgb(173, 216, 230);

// Week number text color
appearance.NavigationCalendarWeekNumberColor = Color.Gray;

// Arrow button color
appearance.NavigationCalendarArrowColor = Color.Black;
```

### Start Day of Week

```csharp
// Set Monday as first day
appearance.NavigationCalendarStartDayOfWeek = DayOfWeek.Monday;

// Set Sunday as first day (default)
appearance.NavigationCalendarStartDayOfWeek = DayOfWeek.Sunday;
```

**Example - Professional Navigation Calendar:**
```csharp
appearance.NavigationCalendarBackColor = Color.White;
appearance.NavigationCalendarHeaderColor = Color.FromArgb(0, 114, 198);
appearance.NavigationCalendarSelectionColor = Color.FromArgb(173, 216, 230);
appearance.NavigationCalendarTodayColor = Color.FromArgb(255, 0, 0);
appearance.NavigationCalendarTextColor = Color.Black;
appearance.NavigationCalendarStartDayOfWeek = DayOfWeek.Monday;
```

## Prime Time Configuration

Prime time highlights specific hours with a different background color (typically business hours).

### Prime Time Properties

```csharp
// Prime time start (e.g., 9:00 AM)
appearance.PrimeTimeStart = 9;

// Prime time end (e.g., 5:00 PM)
appearance.PrimeTimeEnd = 17;

// Prime time cell background color
appearance.PrimeTimeCellColor = Color.White;

// Non-prime time cell background color
appearance.NonPrimeTimeCellColor = Color.FromArgb(240, 240, 240);
```

**Example - Standard Business Hours:**
```csharp
// 9 AM to 5 PM as prime time
appearance.PrimeTimeStart = 9;
appearance.PrimeTimeEnd = 17;
appearance.PrimeTimeCellColor = Color.White;
appearance.NonPrimeTimeCellColor = Color.FromArgb(245, 245, 245);
```

**Example - Extended Business Hours:**
```csharp
// 8 AM to 6 PM as prime time
appearance.PrimeTimeStart = 8;
appearance.PrimeTimeEnd = 18;
appearance.PrimeTimeCellColor = Color.LightYellow;
appearance.NonPrimeTimeCellColor = Color.White;
```

## Time Column Customization

The time column appears on the left in Day, Week, and WorkWeek views.

### Visibility and Format

```csharp
// Show/hide time column
appearance.ShowTime = true;

// Use 24-hour format (true) or 12-hour format (false)
appearance.Hours24 = false; // 12-hour with AM/PM

// 24-hour format
appearance.Hours24 = true; // 00:00 to 23:00
```

### Colors

```csharp
// Time column background color
appearance.TimeBackColor = Color.FromArgb(240, 240, 240);

// Time column text color
appearance.TimeTextColor = Color.Black;

// Mark column color (thick line next to time column)
appearance.MarkColumnColor = Color.Blue;
```

### Font Sizes

```csharp
// Larger font size (for hour labels)
appearance.TimeBigFontSize = 10;

// Smaller font size (for minute labels)
appearance.TimeLittleFontSize = 8;
```

**Example - Clear Time Column:**
```csharp
appearance.ShowTime = true;
appearance.Hours24 = false; // 12-hour format
appearance.TimeBackColor = Color.FromArgb(250, 250, 250);
appearance.TimeTextColor = Color.FromArgb(60, 60, 60);
appearance.TimeBigFontSize = 10;
appearance.TimeLittleFontSize = 8;
appearance.MarkColumnColor = Color.FromArgb(0, 114, 198);
```

## Visual Styles and Themes

### Applying Visual Styles

The `VisualStyle` property applies predefined themes:

```csharp
// Office 2003 style
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2003;

// Office 2007 styles
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2007Blue;
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2007Silver;
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2007Black;

// Office 2010 styles
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2010Blue;
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2010Silver;
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Office2010Black;

// Metro style (modern flat design)
appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Metro;
```

### Metro Theme

Apply the modern Metro theme:

```csharp
// Access schedule grid
var scheduleHost = scheduleControl1.GetScheduleHost();

// Apply Metro style
scheduleHost.Schedule.Appearance.VisualStyle = Syncfusion.Windows.Forms.GridVisualStyles.Metro;
```

### Enable/Disable Themes

```csharp
// Enable theme support
appearance.ThemesEnabled = true;

// Disable themes (use custom colors)
appearance.ThemesEnabled = false;
```


### ThemeChanged Event

Respond to theme changes:

```csharp
// Access the schedule host first
var scheduleHost = scheduleControl1.GetScheduleHost();
scheduleHost.ThemeChanged += ScheduleHost_ThemeChanged;

private void ScheduleHost_ThemeChanged(object sender, EventArgs e)
{
    Console.WriteLine("Theme changed");
    // Perform additional customization
}
```

## Additional Appearance Properties

### Miscellaneous Settings

```csharp
// Maximum days side-by-side in day view
appearance.DayMonthCutoff = 3; // Show up to 3 days

// Time divisions per hour (2, 3, 4, or 6)
appearance.DivisionsPerHour = 4; // 15-minute intervals

// Show full 7-day week in month view
appearance.MonthShowFullWeek = true;

// Start day of week for month calendar
appearance.MonthCalendarStartDayOfWeek = DayOfWeek.Monday;

// Start day of week for week calendar
appearance.WeekCalendarStartDayOfWeek = DayOfWeek.Sunday;

// Basic text color
appearance.TextColor = Color.Black;

// Splitter background color
appearance.SplitterBackColor = Color.Gray;
```

### Appointment Tooltips

```csharp
// Enable appointment tooltips on hover
appearance.ScheduleAppointmentTipsEnabled = true;

// Tooltip format string
appearance.ScheduleAppointmentTipFormat = "{Subject}\n{StartTime} - {EndTime}\n{Location}";
```

## Complete Appearance Examples

### Professional Blue Theme

```csharp
public void ApplyProfessionalBlueTheme(ScheduleControl scheduleControl)
{
    ScheduleAppearance appearance = scheduleControl.Appearance;
    
    // Caption
    appearance.ShowCaption = true;
    appearance.ShowCaptionButtons = true;
    appearance.CaptionBackColor = Color.FromArgb(0, 114, 198);
    
    // Headers
    appearance.MonthWeekHeaderBackColor = Color.FromArgb(230, 240, 250);
    appearance.MonthWeekHeaderForeColor = Color.FromArgb(0, 114, 198);
    appearance.WorkWeekHeaderBackColor = Color.FromArgb(230, 240, 250);
    appearance.WorkWeekHeaderForeColor = Color.FromArgb(0, 114, 198);
    
    // Navigation Calendar
    appearance.NavigationCalendarBackColor = Color.White;
    appearance.NavigationCalendarHeaderColor = Color.FromArgb(0, 114, 198);
    appearance.NavigationCalendarSelectionColor = Color.FromArgb(173, 216, 230);
    appearance.NavigationCalendarTodayColor = Color.Red;
    
    // Prime Time (9 AM - 5 PM)
    appearance.PrimeTimeStart = 9;
    appearance.PrimeTimeEnd = 17;
    appearance.PrimeTimeCellColor = Color.White;
    appearance.NonPrimeTimeCellColor = Color.FromArgb(245, 245, 245);
    
    // Time Column
    appearance.ShowTime = true;
    appearance.Hours24 = false;
    appearance.TimeBackColor = Color.FromArgb(240, 240, 240);
    appearance.TimeTextColor = Color.FromArgb(60, 60, 60);
    appearance.MarkColumnColor = Color.FromArgb(0, 114, 198);
    
    // Borders
    appearance.SolidBorderColor = Color.FromArgb(200, 200, 200);
    appearance.ClickItemBorderColor = Color.FromArgb(0, 114, 198);
    
    // Tooltips
    appearance.ScheduleAppointmentTipsEnabled = true;
}
```

### Dark Theme

```csharp
public void ApplyDarkTheme(ScheduleControl scheduleControl)
{
    ScheduleAppearance appearance = scheduleControl.Appearance;
    
    Color darkBg = Color.FromArgb(45, 45, 45);
    Color lightText = Color.FromArgb(230, 230, 230);
    Color accentColor = Color.FromArgb(0, 122, 204);
    
    // Caption
    appearance.CaptionBackColor = darkBg;
    
    // Headers
    appearance.MonthWeekHeaderBackColor = Color.FromArgb(60, 60, 60);
    appearance.MonthWeekHeaderForeColor = lightText;
    appearance.WorkWeekHeaderBackColor = Color.FromArgb(60, 60, 60);
    appearance.WorkWeekHeaderForeColor = lightText;
    
    // Navigation Calendar
    appearance.NavigationCalendarBackColor = darkBg;
    appearance.NavigationCalendarHeaderColor = Color.FromArgb(60, 60, 60);
    appearance.NavigationCalendarTextColor = lightText;
    appearance.NavigationCalendarSelectionColor = accentColor;
    appearance.NavigationCalendarTodayColor = Color.FromArgb(255, 100, 100);
    
    // Prime Time
    appearance.PrimeTimeCellColor = Color.FromArgb(50, 50, 50);
    appearance.NonPrimeTimeCellColor = Color.FromArgb(35, 35, 35);
    
    // Time Column
    appearance.TimeBackColor = Color.FromArgb(55, 55, 55);
    appearance.TimeTextColor = lightText;
    appearance.MarkColumnColor = accentColor;
    
    // Text
    appearance.TextColor = lightText;
    
    // Borders
    appearance.SolidBorderColor = Color.FromArgb(80, 80, 80);
}
```

### Healthcare Theme (15-minute intervals)

```csharp
public void ApplyHealthcareTheme(ScheduleControl scheduleControl)
{
    ScheduleAppearance appearance = scheduleControl.Appearance;
    
    // 15-minute time slots
    appearance.DivisionsPerHour = 4;
    
    // 12-hour format with clear time display
    appearance.Hours24 = false;
    appearance.TimeBigFontSize = 11;
    appearance.TimeLittleFontSize = 9;
    
    // Clinic hours (8 AM - 6 PM)
    appearance.PrimeTimeStart = 8;
    appearance.PrimeTimeEnd = 18;
    appearance.PrimeTimeCellColor = Color.FromArgb(250, 255, 250);
    appearance.NonPrimeTimeCellColor = Color.FromArgb(240, 240, 240);
    
    // Clean, clinical appearance
    appearance.CaptionBackColor = Color.FromArgb(100, 150, 100);
    appearance.MonthWeekHeaderBackColor = Color.FromArgb(220, 240, 220);
    appearance.TimeBackColor = Color.FromArgb(245, 250, 245);
    
    // Clear borders
    appearance.SolidBorderColor = Color.FromArgb(150, 150, 150);
    
    // Tooltips enabled for patient details
    appearance.ScheduleAppointmentTipsEnabled = true;
    appearance.ScheduleAppointmentTipFormat = 
        "{Subject}\n{StartTime} - {EndTime}\nLocation: {Location}";
}
```

## Best Practices

1. **Consistent Color Scheme:** Use harmonious colors across all components
2. **Accessibility:** Ensure sufficient contrast between text and backgrounds
3. **Prime Time:** Configure based on actual business hours for better visual hierarchy
4. **Time Divisions:** Match divisions to scheduling granularity (15 min for healthcare, 30 min for general)
5. **Themes:** Use built-in themes as starting point, customize as needed
6. **Test Multiple Views:** Verify appearance across Month, Week, and Day views
7. **Navigation Calendar:** Make it visually distinct but harmonious with main schedule
8. **Tooltips:** Enable for better user experience, include relevant appointment details
