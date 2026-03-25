# AutoLabel Events in Windows Forms AutoLabel

This section explains the events available for the AutoLabel control.

## PropertyChanged Event

The AutoLabel control provides the `PropertyChanged` event which is fired when specific properties of the control change.

### When It Fires

The PropertyChanged event is triggered when any of the following properties change:
- `LabeledControl` - The control this label is paired with
- `Gap` - The spacing between label and control
- `Position` - The relative position of the label

### Event Arguments

The event handler receives an argument of type `SyncfusionPropertyChangedEventArgs` containing data related to the property change.

| Property | Type | Description |
|----------|------|-------------|
| `PropertyName` | string | The name of the property that changed |
| `OldValue` | object | The previous value of the property |
| `NewValue` | object | The new value of the property |

## Basic Event Handling

### C# Example

```csharp
using Syncfusion.ComponentModel;
using Syncfusion.Windows.Forms.Tools;

// Subscribe to the event
this.autoLabel1.PropertyChanged += AutoLabel1_PropertyChanged;

// Event handler
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    Console.WriteLine($"Property '{e.PropertyName}' changed from '{e.OldValue}' to '{e.NewValue}'");
}
```

### VB.NET Example

```vb
Imports Syncfusion.ComponentModel
Imports Syncfusion.Windows.Forms.Tools

' Subscribe to the event
AddHandler Me.autoLabel1.PropertyChanged, AddressOf AutoLabel1_PropertyChanged

' Event handler
Private Sub autoLabel1_PropertyChanged(ByVal sender As Object, ByVal e As Syncfusion.ComponentModel.SyncfusionPropertyChangedEventArgs)
    Console.WriteLine($"Property '{e.PropertyName}' changed from '{e.OldValue}' to '{e.NewValue}'")
End Sub
```

## Detecting Specific Property Changes

You can check which property changed and respond accordingly:

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    switch (e.PropertyName)
    {
        case "LabeledControl":
            Console.WriteLine("Labeled control changed");
            // Handle labeled control change
            if (e.NewValue != null)
            {
                Control newControl = (Control)e.NewValue;
                Console.WriteLine($"Now labeling: {newControl.Name}");
            }
            break;
            
        case "Gap":
            Console.WriteLine($"Gap changed from {e.OldValue} to {e.NewValue}");
            // Handle gap change
            break;
            
        case "Position":
            Console.WriteLine($"Position changed from {e.OldValue} to {e.NewValue}");
            // Handle position change
            break;
    }
}
```

## Common Event Scenarios

### Scenario 1: Logging Property Changes

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    string message = $"{DateTime.Now}: AutoLabel property '{e.PropertyName}' " +
                    $"changed from '{e.OldValue ?? "null"}' to '{e.NewValue ?? "null"}'";
    
    System.Diagnostics.Debug.WriteLine(message);
    // Or log to file, database, etc.
}
```

### Scenario 2: Updating UI Based on Changes

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (e.PropertyName == "LabeledControl")
    {
        // Update a status label when the labeled control changes
        if (e.NewValue != null)
        {
            statusLabel.Text = $"Label is now associated with: {((Control)e.NewValue).Name}";
        }
        else
        {
            statusLabel.Text = "Label is not associated with any control";
        }
    }
}
```

### Scenario 3: Validating Property Changes

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (e.PropertyName == "Gap")
    {
        int newGap = (int)e.NewValue;
        
        if (newGap < 0)
        {
            MessageBox.Show("Warning: Gap value is negative. This may cause overlap.",
                          "Invalid Gap", MessageBoxButtons.OK, MessageBoxIcon.Warning);
        }
        else if (newGap > 50)
        {
            MessageBox.Show("Warning: Gap value is very large. Labels may be far from controls.",
                          "Large Gap", MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
}
```

### Scenario 4: Synchronizing Multiple Labels

```csharp
// Keep multiple labels in sync
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (e.PropertyName == "Gap")
    {
        // Apply the same gap to other labels
        int newGap = (int)e.NewValue;
        
        foreach (Control control in this.Controls)
        {
            if (control is AutoLabel && control != sender)
            {
                ((AutoLabel)control).Gap = newGap;
            }
        }
    }
}
```

### Scenario 5: Tracking Position Changes

```csharp
private Dictionary<AutoLabel, List<string>> positionHistory = 
    new Dictionary<AutoLabel, List<string>>();

private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (e.PropertyName == "Position")
    {
        AutoLabel label = (AutoLabel)sender;
        
        if (!positionHistory.ContainsKey(label))
        {
            positionHistory[label] = new List<string>();
        }
        
        positionHistory[label].Add(e.NewValue.ToString());
        
        Console.WriteLine($"Position history for {label.Text}: " +
                         string.Join(" -> ", positionHistory[label]));
    }
}
```

