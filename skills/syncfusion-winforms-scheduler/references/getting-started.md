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
Install-Package Syncfusion.Schedule.Windows -Version *
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

## Data Binding with ArrayListDataProvider

The ScheduleControl is a data-bound control requiring a data provider to manage appointments. Syncfusion provides the built-in `ArrayListDataProvider` class for this purpose.

### ArrayListDataProvider - Built-in Solution

`ArrayListDataProvider` is included with Syncfusion.Schedule namespace and requires no custom implementation.

**Key Features:**
- ✅ **Built-in XML serialization** using integrated SaveXML/LoadXML methods
- ✅ **In-memory collection** (MasterList) holding all appointments
- ✅ **Complete CRUD operations** (Create, Read, Update, Delete)
- ✅ **No custom code needed** - ready to use out of the box
- ✅ **File state tracking** with FileName and IsDirty properties
- ✅ **Production ready** - tested and maintained by Syncfusion

📄 **Read:** [data-provider-xml-implementation.md](data-provider-xml-implementation.md) for complete examples and implementation patterns.

### Using ArrayListDataProvider

No additional files or custom classes needed - simply instantiate and use:

```csharp
using Syncfusion.Schedule;

// Create instance
ArrayListDataProvider dataProvider = new ArrayListDataProvider();
scheduleControl1.DataSource = dataProvider;
```

### Binding Data in Form_Load

```csharp
using System;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;

namespace SchedulerApp
{
    public partial class Form1 : Form
    {
        private ArrayListDataProvider dataProvider;
        private string dataFileName = "schedule.xml";
        
        private void Form1_Load(object sender, EventArgs e)
        {
            // Load existing data or create new provider
            if (File.Exists(dataFileName))
            {
                try
                {
                    // Load from XML file
                    dataProvider = ArrayListDataProvider.LoadXML(dataFileName);
                    dataProvider.FileName = dataFileName;
                    dataProvider.IsDirty = false;
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Failed to load schedule: {ex.Message}");
                    dataProvider = new ArrayListDataProvider();
                    dataProvider.MasterList = new ArrayListAppointmentList();
                    dataProvider.FileName = dataFileName;
                }
            }
            else
            {
                // Create new data provider
                dataProvider = new ArrayListDataProvider();
                dataProvider.MasterList = new ArrayListAppointmentList();
                dataProvider.FileName = dataFileName;
            }
            
            // Set schedule type and bind data
            this.scheduleControl1.ScheduleType = ScheduleViewType.Month;
            this.scheduleControl1.DataSource = dataProvider;
        }
        
        // Save schedule on form closing
        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            try
            {
                dataProvider.SaveXML(dataProvider.FileName);
                dataProvider.IsDirty = false;
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Failed to save schedule: {ex.Message}");
            }
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
ArrayListDataProvider dataProvider = 
    scheduleControl1.DataSource as ArrayListDataProvider;

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

// Refresh to display
scheduleControl1.Refresh();
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
ArrayListDataProvider dataProvider = 
    scheduleControl1.DataSource as ArrayListDataProvider;

// Remove appointment
IScheduleAppointment appointmentToDelete = /* get appointment */;
dataProvider.RemoveItem(appointmentToDelete);

// Refresh to update display
scheduleControl1.Refresh();
```

### Save All Appointments

Save appointments to XML file programmatically:

```csharp
// Get data provider
ArrayListDataProvider dataProvider = 
    scheduleControl1.DataSource as ArrayListDataProvider;

// Save to XML file
dataProvider.SaveXML(dataProvider.FileName);
dataProvider.IsDirty = false;
```

**Save on Form Close:**

```csharp
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    ArrayListDataProvider dataProvider = 
        scheduleControl1.DataSource as ArrayListDataProvider;
    
    if (dataProvider != null && dataProvider.IsDirty)
    {
        DialogResult result = MessageBox.Show(
            "Save changes before closing?",
            "Save Schedule",
            MessageBoxButtons.YesNoCancel,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.Yes)
        {
            dataProvider.SaveXML(dataProvider.FileName);
        }
        else if (result == DialogResult.Cancel)
        {
            e.Cancel = true; // Cancel close
        }
    }
}
```

