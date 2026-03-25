# RadioButtonAdv Events

## Overview

RadioButtonAdv provides specialized events that allow you to respond to user interactions and state changes. These events enable you to implement custom logic when users interact with radio buttons in your application.

### Available Events

RadioButtonAdv supports two primary events specifically designed for radio button functionality:

| Event | Description | Event Args |
|-------|-------------|------------|
| `CheckChanged` | Fires when the Checked property changes | EventArgs |
| `GroupCheckChanged` | Fires when any RadioButtonAdv in the group changes | EventArgs |

Additionally, RadioButtonAdv inherits standard control events like `Click`, `MouseEnter`, `MouseLeave`, etc.

## CheckChanged Event

The `CheckChanged` event is the most commonly used event for RadioButtonAdv. It fires whenever the `Checked` property of the radio button changes, either through user interaction or programmatic changes.

### Event Signature

```csharp
public event EventHandler CheckChanged;
```

### When It Fires

- User clicks on the radio button
- User uses keyboard (Space key) to select
- `Checked` property is set programmatically
- Another radio button in the same group is selected

### Basic Usage

**C#:**
```csharp
// Subscribe to the event
this.radioButtonAdv1.CheckChanged += RadioButtonAdv1_CheckChanged;

// Event handler
private void RadioButtonAdv1_CheckChanged(object sender, EventArgs e)
{
    Console.WriteLine("CheckChanged event is raised");
    
    // Check if the radio button is checked
    RadioButtonAdv radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked)
    {
        MessageBox.Show($"{radio.Text} has been selected");
    }
}
```

**VB.NET:**
```vb
' Subscribe to the event
AddHandler radioButtonAdv1.CheckChanged, AddressOf RadioButtonAdv1_CheckChanged

' Event handler
Private Sub RadioButtonAdv1_CheckChanged(sender As Object, e As EventArgs)
    Console.WriteLine("CheckChanged event is raised")
    
    ' Check if the radio button is checked
    Dim radio As RadioButtonAdv = TryCast(sender, RadioButtonAdv)
    If radio IsNot Nothing AndAlso radio.Checked Then
        MessageBox.Show($"{radio.Text} has been selected")
    End If
End Sub
```

### CheckChanged Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CheckChangedDemo
{
    public partial class CheckChangedForm : Form
    {
        private RadioButtonAdv radioSmall;
        private RadioButtonAdv radioMedium;
        private RadioButtonAdv radioLarge;
        private Label lblSelection;
        private Label lblPrice;

        public CheckChangedForm()
        {
            InitializeComponent();
            InitializeControls();
        }

        private void InitializeControls()
        {
            this.Text = "CheckChanged Event Demo";
            this.Size = new Size(450, 350);

            var title = new Label();
            title.Text = "Select Pizza Size:";
            title.Location = new Point(30, 20);
            title.Size = new Size(300, 25);
            title.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            this.Controls.Add(title);

            // Small
            radioSmall = new RadioButtonAdv();
            radioSmall.Text = "Small (10 inch)";
            radioSmall.Location = new Point(30, 60);
            radioSmall.Size = new Size(350, 30);
            radioSmall.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioSmall.Tag = 8.99m;
            radioSmall.Checked = true;
            radioSmall.CheckChanged += RadioSize_CheckChanged;
            this.Controls.Add(radioSmall);

            // Medium
            radioMedium = new RadioButtonAdv();
            radioMedium.Text = "Medium (12 inch)";
            radioMedium.Location = new Point(30, 100);
            radioMedium.Size = new Size(350, 30);
            radioMedium.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioMedium.Tag = 12.99m;
            radioMedium.CheckChanged += RadioSize_CheckChanged;
            this.Controls.Add(radioMedium);

            // Large
            radioLarge = new RadioButtonAdv();
            radioLarge.Text = "Large (14 inch)";
            radioLarge.Location = new Point(30, 140);
            radioLarge.Size = new Size(350, 30);
            radioLarge.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioLarge.Tag = 15.99m;
            radioLarge.CheckChanged += RadioSize_CheckChanged;
            this.Controls.Add(radioLarge);

            // Selection display
            lblSelection = new Label();
            lblSelection.Location = new Point(30, 200);
            lblSelection.Size = new Size(380, 30);
            lblSelection.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            lblSelection.ForeColor = Color.Navy;
            this.Controls.Add(lblSelection);

            // Price display
            lblPrice = new Label();
            lblPrice.Location = new Point(30, 240);
            lblPrice.Size = new Size(380, 30);
            lblPrice.Font = new Font("Segoe UI", 14F, FontStyle.Bold);
            lblPrice.ForeColor = Color.DarkGreen;
            this.Controls.Add(lblPrice);

            // Initialize display
            UpdateDisplay(radioSmall);
        }

        private void RadioSize_CheckChanged(object sender, EventArgs e)
        {
            RadioButtonAdv radio = sender as RadioButtonAdv;
            if (radio != null && radio.Checked)
            {
                UpdateDisplay(radio);
            }
        }

        private void UpdateDisplay(RadioButtonAdv selectedRadio)
        {
            lblSelection.Text = $"Selected: {selectedRadio.Text}";
            
            if (selectedRadio.Tag != null)
            {
                decimal price = (decimal)selectedRadio.Tag;
                lblPrice.Text = $"Price: ${price:F2}";
            }
        }
    }
}
```

## GroupCheckChanged Event

The `GroupCheckChanged` event fires when the `Checked` property of any RadioButtonAdv within the same container (group) changes. This is useful for monitoring changes across a group of radio buttons.

### Event Signature

```csharp
public event EventHandler GroupCheckChanged;
```

### When It Fires

- Any radio button in the group is checked
- Fires for all radio buttons in the group, not just the one being selected

### Basic Usage

**C#:**
```csharp
// Subscribe to the event
this.radioButtonAdv1.GroupCheckChanged += RadioButtonAdv_GroupCheckChanged;
this.radioButtonAdv2.GroupCheckChanged += RadioButtonAdv_GroupCheckChanged;
this.radioButtonAdv3.GroupCheckChanged += RadioButtonAdv_GroupCheckChanged;

