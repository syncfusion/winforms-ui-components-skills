# Data Provider Implementation with XML Persistence

## Using ArrayListDataProvider - Simple Built-in Approach

Syncfusion ScheduleControl provides a built-in `ArrayListDataProvider` class for managing appointments without requiring custom implementation. This is the simplest and most straightforward approach for most applications.

**Important API Notes:**
- **`ArrayListDataProvider`** - Built-in data provider from `Syncfusion.Schedule` namespace with integrated XML serialization
- **`SaveXML(string fileName)`** - Instance method on `ArrayListDataProvider` to save appointments to XML
- **`LoadXML(string fileName)`** - Static method that returns an `ArrayListDataProvider` with loaded appointments
- **`FileName`** - Property to track the current file name for the data provider
- **`IsDirty`** - Property to track if data has been modified since last save

### Quick Start with ArrayListDataProvider

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;

public partial class Form1 : Form
{
    private ScheduleControl scheduleControl1;
    private ArrayListDataProvider dataProvider;
    
    public Form1()
    {
        InitializeComponent();
        InitializeSchedule();
    }
    
    private void InitializeSchedule()
    {
        // Create schedule control
        scheduleControl1 = new ScheduleControl();
        scheduleControl1.Dock = DockStyle.Fill;
        
        // Initialize built-in ArrayListDataProvider
        dataProvider = new ArrayListDataProvider();
        
        // Create and add appointments
        IScheduleAppointment appointment = dataProvider.NewScheduleAppointment();
        appointment.StartTime = new DateTime(2024, 02, 12, 12, 0, 0);
        appointment.EndTime = new DateTime(2024, 02, 19, 12, 0, 0);
        appointment.AllDay = true;
        appointment.Subject = "Business Trip";
        appointment.LabelValue = 1;
        
        IScheduleAppointment appointment2 = dataProvider.NewScheduleAppointment();
        appointment2.StartTime = new DateTime(2024, 02, 19, 12, 0, 0);
        appointment2.EndTime = new DateTime(2024, 02, 21, 12, 0, 0);
        appointment2.AllDay = true;
        appointment2.Subject = "Conference";
        appointment2.LabelValue = 2;
        
        // Add appointments to provider
        dataProvider.AddItem(appointment);
        dataProvider.AddItem(appointment2);
        
        // Set data source
        scheduleControl1.DataSource = dataProvider;
        
        this.Controls.Add(scheduleControl1);
    }
}
```

## Usage Examples

### Complete Working Example with Save/Load

Here's a complete example showing data persistence with `ArrayListDataProvider`:

```csharp
using System;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;

public class ScheduleForm : Form
{
    private ScheduleControl scheduleControl1;
    private ArrayListDataProvider dataProvider;
    
    public ScheduleForm()
    {
        InitializeSchedule();
    }
    
    private void InitializeSchedule()
    {
        scheduleControl1 = new ScheduleControl();
        scheduleControl1.Dock = DockStyle.Fill;
        
        // Initialize ArrayListDataProvider
        dataProvider = new ArrayListDataProvider();
        
        // Add sample appointments
        AddSampleAppointments();
        
        // Set data source
        scheduleControl1.DataSource = dataProvider;
        
        this.Controls.Add(scheduleControl1);
    }
    
    private void AddSampleAppointments()
    {
        // Create first appointment
        IScheduleAppointment appt1 = dataProvider.NewScheduleAppointment();
        appt1.StartTime = DateTime.Today.AddHours(9);
        appt1.EndTime = DateTime.Today.AddHours(10);
        appt1.Subject = "Team Meeting";
        appt1.Content = "Weekly standup";
        appt1.LabelValue = 1;
        
        // Create second appointment
        IScheduleAppointment appt2 = dataProvider.NewScheduleAppointment();
        appt2.StartTime = DateTime.Today.AddDays(1).AddHours(14);
        appt2.EndTime = DateTime.Today.AddDays(1).AddHours(15);
        appt2.Subject = "Client Call";
        appt2.Content = "Project review";
        appt2.LabelValue = 2;
        
        // Add to provider
        dataProvider.AddItem(appt1);
        dataProvider.AddItem(appt2);
    }
    