## Setting Appointment Text Color

Customize appointment appearance with the `ForeColor` property:

```csharp
// Get data provider
ArrayListDataProvider dataProvider = 
    scheduleControl1.DataSource as ArrayListDataProvider;

// Create appointment with custom color
IScheduleAppointment item = dataProvider.NewScheduleAppointment();
item.StartTime = DateTime.Now;
item.EndTime = item.StartTime.AddDays(2);
item.Subject = "Important Meeting";
item.ForeColor = Color.Red; // Set text color to red

// Add to provider
dataProvider.AddItem(item);
scheduleControl1.Refresh();
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
using Syncfusion.Schedule;
using Syncfusion.Windows.Forms.Schedule;

namespace SchedulerApp
{
    public partial class Form1 : Form
    {
        private ScheduleControl scheduleControl1;
        private ArrayListDataProvider dataProvider;
        
        public Form1()
        {
            InitializeComponent();
            this.Load += Form1_Load;
            this.FormClosing += Form1_FormClosing;
        }
        
        private void Form1_Load(object sender, EventArgs e)
        {
            // Create ScheduleControl
            scheduleControl1 = new ScheduleControl();
            scheduleControl1.Location = new Point(20, 20);
            scheduleControl1.Size = new Size(800, 600);
            scheduleControl1.Dock = DockStyle.Fill;
            
            // Set up data provider
            string fileName = "schedule.xml";
            
            if (File.Exists(fileName))
            {
                // Load existing data
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
                AddSampleAppointments();
            }
            
            // Configure and bind
            scheduleControl1.ScheduleType = ScheduleViewType.Month;
            scheduleControl1.DataSource = dataProvider;
            
            this.Controls.Add(scheduleControl1);
        }
        
        private void AddSampleAppointments()
        {
            // Morning meeting
            IScheduleAppointment meeting1 = dataProvider.NewScheduleAppointment();
            meeting1.StartTime = DateTime.Today.AddHours(9);
            meeting1.EndTime = DateTime.Today.AddHours(10);
            meeting1.Subject = "Team Standup";
            meeting1.Content = "Daily sync meeting";
            meeting1.LabelValue = 2; // Business
            meeting1.ForeColor = Color.Blue;
            dataProvider.AddItem(meeting1);
            
            // Afternoon meeting
            IScheduleAppointment meeting2 = dataProvider.NewScheduleAppointment();
            meeting2.StartTime = DateTime.Today.AddHours(14);
            meeting2.EndTime = DateTime.Today.AddHours(15).AddMinutes(30);
            meeting2.Subject = "Client Presentation";
            meeting2.Content = "Q2 Results Review";
            meeting2.LabelValue = 5; // Must Attend
            meeting2.MarkerValue = 2; // Busy
            meeting2.ForeColor = Color.DarkRed;
            dataProvider.AddItem(meeting2);
        }
        
        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            if (dataProvider != null)
            {
                try
                {
                    dataProvider.SaveXML(dataProvider.FileName);
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Failed to save: {ex.Message}");
                }
            }
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

**Solution:** Ensure `SaveXML()` is called in the FormClosing event:
```csharp
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    ArrayListDataProvider dataProvider = 
        scheduleControl1.DataSource as ArrayListDataProvider;
    
    if (dataProvider != null)
    {
        dataProvider.SaveXML(dataProvider.FileName);
        dataProvider.IsDirty = false;
    }
}
```

### ArrayListDataProvider Type Not Found

**Issue:** Cannot resolve `ArrayListDataProvider` type.

**Solution:** Add the required using directive:
```csharp
using Syncfusion.Schedule; // Required for ArrayListDataProvider
```

## Additional Resources

**XML Implementation Guide:**
For complete source code with XML serialization and implementation examples:

📄 [XML Data Provider Implementation](data-provider-xml-implementation.md)