// Event handler
private void RadioButtonAdv_GroupCheckChanged(object sender, EventArgs e)
{
    Console.WriteLine("GroupCheckChanged event is raised");
    
    RadioButtonAdv radio = sender as RadioButtonAdv;
    if (radio != null)
    {
        // This fires for ALL radio buttons in the group
        Console.WriteLine($"{radio.Text}: Checked = {radio.Checked}");
    }
}
```

**VB.NET:**
```vb
' Subscribe to the event
AddHandler radioButtonAdv1.GroupCheckChanged, AddressOf RadioButtonAdv_GroupCheckChanged
AddHandler radioButtonAdv2.GroupCheckChanged, AddressOf RadioButtonAdv_GroupCheckChanged
AddHandler radioButtonAdv3.GroupCheckChanged, AddressOf RadioButtonAdv_GroupCheckChanged

' Event handler
Private Sub RadioButtonAdv_GroupCheckChanged(sender As Object, e As EventArgs)
    Console.WriteLine("GroupCheckChanged event is raised")
    
    Dim radio As RadioButtonAdv = TryCast(sender, RadioButtonAdv)
    If radio IsNot Nothing Then
        ' This fires for ALL radio buttons in the group
        Console.WriteLine($"{radio.Text}: Checked = {radio.Checked}")
    End If