    // Save schedule to XML
    private void SaveSchedule(string fileName)
    {
        try
        {
            // Save using ArrayListDataProvider's SaveXML method
            dataProvider.SaveXML(fileName);
            dataProvider.FileName = fileName;
            dataProvider.IsDirty = false;
            MessageBox.Show("Schedule saved successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Failed to save: {ex.Message}", "Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
    
    // Load schedule from XML
    private void LoadSchedule(string fileName)
    {
        if (!File.Exists(fileName))
        {
            MessageBox.Show("File not found!", "Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
            return;
        }
        
        try
        {
            // Load using ArrayListDataProvider's static LoadXML method
            dataProvider = ArrayListDataProvider.LoadXML(fileName);
            dataProvider.FileName = fileName;
            dataProvider.IsDirty = false;
            
            // Set data source
            scheduleControl1.DataSource = dataProvider;
            scheduleControl1.Refresh();
            
            MessageBox.Show("Schedule loaded successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Failed to load: {ex.Message}", "Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
```

### Saving Schedule Data to XML

```csharp
using Syncfusion.Schedule;

// Save appointments to XML file using ArrayListDataProvider
ArrayListDataProvider dataProvider = scheduleControl1.DataSource as ArrayListDataProvider;
string fileName = "schedule.xml";

try
{
    dataProvider.SaveXML(fileName);
    dataProvider.FileName = fileName;
    dataProvider.IsDirty = false;
    MessageBox.Show("Schedule saved successfully!");
}
catch (Exception ex)
{
    MessageBox.Show($"Failed to save: {ex.Message}");
}
```

### Loading Schedule Data from XML

```csharp
using Syncfusion.Schedule;

// Load appointments from XML file using ArrayListDataProvider
string fileName = "schedule.xml";

if (File.Exists(fileName))
{
    try
    {
        ArrayListDataProvider dataProvider = ArrayListDataProvider.LoadXML(fileName);
        dataProvider.FileName = fileName;
        dataProvider.IsDirty = false;
        
        scheduleControl1.DataSource = dataProvider;
        scheduleControl1.Refresh();
        MessageBox.Show("Schedule loaded successfully!");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Failed to load: {ex.Message}");
    }
}
```

### Complete Save/Load with ArrayListDataProvider

```csharp
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;
using System.IO;

public partial class ScheduleForm : Form
{
    private ArrayListDataProvider dataProvider;
    
    private void Form1_Load(object sender, EventArgs e)
    {
        string fileName = "default.xml";
        
        // Load existing file or create new provider
        if (File.Exists(fileName))
        {
            // Load from XML
            dataProvider = ArrayListDataProvider.LoadXML(fileName);
            dataProvider.FileName = fileName;
            dataProvider.IsDirty = false;
        }
        else
        {
            // Create new provider
            dataProvider = new ArrayListDataProvider();
            dataProvider.MasterList = new ArrayListAppointmentList();
            dataProvider.FileName = fileName;
            
            // Add sample appointments
            IScheduleAppointment appointment = dataProvider.NewScheduleAppointment();
            appointment.StartTime = new DateTime(2024, 02, 12, 12, 0, 0);
            appointment.EndTime = new DateTime(2024, 02, 19, 12, 0, 0);
            appointment.AllDay = true;
            appointment.Subject = "Business Trip";
            appointment.LabelValue = 1;
            
            IScheduleAppointment appointment2 = dataProvider.NewScheduleAppointment();
            appointment2.StartTime = new DateTime(2024, 02, 19, 12, 0, 0);
            appointment2.EndTime = new DateTime(2024, 02, 21, 12, 0, 0);
            appointment2.AllDay = true;
            appointment2.Subject = "Conference";
            appointment2.LabelValue = 2;
            
            dataProvider.AddItem(appointment);
            dataProvider.AddItem(appointment2);
        }
        
        // Set data source
        scheduleControl1.DataSource = dataProvider;
    }
    
    // Save button click event
    private void btnSave_Click(object sender, EventArgs e)
    {
        try
        {
            dataProvider.SaveXML(dataProvider.FileName);
            dataProvider.IsDirty = false;
            MessageBox.Show("Schedule saved!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Save failed: {ex.Message}");
        }
    }
    
    // Load button click event
    private void btnLoad_Click(object sender, EventArgs e)
    {
        string fileName = "schedule.xml";
        if (File.Exists(fileName))
        {
            try
            {
                dataProvider = ArrayListDataProvider.LoadXML(fileName);
                dataProvider.FileName = fileName;
                dataProvider.IsDirty = false;
                
                // Update data source
                scheduleControl1.DataSource = dataProvider;
                scheduleControl1.Refresh();
                
                MessageBox.Show("Schedule loaded!");
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Load failed: {ex.Message}");
            }
        }
    }
}
```

### Adding Appointments Programmatically

```csharp
// Get the data provider
ArrayListDataProvider dataProvider = scheduleControl1.DataSource as ArrayListDataProvider;

// Create new appointment
IScheduleAppointment appt = dataProvider.NewScheduleAppointment();
appt.StartTime = DateTime.Today.AddHours(9);
appt.EndTime = DateTime.Today.AddHours(10);
appt.Subject = "Team Meeting";
appt.Content = "Weekly standup";
appt.LabelValue = 2;
appt.MarkerValue = 2;

// Add to data provider
dataProvider.AddItem(appt);

// Refresh the schedule to show the new appointment
scheduleControl1.Refresh();

// Optionally save to XML
dataProvider.SaveXML(dataProvider.FileName);
dataProvider.IsDirty = false;
```

### Creating Multiple Appointments at Once

```csharp
private void AddMultipleAppointments()
{
    ArrayListDataProvider dataProvider = scheduleControl1.DataSource as ArrayListDataProvider;
    
    // Array of appointment details
    var appointments = new[]
    {
        new { Subject = "Morning Standup", Start = DateTime.Today.AddHours(9), Duration = 0.5, Label = 1 },
        new { Subject = "Client Meeting", Start = DateTime.Today.AddHours(14), Duration = 1.0, Label = 2 },
        new { Subject = "Code Review", Start = DateTime.Today.AddDays(1).AddHours(10), Duration = 1.5, Label = 3 }
    };
    
    foreach (var apptData in appointments)
    {
        IScheduleAppointment appt = dataProvider.NewScheduleAppointment();
        appt.StartTime = apptData.Start;
        appt.EndTime = apptData.Start.AddHours(apptData.Duration);
        appt.Subject = apptData.Subject;
        appt.LabelValue = apptData.Label;
        
        dataProvider.AddItem(appt);
    }
    
    // Refresh to display all new appointments
    scheduleControl1.Refresh();
}
```

## XML Format Example

The `ScheduleAppearance.SaveXml` method creates a structured XML file containing schedule data:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScheduleAppearance>
  <Appointments>
    <Appointment>
      <UniqueID>1</UniqueID>
      <Subject>Team Meeting</Subject>
      <StartTime>2026-04-16T09:00:00</StartTime>
      <EndTime>2026-04-16T10:00:00</EndTime>
      <Content>Weekly standup</Content>
      <AllDay>false</AllDay>
      <LabelValue>2</LabelValue>
      <MarkerValue>2</MarkerValue>
      <LocationValue>Conference Room A</LocationValue>
      <ReminderValue>3</ReminderValue>
      <Reminder>true</Reminder>
      <Owner>0</Owner>
      <ForeColor>-16776961</ForeColor>
    </Appointment>
  </Appointments>
  <AppearanceSettings>
    <!-- Appearance settings like colors, fonts, etc. -->
  </AppearanceSettings>
</ScheduleAppearance>
```

**Benefits:**
- ✅ Built-in Syncfusion support
- ✅ No additional dependencies
- ✅ Handles both appointments and appearance
- ✅ Standard XML format
- ✅ Cross-platform compatible

## Working with File Dialogs

```csharp
// Save with file dialog
private void SaveScheduleWithDialog()
{
    ArrayListDataProvider dataProvider = scheduleControl1.DataSource as ArrayListDataProvider;
    
    SaveFileDialog saveDialog = new SaveFileDialog
    {
        Filter = "XML files (*.xml)|*.xml|All files (*.*)|*.*",
        Title = "Save Schedule",
        DefaultExt = "xml",
        FileName = "schedule.xml"
    };

    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        try
        {
            dataProvider.SaveXML(saveDialog.FileName);
            dataProvider.FileName = saveDialog.FileName;
            dataProvider.IsDirty = false;
            MessageBox.Show("Schedule saved successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error saving: {ex.Message}", "Save Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}

// Load with file dialog
private void LoadScheduleWithDialog()
{
    OpenFileDialog openDialog = new OpenFileDialog
    {
        Filter = "XML files (*.xml)|*.xml|All files (*.*)|*.*",
        Title = "Open Schedule",
        DefaultExt = "xml"
    };

    if (openDialog.ShowDialog() == DialogResult.OK)
    {
        try
        {
            ArrayListDataProvider dataProvider = ArrayListDataProvider.LoadXML(openDialog.FileName);
            dataProvider.FileName = openDialog.FileName;
            dataProvider.IsDirty = false;
            
            // Update data source
            scheduleControl1.DataSource = dataProvider;
            scheduleControl1.Refresh();
            
            MessageBox.Show("Schedule loaded successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error loading: {ex.Message}", "Load Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
```

## Key Benefits of ArrayListDataProvider

- ✅ **Built-in Solution** - No custom class implementation required
- ✅ **Simple to Use** - Minimal code to get started
- ✅ **Full-featured** - Supports all appointment operations (Add, Remove, Update)
- ✅ **XML Persistence** - Works seamlessly with SaveXML/LoadXML
- ✅ **Production Ready** - Tested and maintained by Syncfusion
- ✅ **Perfect for Prototypes** - Quick setup for demos and testing
- ✅ **Lightweight** - No database or complex infrastructure needed

## When to Use ArrayListDataProvider

**Ideal for:**
- Quick prototypes and demos
- Small to medium datasets (< 1000 appointments)
- Single-user desktop applications
- Simple file-based persistence needs
- Learning and testing scenarios

**Consider Alternatives for:**
- Enterprise applications with thousands of appointments
- Multi-user concurrent access scenarios
- Complex querying and filtering requirements
- Integration with existing database systems

## Best Practices

1. **Always use try-catch** for file operations to handle IO errors
2. **Validate file paths** before saving/loading
3. **Check file existence** before loading with `File.Exists()`
4. **Provide user feedback** for save/load operations
5. **Refresh the UI** after loading:
   - Call `scheduleControl.Refresh()` after `LoadXML()` to update the display
6. **Use correct API syntax**:
   - Save: `dataProvider.SaveXML(fileName)` (instance method on ArrayListDataProvider)
   - Load: `ArrayListDataProvider.LoadXML(fileName)` (static method)
7. **Track file state**:
   - Set `dataProvider.FileName` to track the current file
   - Set `dataProvider.IsDirty = false` after successful save/load
8. **Initialize ArrayListDataProvider early**:
   - Create it in Form_Load or constructor
   - Check for existing file and load, or create new instance
   - Initialize `MasterList` as `new ArrayListAppointmentList()` for new instances
9. **Backup data** before overwriting existing files
10. **Handle exceptions gracefully** with appropriate error messages

## Key Points

- `SaveXML()` is an **instance method** on ArrayListDataProvider - saves appointment data to XML
- `LoadXML()` is a **static method** - returns an ArrayListDataProvider with loaded appointments
- Method names use capital XML: `SaveXML` and `LoadXML` (not SaveXml/LoadXml)
- Always set `FileName` and `IsDirty` properties after save/load operations
- Call `Refresh()` after loading to update the UI
- Initialize `MasterList` as `new ArrayListAppointmentList()` when creating new providers

## Key Advantages of ArrayListDataProvider + XML

- ✅ **Integrated serialization** - Built-in SaveXML/LoadXML methods on the data provider itself
- ✅ **No custom classes needed** - Everything included in ArrayListDataProvider
- ✅ **Easy to implement** - Minimal code, straightforward API
- ✅ **Human-readable** - XML format can be viewed and edited
- ✅ **File state tracking** - Built-in FileName and IsDirty properties
- ✅ **Reliable** - Tested and maintained by Syncfusion
- ✅ **Perfect for prototypes** - Quick setup without complexity
