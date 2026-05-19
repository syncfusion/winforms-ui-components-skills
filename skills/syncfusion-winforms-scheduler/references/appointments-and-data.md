# Appointments and Data Management

This guide covers the data architecture, appointment classes, and data provider implementation for the Windows Forms Scheduler.

## Table of Contents
- [ScheduleAppointment Class](#scheduleappointment-class)
- [ScheduleAppointmentList Class](#scheduleappointmentlist-class)
- [ScheduleDataProvider Class](#scheduledataprovider-class)
- [Data Interfaces](#data-interfaces)
- [Implementing Custom Data Providers](#implementing-custom-data-providers)
- [Drop List Data](#drop-list-data)
- [Complete Implementation Example](#complete-implementation-example)

## ScheduleAppointment Class

The `ScheduleAppointment` class represents individual appointments and implements `IScheduleAppointment`.

### Core Properties

**Time and Duration:**
```csharp
// Start and end times
appointment.StartTime = new DateTime(2026, 03, 24, 9, 0, 0);
appointment.EndTime = new DateTime(2026, 03, 24, 10, 30, 0);

// All-day appointment flag
appointment.AllDay = true; // No specific start/end time
```

**Identification:**
```csharp
// Unique identifier (auto-generated or custom)
appointment.UniqueID = 12345;

// Owner identifier for multi-user scenarios
appointment.Owner = 1; // User ID or resource ID
```

**Content:**
```csharp
// Appointment title
appointment.Subject = "Team Meeting";

// Detailed description/notes
appointment.Content = "Discuss Q2 roadmap and resource allocation";
```

**Categorization:**
```csharp
// Label/category (0-10, references label list)
appointment.LabelValue = 2; // Business category
// 0=None, 1=Important, 2=Business, 3=Personal, etc.

// Status marker (0-3, references marker list)
appointment.MarkerValue = 2; // Busy status
// 0=Free, 1=Tentative, 2=Busy, 3=Out of Office

// Location identifier (references location list)
appointment.LocationValue = "RoomB"; // "RoomB"
```

**Reminders:**
```csharp
// Enable reminder
appointment.Reminder = true;

// Reminder time interval (0-8, references reminder list)
appointment.ReminderValue = 3; // 15 minutes before
// 0=0 min, 1=5 min, 2=10 min, 3=15 min, 4=30 min, 5=1 hour, etc.
```

**Appearance:**
```csharp
// Text color
appointment.ForeColor = Color.Red;
```

**State Management:**
```csharp
// Version number (for data format versioning)
int version = appointment.Version;

// Dirty flag (indicates modification)
bool modified = appointment.Dirty;

// Control whether changes affect dirty flag
appointment.IgnoreChanges = true; // Suppress dirty tracking
```

**Custom Data:**
```csharp
// Arbitrary object storage
appointment.Tag = new { CustomerId = 42, Priority = "High" };
```

### All ScheduleAppointment Properties

| Property | Type | Description |
|----------|------|-------------|
| **UniqueID** | int | Unique integer identifier |
| **Owner** | int | Owner/resource identifier |
| **StartTime** | DateTime | Appointment start date/time |
| **EndTime** | DateTime | Appointment end date/time |
| **Subject** | string | Appointment title |
| **Content** | string | Detailed description |
| **AllDay** | bool | All-day appointment flag |
| **LabelValue** | int | Category/label identifier (0-10) |
| **MarkerValue** | int | Status marker (0-3) |
| **Reminder** | bool | Enable reminder notification |
| **ReminderValue** | int | Reminder interval identifier (0-8) |
|**ReminderText** | string |  Custom reminder display text shown in the UI |
| **LocationValue** | string  | Location identifier |
| **ForeColor** | Color | Appointment text color |
| **Version** | int | Data format version number |
| **Dirty** | bool | Modification flag |
| **IgnoreChanges** | bool | Suppress dirty tracking |
| **Tag** | object | Custom data storage |

## ScheduleAppointmentList Class

The `ScheduleAppointmentList` is a collection of `IScheduleAppointment` objects, implementing `IComparer` to order by `StartTime`.

### Creating and Populating Lists

```csharp
// Create empty list
ScheduleAppointmentList list = new ScheduleAppointmentList();

// Create new appointment
IScheduleAppointment appointment = list.NewScheduleAppointment();
appointment.StartTime = DateTime.Now;
appointment.EndTime = DateTime.Now.AddHours(1);
appointment.Subject = "Meeting";

// Add to list
list.Add(appointment);
```

### Collection Operations

**Accessing Items:**
```csharp
// Get count
int count = list.Count;

// Access by index
IScheduleAppointment firstAppointment = list[0];

// Iterate
foreach (IScheduleAppointment appt in list)
{
    Console.WriteLine($"{appt.StartTime}: {appt.Subject}");
}
```

**Adding and Inserting:**
```csharp
// Add to end
list.Add(appointment);

// Insert at specific position
list.Insert(0, appointment); // Insert at beginning
```

**Removing:**
```csharp
// Remove specific item
list.Remove(appointment);

// Remove by index
list.RemoveAt(2);

// Find index
int index = list.IndexOf(appointment);
```

**Sorting:**
```csharp
// Sort by StartTime (longer appointments rank higher if same start time)
list.SortStartTime();
```

### ScheduleAppointmentList Methods

| Method | Description |
|--------|-------------|
| **this[int i]** | Gets/sets the i-th appointment |
| **Count** | Gets number of appointments |
| **Add(IScheduleAppointment)** | Adds appointment to list |
| **Insert(int, IScheduleAppointment)** | Inserts appointment at position |
| **Remove(IScheduleAppointment)** | Removes appointment from list |
| **RemoveAt(int)** | Removes appointment at index |
| **IndexOf(IScheduleAppointment)** | Returns position of appointment |
| **SortStartTime()** | Sorts list by start time |
| **NewScheduleAppointment()** | Creates new appointment with defaults |

## ScheduleDataProvider Class

The `ScheduleDataProvider` is an abstract base class implementing `IScheduleDataProvider`. It provides:

1. **Virtual methods** for data operations (must override in derived class)
2. **Concrete implementations** for drop list data (can override for customization)

### Required Virtual Method Overrides

When deriving from `ScheduleDataProvider`, override these methods:

```csharp
public class CustomScheduleDataProvider : ScheduleDataProvider
{
    // Get appointments for a specific day
    public override IScheduleAppointmentList GetScheduleForDay(DateTime day)
    {
        // Return appointments for 'day'
    }
    
    // Get appointments in date range
    public override IScheduleAppointmentList GetSchedule(DateTime startDate, DateTime endDate)
    {
        // Return appointments between startDate and endDate
    }
    
    // Get appointments for specific owner/resource on a day
    public override IScheduleAppointmentList GetScheduleForDay(DateTime day, int owner)
    {
        // Return appointments for 'day' belonging to 'owner'
    }
    
    // Get appointments for specific owner in date range
    public override IScheduleAppointmentList GetSchedule(DateTime startDate, DateTime endDate, int owner)
    {
        // Return appointments between dates for 'owner'
    }
    
    // Save changes to persistent storage
    public override void CommitChanges()
    {
        // Save modified appointments
    }
    
    // Create new appointment with default values
    public override IScheduleAppointment NewScheduleAppointment()
    {
        return new ScheduleAppointment();
    }
    
    // Add appointment to data store
    public override void AddItem(IScheduleAppointment item)
    {
        // Add item to collection
    }
    
    // Remove appointment from data store
    public override void RemoveItem(IScheduleAppointment item)
    {
        // Remove item from collection
    }
    
    // Check if data has been modified
    public override bool IsDirty
    {
        get { /* return modification status */ }
        set { /* set modification status */ }
    }
}
```

### SaveOnCloseBehavior Property

Controls save behavior when the ScheduleControl is disposed:

```csharp
dataProvider.SaveOnCloseBehaviorAction = SaveOnCloseBehavior.PromptToSave;
```

**Options:**
- **PromptToSave:** Show dialog asking user to save
- **Save:** Automatically save without prompting
- **DontSave:** Discard changes without saving

## Data Interfaces

### IScheduleAppointment

Defines the appointment data contract:

```csharp
public interface IScheduleAppointment
{
    int UniqueID { get; set; }
    int Owner { get; set; }
    DateTime StartTime { get; set; }
    DateTime EndTime { get; set; }
    string Subject { get; set; }
    string Content { get; set; }
    bool AllDay { get; set; }
    int LabelValue { get; set; }
    int MarkerValue { get; set; }
    bool Reminder { get; set; }
    int ReminderValue { get; set; }
    int LocationValue { get; set; }
    Color ForeColor { get; set; }
    int Version { get; }
    bool Dirty { get; set; }
    bool IgnoreChanges { get; set; }
    object Tag { get; set; }
}
```

### IScheduleAppointmentList

Defines the appointment collection contract:

```csharp
public interface IScheduleAppointmentList
{
    IScheduleAppointment this[int index] { get; set; }
    int Count { get; }
    void Add(IScheduleAppointment item);
    void Insert(int index, IScheduleAppointment item);
    void Remove(IScheduleAppointment item);
    void RemoveAt(int index);
    int IndexOf(IScheduleAppointment item);
    void SortStartTime();
    IScheduleAppointment NewScheduleAppointment();
}
```

### IScheduleDataProvider

Defines the data provider contract:

```csharp
public interface IScheduleDataProvider
{
    IScheduleAppointmentList GetScheduleForDay(DateTime day);
    IScheduleAppointmentList GetSchedule(DateTime startDate, DateTime endDate);
    IScheduleAppointmentList GetScheduleForDay(DateTime day, int owner);
    IScheduleAppointmentList GetSchedule(DateTime startDate, DateTime endDate, int owner);
    void CommitChanges();
    bool IsDirty { get; set; }
    IScheduleAppointment NewScheduleAppointment();
    void AddItem(IScheduleAppointment item);
    void RemoveItem(IScheduleAppointment item);
    ILookUpObjectList GetLabels();
    ILookUpObjectList GetMarkers();
    ILookUpObjectList GetReminders();
    ILookUpObjectList GetLocations();
    ILookUpObjectList GetOwners();
}
```

## Implementing Custom Data Providers

For database or custom storage integration:

```csharp
public class DatabaseScheduleDataProvider : ScheduleDataProvider
{
    private ScheduleAppointmentList masterList;
    private string connectionString;
    
    public DatabaseScheduleDataProvider(string connString)
    {
        connectionString = connString;
        masterList = new ScheduleAppointmentList();
        LoadFromDatabase();
        InitLists(); // Initialize drop lists
    }
    
    private void LoadFromDatabase()
    {
        // Load appointments from database
        // Populate masterList
    }
    
    public override IScheduleAppointmentList GetSchedule(DateTime startDate, DateTime endDate)
    {
        ScheduleAppointmentList result = new ScheduleAppointmentList();
        
        foreach (IScheduleAppointment appt in masterList)
        {
            if (appt.StartTime >= startDate && appt.StartTime < endDate)
            {
                result.Add(appt);
            }
        }
        
        result.SortStartTime();
        return result;
    }
    
    public override IScheduleAppointmentList GetScheduleForDay(DateTime day)
    {
        DateTime startOfDay = day.Date;
        DateTime endOfDay = startOfDay.AddDays(1);
        return GetSchedule(startOfDay, endOfDay);
    }
    
    public override void CommitChanges()
    {
        // Save modified appointments to database
        foreach (IScheduleAppointment appt in masterList)
        {
            if (appt.Dirty)
            {
                SaveToDatabase(appt);
                appt.Dirty = false;
            }
        }
        
        IsDirty = false;
    }
    
    public override void AddItem(IScheduleAppointment item)
    {
        masterList.Add(item);
        item.Dirty = true;
        IsDirty = true;
    }
    
    public override void RemoveItem(IScheduleAppointment item)
    {
        masterList.Remove(item);
        DeleteFromDatabase(item);
        IsDirty = true;
    }
    
    public override IScheduleAppointment NewScheduleAppointment()
    {
        return new ScheduleAppointment
        {
            UniqueID = GenerateUniqueID(),
            StartTime = DateTime.Now,
            EndTime = DateTime.Now.AddHours(1)
        };
    }
    
    private void SaveToDatabase(IScheduleAppointment appt)
    {
        // SQL INSERT or UPDATE
    }
    
    private void DeleteFromDatabase(IScheduleAppointment appt)
    {
        // SQL DELETE
    }
    
    private int GenerateUniqueID()
    {
        // Generate unique ID (e.g., from database sequence)
        return masterList.Count > 0 ? masterList[masterList.Count - 1].UniqueID + 1 : 1;
    }
}
```

## Drop List Data

The `ScheduleDataProvider` provides default drop lists for appointment properties. Override `InitLists()` to customize.

### Default Label List

```csharp
// Default labels (LabelValue 0-10)
labelList = new ListObjectList();
labelList.Add(new ListObject(0, "None", Color.White));
labelList.Add(new ListObject(1, "Important", Color.FromArgb(255, 128, 64)));
labelList.Add(new ListObject(2, "Business", Color.FromArgb(86, 152, 233)));
labelList.Add(new ListObject(3, "Personal", Color.FromArgb(57, 210, 53)));
labelList.Add(new ListObject(4, "Vacation", Color.FromArgb(199, 198, 182)));
labelList.Add(new ListObject(5, "Must Attend", Color.FromArgb(255, 128, 0)));
labelList.Add(new ListObject(6, "Travel Required", Color.FromArgb(0, 255, 255)));
labelList.Add(new ListObject(7, "Needs Preparation", Color.FromArgb(171, 171, 88)));
labelList.Add(new ListObject(8, "Birthday", Color.FromArgb(186, 117, 255)));
labelList.Add(new ListObject(9, "Anniversary", Color.FromArgb(255, 128, 64)));
labelList.Add(new ListObject(10, "Phone Call", Color.FromArgb(255, 128, 64)));
```

### Default Marker List

```csharp
// Default status markers (MarkerValue 0-3)
markerList = new ListObjectList();
markerList.Add(new ListObject(0, "Free", Color.FromArgb(50, Color.RoyalBlue)));
markerList.Add(new ListObject(1, "Tentative", Color.FromArgb(255, 206, 206)));
markerList.Add(new ListObject(2, "Busy", Color.FromArgb(0, 0, 242)));
markerList.Add(new ListObject(3, "Out of Office", Color.FromArgb(128, 0, 64)));
```

### Default Reminder List

```csharp
// Default reminder intervals (ReminderValue 0-8)
reminderList = new ListObjectList();
reminderList.Add(new ListObject(0, "0 minutes", Color.White));
reminderList.Add(new ListObject(1, "5 minutes", Color.White));
reminderList.Add(new ListObject(2, "10 minutes", Color.White));
reminderList.Add(new ListObject(3, "15 minutes", Color.White));
reminderList.Add(new ListObject(4, "30 minutes", Color.White));
reminderList.Add(new ListObject(5, "1 hour", Color.White));
reminderList.Add(new ListObject(6, "2 hours", Color.White));
reminderList.Add(new ListObject(7, "3 hours", Color.White));
reminderList.Add(new ListObject(8, "4 hours", Color.White));
```

### Default Location List

```csharp
// Default locations (LocationValue 0-4)
locationList = new ListObjectList();
locationList.Add(new ListObject(0, "", Color.White)); // No location
locationList.Add(new ListObject(1, "RoomB", Color.White));
locationList.Add(new ListObject(2, "RoomC", Color.White));
locationList.Add(new ListObject(3, "RoomD", Color.White));
locationList.Add(new ListObject(4, "RoomE", Color.White));
```

### Customizing Drop Lists

```csharp
public class CustomDataProvider : ScheduleDataProvider
{
    public override void InitLists()
    {
        // Call base to initialize with defaults
        base.InitLists();
        
        // Customize locations
        locationList = new ListObjectList();
        locationList.Add(new ListObject(0, "", Color.White));
        locationList.Add(new ListObject(1, "Conference Room A", Color.White));
        locationList.Add(new ListObject(2, "Conference Room B", Color.White));
        locationList.Add(new ListObject(3, "Board Room", Color.White));
        locationList.Add(new ListObject(4, "Training Room", Color.White));
        locationList.Add(new ListObject(5, "Off-site", Color.White));
        
        // Customize labels for specific domain
        labelList = new ListObjectList();
        labelList.Add(new ListObject(0, "None", Color.White));
        labelList.Add(new ListObject(1, "Client Meeting", Color.LightBlue));
        labelList.Add(new ListObject(2, "Internal Review", Color.LightGreen));
        labelList.Add(new ListObject(3, "Training", Color.Yellow));
        labelList.Add(new ListObject(4, "Project Work", Color.Orange));
    }
}
```

## Using ArrayListDataProvider

The built-in `ArrayListDataProvider` provides all data provider functionality without requiring custom implementation.

**Quick Start:**

```csharp
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;

// Create data provider
ArrayListDataProvider dataProvider = new ArrayListDataProvider();
dataProvider.MasterList = new ArrayListAppointmentList();
dataProvider.FileName = "schedule.xml";

// Create and add appointment
IScheduleAppointment appointment = dataProvider.NewScheduleAppointment();
appointment.StartTime = DateTime.Today.AddHours(9);
appointment.EndTime = DateTime.Today.AddHours(10);
appointment.Subject = "Team Meeting";
appointment.LabelValue = 2;

// Add to provider
dataProvider.AddItem(appointment);

// Bind to control
scheduleControl1.DataSource = dataProvider;

// Save to XML
dataProvider.SaveXML(dataProvider.FileName);
dataProvider.IsDirty = false;
```

**Key Benefits:**
- ✅ Built-in implementation - no custom code needed
- ✅ Integrated XML serialization with SaveXML/LoadXML methods
- ✅ File state tracking with FileName and IsDirty properties
- ✅ Production ready and maintained by Syncfusion

📄 **Read:** [data-provider-xml-implementation.md](data-provider-xml-implementation.md) for complete examples and patterns.