## Complete Example

Here's a complete example demonstrating event handling with multiple AutoLabel controls:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.ComponentModel;
using Syncfusion.Windows.Forms.Tools;

namespace AutoLabelEvents
{
    public partial class Form1 : Form
    {
        private AutoLabel nameLabel, emailLabel;
        private TextBox nameTextBox, emailTextBox;
        private Label statusLabel;
        
        public Form1()
        {
            InitializeComponent();
            
            // Create status label
            statusLabel = new Label();
            statusLabel.Location = new Point(20, 200);
            statusLabel.Size = new Size(400, 50);
            statusLabel.BorderStyle = BorderStyle.FixedSingle;
            statusLabel.Text = "Status: Ready";
            
            // Create controls
            nameTextBox = new TextBox();
            nameTextBox.Location = new Point(150, 50);
            nameTextBox.Size = new Size(200, 20);
            
            emailTextBox = new TextBox();
            emailTextBox.Location = new Point(150, 100);
            emailTextBox.Size = new Size(200, 20);
            
            // Create AutoLabels with event handlers
            nameLabel = new AutoLabel();
            nameLabel.Text = "Name:";
            nameLabel.LabeledControl = nameTextBox;
            nameLabel.Position = AutoLabelPosition.Left;
            nameLabel.Gap = 10;
            nameLabel.PropertyChanged += Label_PropertyChanged;
            
            emailLabel = new AutoLabel();
            emailLabel.Text = "Email:";
            emailLabel.LabeledControl = emailTextBox;
            emailLabel.Position = AutoLabelPosition.Left;
            emailLabel.Gap = 10;
            emailLabel.PropertyChanged += Label_PropertyChanged;
            
            // Add controls to form
            this.Controls.Add(statusLabel);
            this.Controls.Add(nameTextBox);
            this.Controls.Add(nameLabel);
            this.Controls.Add(emailTextBox);
            this.Controls.Add(emailLabel);
            
            // Add button to test property changes
            Button changeGapButton = new Button();
            changeGapButton.Text = "Increase Gap";
            changeGapButton.Location = new Point(20, 150);
            changeGapButton.Click += (s, e) => {
                nameLabel.Gap += 5;
                emailLabel.Gap += 5;
            };
            this.Controls.Add(changeGapButton);
        }
        
        private void Label_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
        {
            AutoLabel label = (AutoLabel)sender;
            string labelText = label.Text;
            
            statusLabel.Text = $"'{labelText}' - Property '{e.PropertyName}' changed:\n" +
                              $"Old: {e.OldValue ?? "null"}, New: {e.NewValue ?? "null"}";
        }
    }
}
```

## Best Practices

### 1. Unsubscribe from Events

Always unsubscribe from events to prevent memory leaks:

```csharp
protected override void OnFormClosing(FormClosingEventArgs e)
{
    // Unsubscribe from events
    autoLabel1.PropertyChanged -= AutoLabel1_PropertyChanged;
    
    base.OnFormClosing(e);
}
```

### 2. Handle Null Values

Check for null values in event arguments:

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        // Process new value
    }
    else
    {
        // Handle null case
    }
}
```

### 3. Avoid Recursive Changes

Be careful not to change properties within the PropertyChanged event that could trigger the event again:

```csharp
private bool isUpdating = false;

private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    if (isUpdating) return;  // Prevent recursion
    
    try
    {
        isUpdating = true;
        // Make changes if needed
    }
    finally
    {
        isUpdating = false;
    }
}
```

### 4. Use Switch for Multiple Properties

Use a switch statement for cleaner code when handling multiple properties:

```csharp
private void AutoLabel1_PropertyChanged(object sender, SyncfusionPropertyChangedEventArgs e)
{
    switch (e.PropertyName)
    {
        case "LabeledControl":
            HandleLabeledControlChange(e);
            break;
        case "Gap":
            HandleGapChange(e);
            break;
        case "Position":
            HandlePositionChange(e);
            break;
    }
}
```

## Troubleshooting

**Issue**: Event not firing
- Verify event handler is subscribed correctly
- Check that you're actually changing one of the tracked properties (LabeledControl, Gap, Position)
- Ensure the property value is actually changing (not setting to the same value)

**Issue**: Event fires multiple times
- Check for recursive property changes in event handler
- Verify you haven't subscribed to the event multiple times
- Use a flag to prevent recursive calls

**Issue**: Cannot access property values in event handler
- Check for null values before accessing properties
- Ensure proper type casting when accessing NewValue and OldValue
- Verify the property exists on the object
