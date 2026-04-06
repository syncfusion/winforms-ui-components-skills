# Recurrence Appointments

The Windows Forms Scheduler supports complex recurring appointment patterns including daily, weekly, monthly, yearly, and time-based recurrence (seconds, minutes, hours).

## Table of Contents
- [Overview](#overview)
- [Recurrence Rule Syntax](#recurrence-rule-syntax)
- [Rule Types](#rule-types)
- [Time-Based Recurrence](#time-based-recurrence)
- [Adding Recurrence via Dialog](#adding-recurrence-via-dialog)
- [Adding Recurrence Programmatically](#adding-recurrence-programmatically)
- [Recurrence Interfaces](#recurrence-interfaces)
- [Complete Examples](#complete-examples)

## Overview

Recurring appointments allow you to define patterns that repeat over time without creating individual appointment instances. The recurrence engine supports:

- **Date-based patterns:** Daily, Weekly, Monthly, Quarterly, Yearly
- **Time-based patterns:** Every N seconds, minutes, or hours
- **Flexible end dates:** Specific end date or indefinite
- **Complex rules:** Combinations of patterns with custom intervals

## Recurrence Rule Syntax

Recurrence rules are string-based with a standardized format:

```
{StartDate};{EndDate};{RecurrencePattern}[;{TimeInterval}]
```

**Components:**
- **StartDate:** First occurrence date (MM/DD/YYYY format)
- **EndDate:** Last occurrence date (MM/DD/YYYY format)
- **RecurrencePattern:** Rule type (Every DAY, Every WEEK, etc.)
- **TimeInterval:** Optional time-based interval (Every SEC, Every MIN, Every HR)

**Example:**
```
10/08/2015;10/15/2015;Every DAY 1
```
This creates daily appointments from October 8 to October 15, 2015.

## Rule Types

### Every DAY

Repeats every N days.

**Syntax:** `{StartDate};{EndDate};Every DAY {NumberOfDays}`

**Examples:**
```csharp
// Every day for a week
"03/24/2026;03/31/2026;Every DAY 1"

// Every 3 days for a month
"03/24/2026;04/24/2026;Every DAY 3"

// Every other day
"03/24/2026;06/24/2026;Every DAY 2"
```

### Every WEEKDAY

Repeats on all weekdays (Monday through Friday).

**Syntax:** `{StartDate};{EndDate};Every WEEKDAY`

**Examples:**
```csharp
// Weekdays for a month
"03/24/2026;04/24/2026;Every WEEKDAY"

// Weekdays for the year
"01/01/2026;12/31/2026;Every WEEKDAY"
```

### Every WEEKEND

Repeats on weekend days (Saturday and Sunday).

**Syntax:** `{StartDate};{EndDate};Every WEEKEND`

**Examples:**
```csharp
// Weekends for a month
"03/24/2026;04/24/2026;Every WEEKEND"

// Weekends for summer
"06/01/2026;08/31/2026;Every WEEKEND"
```

### Every WEEK

Repeats on specific days of the week.

**Syntax:** `{StartDate};{EndDate};Every WEEK on {DAY}[;Every WEEK on {DAY}]`

**Day Abbreviations:** SUN, MON, TUE, WED, THU, FRI, SAT

**Examples:**
```csharp
// Every Monday
"03/24/2026;06/24/2026;Every WEEK on MON"

// Every Monday and Wednesday
"03/24/2026;06/24/2026;Every WEEK on MON;Every WEEK on WED"

// Every Friday
"03/24/2026;12/31/2026;Every WEEK on FRI"

// Multiple days (Monday, Wednesday, Friday)
"03/24/2026;06/24/2026;Every WEEK on MON;Every WEEK on WED;Every WEEK on FRI"
```

### Every MONTH

Repeats monthly on a specific date or day of week.

**Syntax (by date):** `{StartDate};{EndDate};Every MONTH on {Date}`  
**Syntax (by day):** `{StartDate};{EndDate};Every MONTH on {Day}:{WhichWeek}`

**WhichWeek Values:** 1=First, 2=Second, 3=Third, 4=Fourth, 5=Last

**Examples:**
```csharp
// 20th of every month
"05/08/2026;10/08/2026;Every MONTH on 20"

// First Monday of every month
"05/08/2026;10/08/2026;Every MONTH on MON:1"

// Last Friday of every month
"03/24/2026;12/31/2026;Every MONTH on FRI:5"

// Third Wednesday of every month
"03/24/2026;12/31/2026;Every MONTH on WED:3"
```

### Every QUARTER

Repeats quarterly with month offset.

**Syntax (by date):** `{StartDate};{EndDate};Every QUARTER on {Date} after MONTH:{MonthDifference}`  
**Syntax (by day):** `{StartDate};{EndDate};Every QUARTER on {Day}:{Week} after MONTH:{MonthDifference}`

**Examples:**
```csharp
// 20th day, 1 month after quarter start
"10/13/2026;10/13/2027;Every QUARTER on 20 after MONTH:1"

// First Monday, 1 month after quarter start
"10/13/2026;10/13/2027;Every QUARTER on MON:1 after MONTH:1"

// 15th day at quarter start
"01/15/2026;12/15/2026;Every QUARTER on 15 after MONTH:0"
```

### Every YEAR

Repeats yearly on a specific date or day of week.

**Syntax (by date):** `{StartDate};{EndDate};Every YEAR on {Month} {Date}`  
**Syntax (by day):** `{StartDate};{EndDate};Every YEAR on {Day}:{Week} after {Month}`

**Month Abbreviations:** JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC

**Examples:**
```csharp
// January 20th every year
"10/13/2026;10/13/2029;Every YEAR on JAN 20"

// First Monday of January each year
"10/15/2026;10/15/2029;Every YEAR on MON:1 after JAN"

// Last Friday of December each year
"01/01/2026;12/31/2030;Every YEAR on FRI:5 after DEC"

// March 15th (useful for tax deadlines, etc.)
"03/15/2026;03/15/2030;Every YEAR on MAR 15"
```

## Time-Based Recurrence

Time-based recurrence allows appointments to repeat at intervals measured in seconds, minutes, or hours.

### Prerequisites

Enable seconds in appointment times:

```csharp
scheduleControl1.AllowSecondsInAppointment = true;
```

This property must be set to `true` to use time-based recurrence rules.

### Every SEC

Repeats every N seconds (minimum: 60 seconds).

**Syntax:** `{StartDate};{EndDate};{DatePattern};Every SEC {Seconds}`

**Examples:**
```csharp
// Every 120 seconds (2 minutes) on a specific day
"03/24/2026;03/24/2026;Every DAY 1;Every SEC 120"

// Every 60 seconds on weekdays
"03/24/2026;03/31/2026;Every WEEKDAY;Every SEC 60"

// Every 300 seconds (5 minutes) for a week
"03/24/2026;03/31/2026;Every DAY 1;Every SEC 300"
```

**Important:** Minimum value is 60 seconds. Values below 60 are automatically adjusted to 60.

### Every MIN

Repeats every N minutes.

**Syntax:** `{StartDate};{EndDate};{DatePattern};Every MIN {Minutes}`

**Examples:**
```csharp
// Every 10 minutes for a day
"03/24/2026;03/24/2026;Every DAY 1;Every MIN 10"

// Every 15 minutes on weekdays
"03/24/2026;03/28/2026;Every WEEKDAY;Every MIN 15"

// Every 30 minutes for a month
"03/01/2026;03/31/2026;Every DAY 1;Every MIN 30"

// Every 5 minutes for specific hours (requires time window logic)
"03/24/2026;03/24/2026;Every DAY 1;Every MIN 5"
```

### Every HR

Repeats every N hours.

**Syntax:** `{StartDate};{EndDate};{DatePattern};Every HR {Hours}`

**Examples:**
```csharp
// Every 2 hours for a day
"03/24/2026;03/24/2026;Every DAY 1;Every HR 2"

// Every 4 hours on weekdays
"03/24/2026;03/28/2026;Every WEEKDAY;Every HR 4"

// Every hour for a week
"03/24/2026;03/31/2026;Every DAY 1;Every HR 1"

// Every 3 hours for a month
"03/01/2026;03/31/2026;Every DAY 1;Every HR 3"
```

## Adding Recurrence via Dialog

The Appointment Recurrence dialog provides a UI for creating recurring appointments.

### Steps to Create Recurring Appointment

1. **Open Appointment Form:**
   - Double-click a time slot in the schedule
   - OR right-click → **New Item**

2. **Configure Basic Details:**
   - Enter **Subject** (required)
   - Uncheck **All Day Event** (required for time-based recurrence)
   - Set **Start Time** and **End Time**
   - Fill in **Location**, **Content**, etc. (optional)

3. **Enable Recurrence:**
   - Click **Make Recurring** button
   - Appointment Recurrence Dialog opens

4. **Configure Recurrence Pattern:**
   - Select recurrence type (Daily, Weekly, Monthly, Yearly)
   - Set interval (e.g., every 2 days, every 3 weeks)
   - For time-based: Select interval type (seconds, minutes, hours) and value
   - Set end date or no end date

5. **Save:**
   - Click **OK** on Recurrence Dialog
   - Click **Save and Close** on Appointment Form

### Recurrence Dialog Options

**Daily Options:**
- Every N days
- Every weekday
- Time interval (if AllowSecondsInAppointment = true)

**Weekly Options:**
- Select day(s) of week (checkboxes for SUN-SAT)
- Every N weeks

**Monthly Options:**
- Day N of every M months
- The [First/Second/Third/Fourth/Last] [Day] of every M months

**Yearly Options:**
- On [Month] [Day]
- On the [First/Second/Third/Fourth/Last] [Day] of [Month]

## Adding Recurrence Programmatically

### Using IRecurringScheduleAppointment

Cast the appointment to `IRecurringScheduleAppointment` and set the `RecurrenceRule` property:

```csharp
// Get data provider as recurring provider
IRecurringScheduleDataProvider recurringProvider = 
    scheduleControl1.DataSource as IRecurringScheduleDataProvider;

// Create new appointment
IScheduleAppointment app = recurringProvider.NewScheduleAppointment();
IRecurringScheduleAppointment recurringItem = app as IRecurringScheduleAppointment;

if (recurringItem != null)
{
    // Set base appointment details
    recurringItem.StartTime = new DateTime(2026, 03, 24, 9, 0, 0);
    recurringItem.EndTime = new DateTime(2026, 03, 24, 10, 0, 0);
    recurringItem.Subject = "Daily Standup Meeting";
    recurringItem.Content = "Team sync and status updates";
    recurringItem.LabelValue = 2; // Business
    
    // Set recurrence rule (every weekday for 3 months)
    recurringItem.RecurrenceRule = "03/24/2026;06/24/2026;Every WEEKDAY";
    
    // Add to data provider with end date
    recurringProvider.AddNewRecurringAppointments(recurringItem, new DateTime(2026, 12, 31));
}
```

### Complex Recurrence Examples

**Weekly team meeting (every Monday):**
```csharp
recurringItem.StartTime = new DateTime(2026, 03, 24, 14, 0, 0);
recurringItem.EndTime = new DateTime(2026, 03, 24, 15, 0, 0);
recurringItem.Subject = "Weekly Team Meeting";
recurringItem.RecurrenceRule = "03/24/2026;12/31/2026;Every WEEK on MON";
```

**Bi-weekly review (every 2 weeks on Friday):**
```csharp
recurringItem.StartTime = new DateTime(2026, 03, 27, 16, 0, 0);
recurringItem.EndTime = new DateTime(2026, 03, 27, 17, 0, 0);
recurringItem.Subject = "Bi-weekly Sprint Review";
recurringItem.RecurrenceRule = "03/27/2026;12/31/2026;Every WEEK on FRI"; // Note: interval handled differently
```

**Monthly all-hands (first Monday of month):**
```csharp
recurringItem.StartTime = new DateTime(2026, 04, 06, 10, 0, 0); // First Monday in April
recurringItem.EndTime = new DateTime(2026, 04, 06, 11, 0, 0);
recurringItem.Subject = "Monthly All-Hands";
recurringItem.RecurrenceRule = "04/06/2026;12/31/2026;Every MONTH on MON:1";
```

**Hourly status check:**
```csharp
scheduleControl1.AllowSecondsInAppointment = true; // Required

recurringItem.StartTime = new DateTime(2026, 03, 24, 8, 0, 0);
recurringItem.EndTime = new DateTime(2026, 03, 24, 8, 5, 0);
recurringItem.Subject = "System Status Check";
recurringItem.RecurrenceRule = "03/24/2026;03/24/2026;Every DAY 1;Every HR 1";
```

**15-minute intervals for a specific day:**
```csharp
scheduleControl1.AllowSecondsInAppointment = true;

recurringItem.StartTime = new DateTime(2026, 03, 24, 9, 0, 0);
recurringItem.EndTime = new DateTime(2026, 03, 24, 9, 10, 0);
recurringItem.Subject = "Patient Appointment Slot";
recurringItem.RecurrenceRule = "03/24/2026;03/24/2026;Every DAY 1;Every MIN 15";
```

## Recurrence Interfaces

### IRecurringScheduleAppointment

Extends `IScheduleAppointment` with recurrence support:

```csharp
public interface IRecurringScheduleAppointment : IScheduleAppointment
{
    string RecurrenceRule { get; set; }
    // ... inherits all IScheduleAppointment properties
}
```

**Key Property:**
- **RecurrenceRule:** String defining the recurrence pattern

### IRecurringScheduleDataProvider

Extends `IScheduleDataProvider` with recurring appointment management:

```csharp
public interface IRecurringScheduleDataProvider : IScheduleDataProvider
{
    void AddNewRecurringAppointments(IRecurringScheduleAppointment item, DateTime endDate);
    // ... inherits all IScheduleDataProvider methods
}
```

**Key Method:**
- **AddNewRecurringAppointments(item, endDate):** Adds recurring appointment with specified end date

## Complete Examples

### Daily Recurring Meeting

```csharp
public void CreateDailyMeeting(ScheduleControl scheduleControl)
{
    IRecurringScheduleDataProvider dataProvider = 
        scheduleControl.DataSource as IRecurringScheduleDataProvider;
    
    IScheduleAppointment app = dataProvider.NewScheduleAppointment();
    IRecurringScheduleAppointment meeting = app as IRecurringScheduleAppointment;
    
    if (meeting != null)
    {
        meeting.StartTime = new DateTime(2026, 03, 24, 9, 0, 0);
        meeting.EndTime = new DateTime(2026, 03, 24, 9, 30, 0);
        meeting.Subject = "Daily Standup";
        meeting.Content = "Team sync meeting";
        meeting.LabelValue = 2; // Business
        meeting.MarkerValue = 2; // Busy
        meeting.LocationValue = 1; // Room B
        
        // Every day for 3 months
        meeting.RecurrenceRule = "03/24/2026;06/24/2026;Every DAY 1";
        
        dataProvider.AddNewRecurringAppointments(meeting, new DateTime(2026, 12, 31));
    }
}
```

### Weekly Training Sessions

```csharp
public void CreateWeeklyTraining(ScheduleControl scheduleControl)
{
    IRecurringScheduleDataProvider dataProvider = 
        scheduleControl.DataSource as IRecurringScheduleDataProvider;
    
    IScheduleAppointment app = dataProvider.NewScheduleAppointment();
    IRecurringScheduleAppointment training = app as IRecurringScheduleAppointment;
    
    if (training != null)
    {
        training.StartTime = new DateTime(2026, 03, 26, 14, 0, 0); // Thursday
        training.EndTime = new DateTime(2026, 03, 26, 16, 0, 0);
        training.Subject = "Technical Training";
        training.Content = "Weekly skill development session";
        training.LabelValue = 7; // Needs Preparation
        training.ForeColor = Color.Purple;
        
        // Every Thursday
        training.RecurrenceRule = "03/26/2026;12/31/2026;Every WEEK on THU";
        
        dataProvider.AddNewRecurringAppointments(training, new DateTime(2027, 12, 31));
    }
}
```

### Minute-Based Appointment Slots

```csharp
public void CreateAppointmentSlots(ScheduleControl scheduleControl)
{
    // Enable seconds/minutes precision
    scheduleControl.AllowSecondsInAppointment = true;
    
    IRecurringScheduleDataProvider dataProvider = 
        scheduleControl.DataSource as IRecurringScheduleDataProvider;
    
    IScheduleAppointment app = dataProvider.NewScheduleAppointment();
    IRecurringScheduleAppointment slot = app as IRecurringScheduleAppointment;
    
    if (slot != null)
    {
        slot.StartTime = new DateTime(2026, 03, 24, 9, 0, 0);
        slot.EndTime = new DateTime(2026, 03, 24, 9, 10, 0); // 10-minute slots
        slot.Subject = "Available Slot";
        slot.LabelValue = 0; // None
        slot.MarkerValue = 0; // Free
        
        // Every 15 minutes from 9 AM to 5 PM
        slot.RecurrenceRule = "03/24/2026;03/24/2026;Every DAY 1;Every MIN 15";
        
        dataProvider.AddNewRecurringAppointments(slot, new DateTime(2026, 03, 24, 17, 0, 0));
    }
}
```

### Monthly Board Meeting

```csharp
public void CreateMonthlyBoardMeeting(ScheduleControl scheduleControl)
{
    IRecurringScheduleDataProvider dataProvider = 
        scheduleControl.DataSource as IRecurringScheduleDataProvider;
    
    IScheduleAppointment app = dataProvider.NewScheduleAppointment();
    IRecurringScheduleAppointment boardMeeting = app as IRecurringScheduleAppointment;
    
    if (boardMeeting != null)
    {
        boardMeeting.StartTime = new DateTime(2026, 04, 01, 10, 0, 0); // First Wed in April
        boardMeeting.EndTime = new DateTime(2026, 04, 01, 12, 0, 0);
        boardMeeting.Subject = "Board Meeting";
        boardMeeting.Content = "Monthly board of directors meeting";
        boardMeeting.LabelValue = 5; // Must Attend
        boardMeeting.MarkerValue = 3; // Out of Office
        boardMeeting.LocationValue = 3; // Board Room
        boardMeeting.ForeColor = Color.DarkRed;
        
        // First Wednesday of every month
        boardMeeting.RecurrenceRule = "04/01/2026;12/31/2026;Every MONTH on WED:1";
        
        dataProvider.AddNewRecurringAppointments(boardMeeting, new DateTime(2027, 12, 31));
    }
}
```

## Best Practices

1. **Always Set AllowSecondsInAppointment:** Required for time-based recurrence (Every SEC, Every MIN, Every HR)
2. **Minimum Time Interval:** The minimum for `Every SEC` is 60 seconds; lower values default to 60
3. **End Dates:** Always specify reasonable end dates to prevent infinite appointment generation
4. **Test Recurrence Rules:** Verify rules generate expected appointments before deploying to production
5. **Performance Consideration:** Large numbers of recurring appointments with short intervals can impact performance
6. **UI Feedback:** Provide clear feedback when users create recurrence rules that generate many appointments
7. **Documentation:** Document custom recurrence patterns for maintenance and support teams
