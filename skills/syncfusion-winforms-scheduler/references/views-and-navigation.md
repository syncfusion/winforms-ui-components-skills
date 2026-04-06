# Views and Navigation

The Windows Forms Scheduler provides multiple view types and navigation mechanisms to display and interact with schedule data.

## Schedule View Types

The `ScheduleType` property controls the current view mode. Five view types are available:

### Month View

Displays a full month calendar with appointments shown as colored bars.

```csharp
scheduleControl1.ScheduleType = ScheduleViewType.Month;
```

**Characteristics:**
- Shows 4-6 weeks of dates
- Appointments appear as colored bars with subject text
- Multiple appointments stack vertically in each date cell
- Best for overview of schedule across weeks
- All-day appointments appear at the top of each date

**Use Cases:**
- Monthly planning and overview
- Identifying busy periods at a glance
- Scheduling multi-day events
- Long-range calendar views

### Day View

Displays a single day with hourly time slots.

```csharp
scheduleControl1.ScheduleType = ScheduleViewType.Day;
```

**Characteristics:**
- Time column on the left (24-hour or 12-hour format)
- Appointments sized proportionally to duration
- Drag-and-drop to reschedule
- Resize appointments to change duration
- All-day row at the top

**Use Cases:**
- Detailed daily scheduling
- Hour-by-hour time management
- Meeting room scheduling
- Healthcare appointment tracking

### Week View

Displays seven days (Sunday-Saturday) side by side.

```csharp
scheduleControl1.ScheduleType = ScheduleViewType.Week;
```

**Characteristics:**
- Seven columns (one per day)
- Time slots for each day
- Shows full week including weekends
- Appointments span across columns for multi-day events

**Use Cases:**
- Weekly planning
- Resource allocation across a week
- Comparing schedules across multiple days
- Event planning with weekend coverage

### WorkWeek View

Displays five weekdays (Monday-Friday).

```csharp
scheduleControl1.ScheduleType = ScheduleViewType.WorkWeek;
```

**Characteristics:**
- Five columns (Monday through Friday)
- Excludes Saturday and Sunday
- Same time slot functionality as Week view
- Focus on business days

**Use Cases:**
- Business calendar management
- Office hour scheduling
- Meeting planning for standard work weeks
- Employee availability tracking

### CustomWeek View

Displays a custom number of days based on the selection in the Navigation Calendar.

```csharp
scheduleControl1.ScheduleType = ScheduleViewType.CustomWeek;
```

**Characteristics:**
- User selects date range in Navigation Calendar
- Can display any number of consecutive days
- Flexible for custom planning periods
- Adapts to selected date range

**Use Cases:**
- Conference or event planning (3-4 days)
- Project sprint planning
- Custom work schedules (4-day weeks)
- Flexible scheduling periods

## Changing Views

### Via Context Menu

Users can change views by:

1. Right-clicking anywhere in the ScheduleGrid area
2. Context menu appears with view options:
   - Day
   - WorkWeek
   - Week
   - Month
   - Custom Week
3. Click desired view option

### Programmatically

```csharp
// Switch to Day view
scheduleControl1.ScheduleType = ScheduleViewType.Day;

// Switch to Week view
scheduleControl1.ScheduleType = ScheduleViewType.Week;

// Switch to WorkWeek view
scheduleControl1.ScheduleType = ScheduleViewType.WorkWeek;

// Switch to Month view
scheduleControl1.ScheduleType = ScheduleViewType.Month;

// Switch to CustomWeek view
scheduleControl1.ScheduleType = ScheduleViewType.CustomWeek;
```

### View-Specific Configuration

```csharp
// Maximum number of days to display side-by-side in Day view
scheduleControl1.Appearance.DayMonthCutOff = 3; // Show up to 3 days

// Number of time divisions per hour in Day/Week views
scheduleControl1.Appearance.DivisionsPerHour = 4; // 15-minute intervals

// Configure Month view to show full week (7 columns)
scheduleControl1.Appearance.MonthShowFullWeek = true;
```