End Sub
```

### GroupCheckChanged Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace GroupCheckChangedDemo
{
    public partial class GroupForm : Form
    {
        private Panel panelGroup1;
        private Panel panelGroup2;
        private TextBox txtEventLog;

        public GroupForm()
        {
            InitializeComponent();
            InitializeGroups();
        }

        private void InitializeGroups()
        {
            this.Text = "GroupCheckChanged Event Demo";
            this.Size = new Size(600, 500);

            // Event log
            var lblLog = new Label();
            lblLog.Text = "Event Log:";
            lblLog.Location = new Point(30, 20);
            lblLog.Size = new Size(100, 20);
            this.Controls.Add(lblLog);

            txtEventLog = new TextBox();
            txtEventLog.Location = new Point(30, 45);
            txtEventLog.Size = new Size(520, 150);
            txtEventLog.Multiline = true;
            txtEventLog.ScrollBars = ScrollBars.Vertical;
            txtEventLog.ReadOnly = true;
            this.Controls.Add(txtEventLog);

            var btnClear = new Button();
            btnClear.Text = "Clear Log";
            btnClear.Location = new Point(470, 20);
            btnClear.Size = new Size(80, 25);
            btnClear.Click += (s, e) => txtEventLog.Clear();
            this.Controls.Add(btnClear);

            // Group 1
            panelGroup1 = new Panel();
            panelGroup1.Location = new Point(30, 210);
            panelGroup1.Size = new Size(250, 200);
            panelGroup1.BorderStyle = BorderStyle.FixedSingle;
            this.Controls.Add(panelGroup1);

            var lblGroup1 = new Label();
            lblGroup1.Text = "Group 1 - Beverages";
            lblGroup1.Location = new Point(10, 10);
            lblGroup1.Size = new Size(200, 20);
            lblGroup1.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
            panelGroup1.Controls.Add(lblGroup1);

            CreateRadioInGroup(panelGroup1, "Coffee", new Point(20, 40), 1);
            CreateRadioInGroup(panelGroup1, "Tea", new Point(20, 80), 1);
            CreateRadioInGroup(panelGroup1, "Juice", new Point(20, 120), 1);

            // Group 2
            panelGroup2 = new Panel();
            panelGroup2.Location = new Point(300, 210);
            panelGroup2.Size = new Size(250, 200);
            panelGroup2.BorderStyle = BorderStyle.FixedSingle;
            this.Controls.Add(panelGroup2);

            var lblGroup2 = new Label();
            lblGroup2.Text = "Group 2 - Snacks";
            lblGroup2.Location = new Point(10, 10);
            lblGroup2.Size = new Size(200, 20);
            lblGroup2.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
            panelGroup2.Controls.Add(lblGroup2);

            CreateRadioInGroup(panelGroup2, "Chips", new Point(20, 40), 2);
            CreateRadioInGroup(panelGroup2, "Cookies", new Point(20, 80), 2);
            CreateRadioInGroup(panelGroup2, "Nuts", new Point(20, 120), 2);
        }

        private void CreateRadioInGroup(Panel parent, string text, Point location, int groupNum)
        {
            var radio = new RadioButtonAdv();
            radio.Text = text;
            radio.Location = location;
            radio.Size = new Size(200, 30);
            radio.Style = RadioButtonAdvStyle.Office2016Colorful;
            radio.Tag = $"Group{groupNum}";
            
            // Subscribe to both events
            radio.CheckChanged += Radio_CheckChanged;
            radio.GroupCheckChanged += Radio_GroupCheckChanged;
            
            parent.Controls.Add(radio);
        }

        private void Radio_CheckChanged(object sender, EventArgs e)
        {
            var radio = sender as RadioButtonAdv;
            if (radio != null && radio.Checked)
            {
                LogEvent($"CheckChanged: {radio.Text} in {radio.Tag} - SELECTED");
            }
        }

        private void Radio_GroupCheckChanged(object sender, EventArgs e)
        {
            var radio = sender as RadioButtonAdv;
            if (radio != null)
            {
                string status = radio.Checked ? "CHECKED" : "UNCHECKED";
                LogEvent($"GroupCheckChanged: {radio.Text} in {radio.Tag} - {status}");
            }
        }

        private void LogEvent(string message)
        {
            string timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
            txtEventLog.AppendText($"[{timestamp}] {message}\r\n");
        }
    }
}
```

## Event Handling Patterns

### Pattern 1: Single Handler for Multiple Radio Buttons

**C#:**
```csharp
private void InitializeRadioButtons()
{
    // Create multiple radio buttons
    var radios = new[] {
        CreateRadio("Option 1", new Point(30, 30)),
        CreateRadio("Option 2", new Point(30, 70)),
        CreateRadio("Option 3", new Point(30, 110))
    };

    // Use same handler for all
    foreach (var radio in radios)
    {
        radio.CheckChanged += SharedRadio_CheckChanged;
    }
}

private void SharedRadio_CheckChanged(object sender, EventArgs e)
{
    var radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked)
    {
        // Handle based on which radio button was selected
        switch (radio.Text)
        {
            case "Option 1":
                ProcessOption1();
                break;
            case "Option 2":
                ProcessOption2();
                break;
            case "Option 3":
                ProcessOption3();
                break;
        }
    }
}
```

### Pattern 2: Using Tags for Data Association

**C#:**
```csharp
private void InitializeRadioButtons()
{
    var radio1 = new RadioButtonAdv();
    radio1.Text = "Red";
    radio1.Tag = Color.Red;
    radio1.CheckChanged += ColorRadio_CheckChanged;

    var radio2 = new RadioButtonAdv();
    radio2.Text = "Green";
    radio2.Tag = Color.Green;
    radio2.CheckChanged += ColorRadio_CheckChanged;

    var radio3 = new RadioButtonAdv();
    radio3.Text = "Blue";
    radio3.Tag = Color.Blue;
    radio3.CheckChanged += ColorRadio_CheckChanged;
}

private void ColorRadio_CheckChanged(object sender, EventArgs e)
{
    var radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked && radio.Tag is Color color)
    {
        this.BackColor = color;
    }
}
```

