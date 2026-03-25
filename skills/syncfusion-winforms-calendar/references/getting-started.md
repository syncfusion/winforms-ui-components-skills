# Getting Started

This guide covers installation, setup, and basic configuration of the SfCalendar control.

## When to Read This

Read this guide when you need to:
- Install SfCalendar for the first time
- Add SfCalendar to a WinForms project via designer or code
- Understand required assemblies and namespaces
- Set up a basic calendar with initial date
- Configure essential properties
- Verify installation and basic functionality

## Assembly Requirements

To use SfCalendar, you need the following assemblies:

| Assembly | Purpose |
|----------|---------|
| `Syncfusion.Core.WinForms.dll` | Core functionality for Syncfusion WinForms controls |
| `Syncfusion.SfInput.WinForms.dll` | Input controls including SfCalendar |
| `Syncfusion.Shared.Base.dll` | Shared base functionality |

**Namespace:**

**C#:**
```csharp
using Syncfusion.WinForms.Input;
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Input
```

## Installation Methods

### Method 1: NuGet Package Manager Console

```powershell
Install-Package Syncfusion.SfInput.WinForms
```

This automatically installs all required dependencies.

### Method 2: NuGet Package Manager UI

1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for "Syncfusion.SfInput.WinForms"
4. Click **Install**
5. Accept the license agreement

### Method 3: Manual Assembly Reference