## Navigation Calendar

The Navigation Calendar is a multi-month calendar control for date selection and navigation.

### Accessing Navigation Calendar

```csharp
// Get navigation calendar reference
NavigationCalendar navCalendar = scheduleControl1.Calendar;
```

### Key Properties

```csharp
// Get or set the current date
navCalendar.DateValue = new DateTime(2026, 03, 24);

// Get selected dates
DateTime[] selectedDates = navCalendar.SelectedDates;

// Set today's date (affects "Today" highlighting)
navCalendar.Today = DateTime.Now;

// Show/hide week numbers
navCalendar.ShowWeekNumbers = true;
```

### Navigation Calendar Methods

```csharp
// Get the first day of the month for a given date
DateTime firstDay = navCalendar.FirstDayOfMonth(DateTime.Now);

// Get the Monday before a given date
DateTime monday = navCalendar.MondayBeforeDate(DateTime.Now);

// Get the Sunday after a given date
DateTime sunday = navCalendar.SundayAfterDate(DateTime.Now);
```

### DateValueChanged Event

Detect when users select different dates:

```csharp
// Subscribe to date selection changes
scheduleControl1.Calendar.DateValueChanged += Calendar_DateValueChanged;

private void Calendar_DateValueChanged(object sender, EventArgs e)
{
    DateTime selectedDate = scheduleControl1.Calendar.DateValue;
    Console.WriteLine($"Date selected: {selectedDate:yyyy-MM-dd}");
    
    // Update schedule view to selected date
    // (automatically handled by control)
}
```

## Navigation Panel

The Navigation Panel contains the Navigation Calendar and optional custom controls.

### Visibility and Docking

```csharp
// Access navigation panel
var navPanel = scheduleControl1.NavigationPanel;

// Control visibility
scheduleControl1.Appearance.ShowCaption = true; // Show/hide entire panel

// Navigation panel docking (left or right)
// Typically set through designer or at initialization
```

### Adding Custom Controls to Navigation Panel

```csharp
// Add custom control under the navigation calendar
Button customButton = new Button();
customButton.Text = "Today";
customButton.Dock = DockStyle.Top;
customButton.Click += (s, e) => {
    scheduleControl1.Calendar.DateValue = DateTime.Today;
};

scheduleControl1.AddControlToNavigationPanel(customButton);
```

## Caption Panel

The Caption Panel displays navigation buttons and a caption above the schedule.

### Caption Panel Configuration

```csharp
// Show/hide caption panel
scheduleControl1.Appearance.ShowCaption = true;

// Show/hide navigation buttons (forward/backward)
scheduleControl1.Appearance.ShowCaptionButtons = true;

// Customize caption background color
scheduleControl1.Appearance.CaptionBackColor = Color.FromArgb(0, 114, 198);
```

### Navigation Buttons

The caption panel includes two navigation buttons:
- **Backward Button:** Navigate to previous period (day, week, month)
- **Forward Button:** Navigate to next period

These buttons automatically adjust based on current `ScheduleType`:
- **Month view:** Move by month
- **Week/WorkWeek view:** Move by week
- **Day view:** Move by day

## Week Configuration

### First Day of Week

Configure which day appears in the leftmost column:

```csharp
// Navigation Calendar first day of week
scheduleControl1.Appearance.NavigationCalendarStartDayOfWeek = DayOfWeek.Monday;

// Month Calendar first day of week
scheduleControl1.Appearance.MonthCalendarStartDayOfWeek = DayOfWeek.Sunday;

// Week Calendar first day of week
scheduleControl1.Appearance.WeekCalendarStartDayOfWeek = DayOfWeek.Sunday;
```

### Week Number Display

```csharp
// Show week numbers in navigation calendar
scheduleControl1.Calendar.ShowWeekNumbers = true;

// Customize week number color
scheduleControl1.Appearance.NavigationCalendarWeekNumberColor = Color.Gray;
```