### Pattern 3: Validation on Selection

**C#:**
```csharp
private void RadioButton_CheckChanged(object sender, EventArgs e)
{
    var radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked)
    {
        // Validate selection
        if (!IsValidSelection(radio))
        {
            MessageBox.Show("This option is not available", 
                          "Invalid Selection", 
                          MessageBoxButtons.OK, 
                          MessageBoxIcon.Warning);
            
            // Revert to previous selection
            radio.Checked = false;
            previousSelection.Checked = true;
            return;
        }

        // Store as previous selection
        previousSelection = radio;
        
        // Process valid selection
        ProcessSelection(radio);
    }
}
```

## Common Event Scenarios

### Scenario 1: Update UI Based on Selection

**C#:**
```csharp
private void Radio_CheckChanged(object sender, EventArgs e)
{
    var radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked)
    {
        // Update related controls
        txtDescription.Text = GetDescription(radio.Text);
        picImage.Image = GetImage(radio.Text);
        lblPrice.Text = GetPrice(radio.Text);
        
        // Enable/disable controls
        btnConfirm.Enabled = true;
    }
}
```

### Scenario 2: Cascade Changes

**C#:**
```csharp
private void RadioCategory_CheckChanged(object sender, EventArgs e)
{
    var radio = sender as RadioButtonAdv;
    if (radio != null && radio.Checked)
    {
        // Clear and repopulate dependent controls
        panelSubOptions.Controls.Clear();
        
        // Load sub-options based on category
        LoadSubOptions(radio.Text);
    }
}
```

### Scenario 3: Real-time Calculation

**C#:**
```csharp
private void RadioSize_CheckChanged(object sender, EventArgs e)
{
    CalculateTotal();
}

private void CalculateTotal()
{
    decimal total = 0;
    
    // Get size price
    if (radioSmall.Checked) total += 10.00m;
    else if (radioMedium.Checked) total += 15.00m;
    else if (radioLarge.Checked) total += 20.00m;
    
    // Update display
    lblTotal.Text = $"Total: ${total:F2}";
}
```

## Best Practices

### Event Handler Guidelines

1. **Always check if radio button is checked** before taking action in CheckChanged
2. **Use CheckChanged over Click** for radio button state changes
3. **Avoid heavy processing** in event handlers; use async for long operations
4. **Unsubscribe from events** when dynamically creating/destroying controls
5. **Use sender parameter** to identify which radio button triggered the event

### CheckChanged vs GroupCheckChanged

**Use CheckChanged when:**
- You only care about the selected radio button
- Implementing simple selection logic
- Updating UI based on current selection

**Use GroupCheckChanged when:**
- You need to know about all state changes in the group
- Implementing complex validation across the group
- Monitoring uncheck events as well as check events

### Error Handling

**C#:**
```csharp
private void Radio_CheckChanged(object sender, EventArgs e)
{
    try
    {
        var radio = sender as RadioButtonAdv;
        if (radio != null && radio.Checked)
        {
            ProcessSelection(radio);
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error processing selection: {ex.Message}",
                       "Error",
                       MessageBoxButtons.OK,
                       MessageBoxIcon.Error);
        
        // Log the error
        LogError(ex);
    }
}
```

### Performance Considerations

```csharp
// DON'T: Create expensive objects in event handler
private void Radio_CheckChanged(object sender, EventArgs e)
{
    // Bad: Creates new connection each time
    var connection = new SqlConnection(connectionString);
    // ...
}

// DO: Use cached or pre-created objects
private SqlConnection _connection; // Field

private void Radio_CheckChanged(object sender, EventArgs e)
{
    // Good: Use existing connection
    if (_connection.State != ConnectionState.Open)
        _connection.Open();
    // ...
}
```

## Troubleshooting

### Event Not Firing

If CheckChanged event doesn't fire:
1. Verify event handler is properly subscribed
2. Check if `RaiseEventOnClick` is set to false (affects Click but not CheckChanged)
3. Ensure control is enabled
4. Verify the Checked property is actually changing

### Event Fires Multiple Times

If events fire unexpectedly:
1. Check for duplicate event subscriptions
2. Avoid setting `Checked` property within the event handler (causes recursion)
3. Use flags to prevent re-entry

### GroupCheckChanged Issues

If GroupCheckChanged behaves unexpectedly:
1. Ensure all radio buttons are in the same container
2. Remember it fires for ALL radio buttons in the group
3. Check the `Checked` property to determine if selection or deselection