1. Locate Syncfusion assemblies (typically in `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\[Version]\Assemblies\[Framework]\`)
2. Right-click **References** in Solution Explorer
3. Select **Add Reference**
4. Browse to the assemblies location
5. Select the three required DLLs
6. Click **OK**

## Adding SfCalendar via Designer

1. **Create or open a WinForms project** in Visual Studio
2. **Open the Toolbox** (View → Toolbox or Ctrl+Alt+X)
3. **Locate SfCalendar** in the Toolbox (under Syncfusion Controls)
   - If not visible, right-click Toolbox → Choose Items → Browse to assemblies
4. **Drag and drop** SfCalendar onto your form
5. **Configure properties** in the Properties window:

| Property | Value | Description |
|----------|-------|-------------|
| `Location` | `20, 20` | Position on form |
| `Size` | `350, 300` | Calendar dimensions |
| `(Name)` | `sfCalendar1` | Control instance name |

## Adding SfCalendar via Code

### Basic Code Setup

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using System;
using System.Drawing;
using System.Windows.Forms;

namespace CalendarApp
{
    public partial class Form1 : Form
    {
        private SfCalendar sfCalendar1;
        
        public Form1()
        {
            InitializeComponent();
            
            // Create SfCalendar instance
            sfCalendar1 = new SfCalendar();
            sfCalendar1.Location = new Point(20, 20);
            sfCalendar1.Size = new Size(350, 300);
            
            // Add to form
            this.Controls.Add(sfCalendar1);
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Input
Imports System
Imports System.Drawing
Imports System.Windows.Forms

Namespace CalendarApp
    Public Partial Class Form1
        Inherits Form
        
        Private sfCalendar1 As SfCalendar
        
        Public Sub New()
            InitializeComponent()
            
            ' Create SfCalendar instance
            sfCalendar1 = New SfCalendar()
            sfCalendar1.Location = New Point(20, 20)
            sfCalendar1.Size = New Size(350, 300)
            
            ' Add to form
            Me.Controls.Add(sfCalendar1)
        End Sub
    End Class
End Namespace
```

## Setting Initial Date

### Set Specific Date

**C#:**
```csharp
sfCalendar1.SelectedDate = new DateTime(2026, 3, 25);
```

**VB.NET:**
```vb
sfCalendar1.SelectedDate = New DateTime(2026, 3, 25)
```

### Set Today's Date

**C#:**
```csharp
sfCalendar1.SelectedDate = DateTime.Today;
```

**VB.NET:**
```vb
sfCalendar1.SelectedDate = DateTime.Today
```

### No Initial Selection

**C#:**
```csharp
sfCalendar1.SelectedDate = null;  // No date selected initially
```

**VB.NET:**
```vb
sfCalendar1.SelectedDate = Nothing
```

## Basic Configuration

### Essential Properties

**C#:**
```csharp
// Set calendar size
sfCalendar1.Size = new Size(350, 300);

// Show Today button
sfCalendar1.ShowToday = true;

// Show None button (clear selection)
sfCalendar1.ShowNone = true;

// Set first day of week
sfCalendar1.FirstDayOfWeek = DayOfWeek.Monday;
```

**VB.NET:**
```vb
' Set calendar size
sfCalendar1.Size = New Size(350, 300)

' Show Today button
sfCalendar1.ShowToday = True

' Show None button (clear selection)
sfCalendar1.ShowNone = True

' Set first day of week
sfCalendar1.FirstDayOfWeek = DayOfWeek.Monday
```

## Handling Date Selection

### Selection Changed Event

**C#:**
```csharp
sfCalendar1.SelectedDateChanged += SfCalendar1_SelectedDateChanged;

private void SfCalendar1_SelectedDateChanged(object sender, EventArgs e)
{
    if (sfCalendar1.SelectedDate.HasValue)
    {
        DateTime selectedDate = sfCalendar1.SelectedDate.Value;
        MessageBox.Show($"Selected date: {selectedDate:D}");
    }
    else
    {
        MessageBox.Show("No date selected");
    }
}
```

**VB.NET:**
```vb
AddHandler sfCalendar1.SelectedDateChanged, AddressOf SfCalendar1_SelectedDateChanged

Private Sub SfCalendar1_SelectedDateChanged(sender As Object, e As EventArgs)
    If sfCalendar1.SelectedDate.HasValue Then
        Dim selectedDate As DateTime = sfCalendar1.SelectedDate.Value
        MessageBox.Show($"Selected date: {selectedDate:D}")
    Else
        MessageBox.Show("No date selected")
    End If
End Sub
```

## Complete Setup Example

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using System;
using System.Drawing;
using System.Windows.Forms;

namespace CalendarExample
{
    public class CalendarDemoForm : Form
    {
        private SfCalendar calendar;
        private Label lblInstruction;
        private Label lblSelected;
        private Button btnShowSelection;
        
        public CalendarDemoForm()
        {
            InitializeForm();
            SetupCalendar();
            SetupControls();
        }
        
        private void InitializeForm()
        {
            this.Text = "SfCalendar Demo";
            this.Size = new Size(420, 480);
            this.StartPosition = FormStartPosition.CenterScreen;
        }
        
        private void SetupCalendar()
        {
            calendar = new SfCalendar
            {
                Location = new Point(20, 50),
                Size = new Size(370, 320),
                ShowToday = true,
                ShowNone = true,
                SelectedDate = DateTime.Today
            };
            
            calendar.SelectedDateChanged += Calendar_SelectedDateChanged;
            this.Controls.Add(calendar);
        }
        
        private void SetupControls()
        {
            lblInstruction = new Label
            {
                Location = new Point(20, 15),
                Size = new Size(370, 25),
                Text = "Select a date from the calendar:",
                Font = new Font("Segoe UI", 10F, FontStyle.Bold)
            };
            
            lblSelected = new Label
            {
                Location = new Point(20, 380),
                Size = new Size(370, 30),
                Text = $"Selected: {DateTime.Today:D}",
                Font = new Font("Segoe UI", 9F)
            };
            
            btnShowSelection = new Button
            {
                Location = new Point(20, 415),
                Size = new Size(150, 30),
                Text = "Show Selection"
            };
            btnShowSelection.Click += BtnShowSelection_Click;
            
            this.Controls.Add(lblInstruction);
            this.Controls.Add(lblSelected);
            this.Controls.Add(btnShowSelection);
        }
        
        private void Calendar_SelectedDateChanged(object sender, EventArgs e)
        {
            if (calendar.SelectedDate.HasValue)
            {
                lblSelected.Text = $"Selected: {calendar.SelectedDate.Value:D}";
            }
            else
            {
                lblSelected.Text = "No date selected";
            }
        }
        
        private void BtnShowSelection_Click(object sender, EventArgs e)
        {
            if (calendar.SelectedDate.HasValue)
            {
                MessageBox.Show(
                    $"Selected Date: {calendar.SelectedDate.Value:D}\n" +
                    $"Day of Week: {calendar.SelectedDate.Value.DayOfWeek}",
                    "Date Information",
                    MessageBoxButtons.OK,
                    MessageBoxIcon.Information
                );
            }
            else
            {
                MessageBox.Show("Please select a date first.", "No Selection",
                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
            }
        }
        
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new CalendarDemoForm());
        }
    }
}
```

## Troubleshooting

### Issue 1: SfCalendar not in Toolbox

**Solution:**
- Right-click Toolbox → Choose Items
- Click .NET Framework Components tab
- Click Browse and navigate to `Syncfusion.SfInput.WinForms.dll`
- Select the assembly and click Open
- Ensure SfCalendar is checked in the list
- Click OK

### Issue 2: Assembly Reference Errors

**Solution:**
- Verify all three required assemblies are referenced
- Check that assembly versions match
- Clean and rebuild solution
- Ensure target framework is compatible (.NET Framework 4.5+ or .NET 6+)

### Issue 3: SelectedDate Returns Null

**Solution:**
- Check if a date has been selected: `if (calendar.SelectedDate.HasValue)`
- Use `SelectedDate.Value` to get the actual DateTime
- Verify `SelectedDateChanged` event is wired up correctly

### Issue 4: Calendar Not Displaying

**Solution:**
- Verify `Size` property is set appropriately (minimum ~300x250)
- Check `Visible` property is true
- Ensure control is added to form's Controls collection
- Verify no other controls are overlapping the calendar

### Issue 5: Namespace Not Found

**Solution:**
- Add `using Syncfusion.WinForms.Input;` (C#) or `Imports Syncfusion.WinForms.Input` (VB.NET)
- Verify `Syncfusion.SfInput.WinForms.dll` is referenced
- Clean and rebuild the project

## Next Steps

- **[Date Selection](date-selection.md)** - Learn about single, multiple, and range selection modes, date restrictions, special dates, and blackout dates
- **[Views and Navigation](views-and-navigation.md)** - Explore Month, Year, Decade, and Century views for easy navigation
- **[Appearance Customization](appearance-customization.md)** - Customize colors, fonts, themes, and cell styling
