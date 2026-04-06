# Getting Started with Windows Forms Scheduler

This guide covers installation, basic setup, and fundamental appointment management operations for the Syncfusion Windows Forms Scheduler control.

## Assembly Deployment

### Required Assemblies

To use the ScheduleControl, add the following assembly references to your project:

- **Syncfusion.Grid.Base** - Base grid functionality
- **Syncfusion.Grid.Windows** - Windows Forms grid components
- **Syncfusion.Schedule.Base** - Core scheduling functionality
- **Syncfusion.Schedule.Windows** - Windows Forms scheduling UI
- **Syncfusion.Shared.Base** - Shared base utilities
- **Syncfusion.Tools.Windows** - Additional tool components

### NuGet Package Installation

Alternatively, install via NuGet Package Manager:

```bash
Install-Package Syncfusion.Schedule.Windows
```

This automatically includes all required dependencies.

## Creating the ScheduleControl

### Adding via Designer

1. Open your Windows Forms project in Visual Studio
2. Locate **ScheduleControl** in the Toolbox
3. Drag and drop the control onto your form designer
4. Required assemblies are automatically added to your project
5. Use the Properties window to configure the `Appearance` property and other settings

**Visual Studio Designer:**
- The ScheduleControl appears on the design surface with default appearance
- Access properties through the Property Grid
- The `Appearance` property provides extensive visual customization options

### Adding via Code

For programmatic control creation:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Schedule;

namespace SchedulerApp
{
    public partial class Form1 : Form
    {
        private ScheduleControl scheduleControl1;
        
        public Form1()
        {
            InitializeComponent();
            CreateScheduleControl();
        }
        
        private void CreateScheduleControl()
        {
            // Create instance
            scheduleControl1 = new ScheduleControl();
            
            // Set location and size
            scheduleControl1.Location = new Point(82, 12);
            scheduleControl1.Size = new Size(800, 600);
            
            // Add to form
            this.Controls.Add(scheduleControl1);
        }
    }
}
```

## Data Binding with SimpleScheduleDataProvider

The ScheduleControl is a data-bound control requiring a data provider to manage appointments.

### Using SimpleScheduleDataProvider

The `SimpleScheduleDataProvider` class provides a basic implementation of the data provider interfaces. This implementation ships with the Syncfusion samples.

**Location:** `Syncfusion_install_folder\Syncfusion\Essential Studio\{version}\Windows\Schedule.Windows\Samples\{framework}\ScheduleSample\CS\SimpleScheduleDataProvider.cs`

### Adding SimpleScheduleDataProvider to Your Project

1. Locate the `SimpleScheduleDataProvider.cs` file in the sample folder
2. Right-click your project in Solution Explorer → Add → Existing Item
3. Browse to and select `SimpleScheduleDataProvider.cs`
4. The file is now part of your project

### Binding Data in Form_Load

```csharp
using System;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Schedule;
using GridScheduleSample; // Namespace for SimpleScheduleDataProvider

namespace SchedulerApp
{
    public partial class Form1 : Form
    {
        private void Form1_Load(object sender, EventArgs e)
        {
            // Create data provider
            SimpleScheduleDataProvider data;
            
            // Check if saved data exists
            if (File.Exists("default.schedule"))
            {
                // Load existing data
                data = SimpleScheduleDataProvider.LoadBinary("default.schedule");
                data.FileName = "default.schedule";
            }
            else
            {
                // Create new data provider
                data = new SimpleScheduleDataProvider();
                data.MasterList = new SimpleScheduleAppointmentList();
                data.FileName = "default.schedule";
            }
            
            // Set schedule type and bind data
            this.scheduleControl1.ScheduleType = ScheduleViewType.Month;
            this.scheduleControl1.DataSource = data;
        }
    }
}
```

## Basic Appointment Operations

### Insert Appointment

To create a new appointment:

1. **Double-click** a timeslot in the ScheduleGrid
2. The **Appointment Form** appears
3. Enter appointment details:
   - **Subject:** Appointment title
   - **Start Time:** Beginning date/time
   - **End Time:** Ending date/time
   - **All Day Event:** Check for all-day appointments
   - **Location:** Meeting location
   - **Content:** Detailed description
4. Click **Save and Close**

The appointment appears in the schedule grid.

**Programmatic Insert:**

```csharp
// Get data provider
SimpleScheduleDataProvider dataProvider = 
    scheduleControl1.DataSource as SimpleScheduleDataProvider;

// Create new appointment
IScheduleAppointment appointment = dataProvider.NewScheduleAppointment();
appointment.StartTime = DateTime.Now.AddHours(1);
appointment.EndTime = DateTime.Now.AddHours(2);
appointment.Subject = "Team Meeting";
appointment.Content = "Discuss Q2 project milestones";
appointment.LabelValue = 2; // Business category
appointment.MarkerValue = 2; // Busy status

// Add to data provider
dataProvider.AddItem(appointment);
```

### Edit Appointment

To modify an existing appointment:

1. **Double-click** the appointment in the schedule
2. OR **Right-click** → Select **Edit Item** from context menu
3. Modify the desired fields in the Appointment Form
4. Click **Save and Close**

**Programmatic Edit:**

```csharp
// Appointments are objects - modify properties directly
IScheduleAppointment appointment = /* get existing appointment */;
appointment.Subject = "Updated Meeting Title";
appointment.EndTime = appointment.EndTime.AddHours(1); // Extend by 1 hour
appointment.LabelValue = 5; // Change to "Must Attend" category