## View-Specific Display Options

### Month View

```csharp
// Show full 7-day week or stack weekend columns
scheduleControl1.Appearance.MonthShowFullWeek = true; // 7 columns
// false = 6 columns with Saturday/Sunday stacked
```

### Day/Week Views

```csharp
// Set time divisions per hour (2, 3, 4, or 6)
scheduleControl1.Appearance.DivisionsPerHour = 4; // 15-minute intervals
// 2 = 30 minutes, 3 = 20 minutes, 4 = 15 minutes, 6 = 10 minutes

// Maximum days side-by-side in Day view before switching to Week
scheduleControl1.Appearance.DayMonthCutOff = 3;
```

## Complete Navigation Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Schedule;

public class ScheduleNavigationManager
{
    private ScheduleControl scheduleControl;
    
    public ScheduleNavigationManager(ScheduleControl control)
    {
        scheduleControl = control;
        ConfigureNavigation();
    }
    
    private void ConfigureNavigation()
    {
        // Configure navigation calendar
        scheduleControl.Calendar.ShowWeekNumbers = true;
        scheduleControl.Calendar.DateValueChanged += OnDateChanged;
        
        // Set first day of week to Monday
        scheduleControl.Appearance.NavigationCalendarStartDayOfWeek = DayOfWeek.Monday;
        scheduleControl.Appearance.MonthCalendarStartDayOfWeek = DayOfWeek.Monday;
        
        // Configure caption
        scheduleControl.Appearance.ShowCaption = true;
        scheduleControl.Appearance.ShowCaptionButtons = true;
        
        // Add quick navigation buttons
        AddQuickNavigationButtons();
    }
    
    private void AddQuickNavigationButtons()
    {
        // Today button
        Button btnToday = new Button { Text = "Today", Dock = DockStyle.Top, Height = 30 };
        btnToday.Click += (s, e) => GoToToday();
        scheduleControl.AddControlToNavigationPanel(btnToday);
        
        // View selector
        ComboBox cboView = new ComboBox { 
            Dock = DockStyle.Top, 
            DropDownStyle = ComboBoxStyle.DropDownList,
            Height = 25
        };
        cboView.Items.AddRange(new object[] { "Day", "Week", "WorkWeek", "Month" });
        cboView.SelectedIndex = 3; // Month
        cboView.SelectedIndexChanged += (s, e) => ChangeView(cboView.SelectedItem.ToString());
        scheduleControl.AddControlToNavigationPanel(cboView);
    }
    
    private void OnDateChanged(object sender, EventArgs e)
    {
        DateTime selectedDate = scheduleControl.Calendar.DateValue;
        Console.WriteLine($"Navigated to: {selectedDate:MMMM dd, yyyy}");
    }
    
    public void GoToToday()
    {
        scheduleControl.Calendar.DateValue = DateTime.Today;
    }
    
    public void ChangeView(string viewName)
    {
        switch (viewName)
        {
            case "Day":
                scheduleControl.ScheduleType = ScheduleViewType.Day;
                break;
            case "Week":
                scheduleControl.ScheduleType = ScheduleViewType.Week;
                break;
            case "WorkWeek":
                scheduleControl.ScheduleType = ScheduleViewType.WorkWeek;
                break;
            case "Month":
                scheduleControl.ScheduleType = ScheduleViewType.Month;
                break;
        }
    }
}
```

## Best Practices

1. **Default to Month View:** Start with Month view for best overview, let users drill down to Day/Week
2. **Configure First Day:** Set first day of week based on regional standards (Monday for ISO, Sunday for US)
3. **Enable Week Numbers:** Helpful for business planning and cross-team coordination
4. **Provide View Shortcuts:** Add quick access buttons or keyboard shortcuts for view switching
5. **Handle DateValueChanged:** Use this event for custom logic when users navigate dates
6. **Match Time Divisions to Use Case:** Healthcare = 15 min, general business = 30 min, detailed tracking = 10 min