// Changes are tracked automatically
```

### Delete Appointment

To remove an appointment:

1. **Right-click** the appointment in the schedule
2. Select **Delete Item** from the context menu
3. The appointment is removed immediately

**Programmatic Delete:**

```csharp
// Get data provider
SimpleScheduleDataProvider dataProvider = 
    scheduleControl1.DataSource as SimpleScheduleDataProvider;

// Remove appointment
IScheduleAppointment appointmentToDelete = /* get appointment */;
dataProvider.RemoveItem(appointmentToDelete);
```

### Save All Appointments

When closing the form, a dialog prompts to save changes:

```csharp
// Automatic save prompt on form close
// User clicks "Yes" to save changes to disk file

// Programmatic save
SimpleScheduleDataProvider dataProvider = 
    scheduleControl1.DataSource as SimpleScheduleDataProvider;
    
dataProvider.CommitChanges();
```

**Auto-save Configuration:**

```csharp
// Configure save behavior on control disposal
dataProvider.SaveOnCloseBehaviorAction = SaveOnCloseBehavior.PromptToSave;
// Options: PromptToSave, Save, DontSave
```

## Setting Appointment Text Color

Customize appointment appearance with the `ForeColor` property:

```csharp
SimpleScheduleAppointmentList masterList = new SimpleScheduleAppointmentList();

ScheduleAppointment item = masterList.NewScheduleAppointment() as ScheduleAppointment;
item.StartTime = DateTime.Now;
item.EndTime = item.StartTime.AddDays(2);
item.Subject = "Important Meeting";
item.ForeColor = Color.Red; // Set text color to red
masterList.Add(item);
```

**Common Color Patterns:**

```csharp
// Red for urgent/high priority
appointment.ForeColor = Color.Red;

// Blue for standard meetings
appointment.ForeColor = Color.Blue;

// Green for personal events
appointment.ForeColor = Color.Green;

// Dark colors for better contrast
appointment.ForeColor = Color.DarkRed;
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Schedule;
using GridScheduleSample;

namespace SchedulerApp
{
    public partial class Form1 : Form
    {
        private ScheduleControl scheduleControl1;
        
        public Form1()
        {
            InitializeComponent();
            this.Load += Form1_Load;
        }
        
        private void Form1_Load(object sender, EventArgs e)
        {
            // Create ScheduleControl
            scheduleControl1 = new ScheduleControl();
            scheduleControl1.Location = new Point(20, 20);
            scheduleControl1.Size = new Size(800, 600);
            scheduleControl1.Dock = DockStyle.Fill;
            
            // Set up data provider
            SimpleScheduleDataProvider data;
            
            if (File.Exists("appointments.schedule"))
            {
                data = SimpleScheduleDataProvider.LoadBinary("appointments.schedule");
                data.FileName = "appointments.schedule";
            }
            else
            {
                data = new SimpleScheduleDataProvider();
                data.MasterList = new SimpleScheduleAppointmentList();
                data.FileName = "appointments.schedule";
                
                // Add sample appointments
                AddSampleAppointments(data);
            }
            
            // Configure and bind
            scheduleControl1.ScheduleType = ScheduleViewType.Month;
            scheduleControl1.DataSource = data;
            
            this.Controls.Add(scheduleControl1);
        }
        
        private void AddSampleAppointments(SimpleScheduleDataProvider data)
        {
            // Morning meeting
            IScheduleAppointment meeting1 = data.NewScheduleAppointment();
            meeting1.StartTime = DateTime.Today.AddHours(9);
            meeting1.EndTime = DateTime.Today.AddHours(10);
            meeting1.Subject = "Team Standup";
            meeting1.Content = "Daily sync meeting";
            meeting1.LabelValue = 2; // Business
            meeting1.ForeColor = Color.Blue;
            data.AddItem(meeting1);
            
            // Afternoon meeting
            IScheduleAppointment meeting2 = data.NewScheduleAppointment();
            meeting2.StartTime = DateTime.Today.AddHours(14);
            meeting2.EndTime = DateTime.Today.AddHours(15, 30);
            meeting2.Subject = "Client Presentation";
            meeting2.Content = "Q2 Results Review";
            meeting2.LabelValue = 5; // Must Attend
            meeting2.MarkerValue = 2; // Busy
            meeting2.ForeColor = Color.DarkRed;
            data.AddItem(meeting2);
        }
    }
}
```

## Troubleshooting

### ScheduleControl Not Displaying Appointments

**Issue:** Control shows but no appointments appear.

**Solution:** Ensure `DataSource` is set after creating the data provider:
```csharp
scheduleControl1.DataSource = data; // Must be set
```

### Assembly Reference Errors

**Issue:** Cannot find `ScheduleControl` type.

**Solution:** Add all required Syncfusion assemblies:
- Syncfusion.Schedule.Windows (primary)
- Syncfusion.Schedule.Base
- Syncfusion.Grid.Windows
- Syncfusion.Grid.Base

### Data Not Persisting

**Issue:** Appointments disappear after closing application.

**Solution:** Ensure `CommitChanges()` is called or `SaveOnCloseBehaviorAction` is configured:
```csharp
data.SaveOnCloseBehaviorAction = SaveOnCloseBehavior.PromptToSave;
```

### SimpleScheduleDataProvider Not Found

**Issue:** Cannot resolve `SimpleScheduleDataProvider` type.

**Solution:** Add the `SimpleScheduleDataProvider.cs` file from the Syncfusion samples to your project. This is a sample implementation, not a built-in type.
