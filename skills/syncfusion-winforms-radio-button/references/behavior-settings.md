# Behavior Settings

## Overview

RadioButtonAdv provides several behavior-related properties that control how the control responds to user interactions and manages its internal state. These settings allow you to fine-tune the control's functionality to match your application's requirements.

### Key Behavior Features

- **Auto Height Calculation**: Automatically adjust height based on content
- **Focus Rectangle Control**: Show or hide the focus indicator
- **Event Control**: Manage when events are fired
- **State Value Association**: Associate custom values with radio button states

## AutoHeight Property

The `AutoHeight` property enables automatic height calculation based on the control's content, particularly useful when combined with text wrapping.

### Property Details

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `AutoHeight` | bool | Automatically calculates and sets the control height | False |

### Basic Usage

**C#:**
```csharp
// Enable automatic height calculation
this.radioButtonAdv1.AutoHeight = true;
this.radioButtonAdv1.Text = "Option 1";
```

**VB.NET:**
```vb
' Enable automatic height calculation
Me.radioButtonAdv1.AutoHeight = True
Me.radioButtonAdv1.Text = "Option 1"
```

### AutoHeight with Text Wrapping

AutoHeight is most useful when combined with text wrapping for dynamic content:

**C#:**
```csharp
// Automatically adjust height for wrapped text
this.radioButtonAdv1.WrapText = true;
this.radioButtonAdv1.AutoHeight = true;
this.radioButtonAdv1.Width = 250;
this.radioButtonAdv1.Text = "This is a long text that will wrap to multiple lines and the control height will automatically adjust";
```

**VB.NET:**
```vb
' Automatically adjust height for wrapped text
Me.radioButtonAdv1.WrapText = True
Me.radioButtonAdv1.AutoHeight = True
Me.radioButtonAdv1.Width = 250
Me.radioButtonAdv1.Text = "This is a long text that will wrap to multiple lines and the control height will automatically adjust"
```

### AutoHeight Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace AutoHeightDemo
{
    public partial class AutoHeightForm : Form
    {
        private RadioButtonAdv radioFixed;
        private RadioButtonAdv radioAuto;
        private Button btnChangeText;

        public AutoHeightForm()
        {
            InitializeComponent();
            InitializeControls();
        }

        private void InitializeControls()
        {
            this.Text = "AutoHeight Demonstration";
            this.Size = new Size(500, 350);
            this.BackColor = Color.White;

            // Fixed height radio button
            radioFixed = new RadioButtonAdv();
            radioFixed.Text = "Fixed Height - This text may be cut off if too long";
            radioFixed.Location = new Point(30, 30);
            radioFixed.Size = new Size(400, 40);
            radioFixed.WrapText = true;
            radioFixed.AutoHeight = false;
            radioFixed.BackColor = Color.LightYellow;
            this.Controls.Add(radioFixed);

            var labelFixed = new Label();
            labelFixed.Text = "(AutoHeight = false)";
            labelFixed.Location = new Point(30, 75);
            labelFixed.ForeColor = Color.Gray;
            labelFixed.Font = new Font("Arial", 8F, FontStyle.Italic);
            this.Controls.Add(labelFixed);

            // Auto height radio button
            radioAuto = new RadioButtonAdv();
            radioAuto.Text = "Auto Height - This control will grow as needed";
            radioAuto.Location = new Point(30, 120);
            radioAuto.Width = 400;
            radioAuto.WrapText = true;
            radioAuto.AutoHeight = true;
            radioAuto.BackColor = Color.LightGreen;
            this.Controls.Add(radioAuto);

            var labelAuto = new Label();
            labelAuto.Text = "(AutoHeight = true)";
            labelAuto.Location = new Point(30, 160);
            labelAuto.ForeColor = Color.Gray;
            labelAuto.Font = new Font("Arial", 8F, FontStyle.Italic);
            this.Controls.Add(labelAuto);

            // Button to change text
            btnChangeText = new Button();
            btnChangeText.Text = "Add More Text";
            btnChangeText.Location = new Point(30, 220);
            btnChangeText.Size = new Size(150, 40);
            btnChangeText.Click += BtnChangeText_Click;
            this.Controls.Add(btnChangeText);
        }

        private void BtnChangeText_Click(object sender, EventArgs e)
        {
            string longText = "This is a much longer text that demonstrates how AutoHeight works. " +
                            "When AutoHeight is enabled, the control automatically expands to fit all " +
                            "the content. When disabled, the text may be clipped or truncated.";

            radioFixed.Text = longText;
            radioAuto.Text = longText;
        }
    }
}
```

## DrawFocusRectangle Property

The `DrawFocusRectangle` property controls whether a focus rectangle is drawn when the control receives keyboard focus.

### Property Details

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `DrawFocusRectangle` | bool | Shows or hides the focus rectangle | True |

### Usage

**C#:**
```csharp
// Show focus rectangle (default)
this.radioButtonAdv1.DrawFocusRectangle = true;
this.radioButtonAdv1.Text = "With Focus Rectangle";

// Hide focus rectangle
this.radioButtonAdv2.DrawFocusRectangle = false;
this.radioButtonAdv2.Text = "Without Focus Rectangle";
```

**VB.NET:**
```vb
' Show focus rectangle (default)
Me.radioButtonAdv1.DrawFocusRectangle = True
Me.radioButtonAdv1.Text = "With Focus Rectangle"

' Hide focus rectangle
Me.radioButtonAdv2.DrawFocusRectangle = False
Me.radioButtonAdv2.Text = "Without Focus Rectangle"
```

### When to Use

**Enable Focus Rectangle When:**
- Accessibility is a priority
- Users navigate primarily with keyboard
- The application is keyboard-driven

**Disable Focus Rectangle When:**
- Creating a clean, minimalist UI
- Mouse interaction is primary
- Custom focus indicators are implemented

### Focus Rectangle Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace FocusRectangleDemo
{
    public partial class FocusForm : Form
    {
        public FocusForm()
        {
            InitializeComponent();
            DemonstrateFocusRectangle();
        }

        private void DemonstrateFocusRectangle()
        {
            this.Text = "Focus Rectangle Demo";
            this.Size = new Size(450, 300);

            var label1 = new Label();
            label1.Text = "Tab through these options to see the focus rectangle:";
            label1.Location = new Point(30, 20);
            label1.Size = new Size(380, 20);
            this.Controls.Add(label1);

            // With focus rectangle
            var radio1 = new RadioButtonAdv();
            radio1.Text = "Option 1 (Focus Rectangle Visible)";
            radio1.Location = new Point(30, 60);
            radio1.Size = new Size(350, 30);
            radio1.DrawFocusRectangle = true;
            radio1.TabIndex = 0;
            this.Controls.Add(radio1);

            var radio2 = new RadioButtonAdv();
            radio2.Text = "Option 2 (Focus Rectangle Visible)";
            radio2.Location = new Point(30, 100);
            radio2.Size = new Size(350, 30);
            radio2.DrawFocusRectangle = true;
            radio2.TabIndex = 1;
            this.Controls.Add(radio2);

            // Without focus rectangle
            var radio3 = new RadioButtonAdv();
            radio3.Text = "Option 3 (Focus Rectangle Hidden)";
            radio3.Location = new Point(30, 150);
            radio3.Size = new Size(350, 30);
            radio3.DrawFocusRectangle = false;
            radio3.TabIndex = 2;
            this.Controls.Add(radio3);

            var radio4 = new RadioButtonAdv();
            radio4.Text = "Option 4 (Focus Rectangle Hidden)";
            radio4.Location = new Point(30, 190);
            radio4.Size = new Size(350, 30);
            radio4.DrawFocusRectangle = false;
            radio4.TabIndex = 3;
            this.Controls.Add(radio4);
        }
    }
}
```

## RaiseEventOnClick Property

The `RaiseEventOnClick` property controls whether the Click event is fired when the control is clicked.

### Property Details

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `RaiseEventOnClick` | bool | Specifies whether OnClick event should fire | True |

### Usage

**C#:**
```csharp
// Enable Click event (default)
this.radioButtonAdv1.RaiseEventOnClick = true;
this.radioButtonAdv1.Click += RadioButtonAdv1_Click;

// Disable Click event
this.radioButtonAdv2.RaiseEventOnClick = false;
this.radioButtonAdv2.Click += RadioButtonAdv2_Click; // This won't fire
```

**VB.NET:**
```vb
' Enable Click event (default)
Me.radioButtonAdv1.RaiseEventOnClick = True
AddHandler radioButtonAdv1.Click, AddressOf RadioButtonAdv1_Click

' Disable Click event
Me.radioButtonAdv2.RaiseEventOnClick = False
AddHandler radioButtonAdv2.Click, AddressOf RadioButtonAdv2_Click ' This won't fire
```

### Event Control Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace EventControlDemo
{
    public partial class EventForm : Form
    {
        private RadioButtonAdv radioWithEvent;
        private RadioButtonAdv radioWithoutEvent;
        private Label lblStatus;
        private int clickCount = 0;

        public EventForm()
        {
            InitializeComponent();
            InitializeEventDemo();
        }

        private void InitializeEventDemo()
        {
            this.Text = "RaiseEventOnClick Demo";
            this.Size = new Size(450, 250);

            // Status label
            lblStatus = new Label();
            lblStatus.Text = "Click count: 0";
            lblStatus.Location = new Point(30, 20);
            lblStatus.Size = new Size(350, 30);
            lblStatus.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            lblStatus.ForeColor = Color.Navy;
            this.Controls.Add(lblStatus);

            // Radio button with event enabled
            radioWithEvent = new RadioButtonAdv();
            radioWithEvent.Text = "Click Event Enabled";
            radioWithEvent.Location = new Point(30, 70);
            radioWithEvent.Size = new Size(350, 30);
            radioWithEvent.RaiseEventOnClick = true;
            radioWithEvent.Click += Radio_Click;
            this.Controls.Add(radioWithEvent);

            // Radio button with event disabled
            radioWithoutEvent = new RadioButtonAdv();
            radioWithoutEvent.Text = "Click Event Disabled (Won't increment counter)";
            radioWithoutEvent.Location = new Point(30, 120);
            radioWithoutEvent.Size = new Size(350, 30);
            radioWithoutEvent.RaiseEventOnClick = false;
            radioWithoutEvent.Click += Radio_Click;
            this.Controls.Add(radioWithoutEvent);

            var btnReset = new Button();
            btnReset.Text = "Reset Counter";
            btnReset.Location = new Point(30, 170);
            btnReset.Size = new Size(120, 30);
            btnReset.Click += (s, e) => {
                clickCount = 0;
                UpdateStatus();
            };
            this.Controls.Add(btnReset);
        }

        private void Radio_Click(object sender, EventArgs e)
        {
            clickCount++;
            UpdateStatus();
        }

        private void UpdateStatus()
        {
            lblStatus.Text = $"Click count: {clickCount}";
        }
    }
}
```

## RadioButtonAdv State Values

RadioButtonAdv allows you to associate integer and string values with different check states, making it easy to retrieve meaningful data based on the radio button's state.

### Value Properties

| Property | Type | Description |
|----------|------|-------------|
| `CheckedInt` | int | Integer value when checked |
| `CheckedString` | string | String value when checked |
| `UncheckedInt` | int | Integer value when unchecked |
| `UncheckedString` | string | String value when unchecked |
| `IntValue` | int | Gets/sets checked RadioButtonAdv by TabIndex |

### Basic Value Association

**C#:**
```csharp
// Associate values with radio button states
this.radioButtonAdv1.CheckedInt = 100;
this.radioButtonAdv1.CheckedString = "Premium Plan Selected";
this.radioButtonAdv1.UncheckedInt = 0;
this.radioButtonAdv1.UncheckedString = "Premium Plan Not Selected";

// Retrieve values based on state
if (radioButtonAdv1.Checked)
{
    int value = radioButtonAdv1.CheckedInt;
    string message = radioButtonAdv1.CheckedString;
    MessageBox.Show($"{message} - Value: {value}");
}
```

**VB.NET:**
```vb
' Associate values with radio button states
Me.radioButtonAdv1.CheckedInt = 100
Me.radioButtonAdv1.CheckedString = "Premium Plan Selected"
Me.radioButtonAdv1.UncheckedInt = 0
Me.radioButtonAdv1.UncheckedString = "Premium Plan Not Selected"

' Retrieve values based on state
If radioButtonAdv1.Checked Then
    Dim value As Integer = radioButtonAdv1.CheckedInt
    Dim message As String = radioButtonAdv1.CheckedString
    MessageBox.Show($"{message} - Value: {value}")
End If
```

### Complete Value Association Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ValueAssociationDemo
{
    public partial class ValueForm : Form
    {
        private RadioButtonAdv radioBasic;
        private RadioButtonAdv radioPro;
        private RadioButtonAdv radioEnterprise;
        private Button btnGetValue;
        private Label lblResult;

        public ValueForm()
        {
            InitializeComponent();
            InitializeValueDemo();
        }

        private void InitializeValueDemo()
        {
            this.Text = "State Value Association Demo";
            this.Size = new Size(500, 350);

            var label = new Label();
            label.Text = "Select a subscription plan:";
            label.Location = new Point(30, 20);
            label.Size = new Size(400, 25);
            label.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            this.Controls.Add(label);

            // Basic plan - $9.99
            radioBasic = new RadioButtonAdv();
            radioBasic.Text = "Basic Plan - $9.99/month";
            radioBasic.Location = new Point(30, 60);
            radioBasic.Size = new Size(400, 30);
            radioBasic.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioBasic.Checked = true;
            radioBasic.CheckedInt = 999;
            radioBasic.CheckedString = "Basic Plan";
            radioBasic.UncheckedInt = 0;
            radioBasic.UncheckedString = "Not Selected";
            this.Controls.Add(radioBasic);

            // Professional plan - $19.99
            radioPro = new RadioButtonAdv();
            radioPro.Text = "Professional Plan - $19.99/month";
            radioPro.Location = new Point(30, 110);
            radioPro.Size = new Size(400, 30);
            radioPro.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioPro.CheckedInt = 1999;
            radioPro.CheckedString = "Professional Plan";
            radioPro.UncheckedInt = 0;
            radioPro.UncheckedString = "Not Selected";
            this.Controls.Add(radioPro);

            // Enterprise plan - $49.99
            radioEnterprise = new RadioButtonAdv();
            radioEnterprise.Text = "Enterprise Plan - $49.99/month";
            radioEnterprise.Location = new Point(30, 160);
            radioEnterprise.Size = new Size(400, 30);
            radioEnterprise.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioEnterprise.CheckedInt = 4999;
            radioEnterprise.CheckedString = "Enterprise Plan";
            radioEnterprise.UncheckedInt = 0;
            radioEnterprise.UncheckedString = "Not Selected";
            this.Controls.Add(radioEnterprise);

            // Get value button
            btnGetValue = new Button();
            btnGetValue.Text = "Get Selected Value";
            btnGetValue.Location = new Point(30, 220);
            btnGetValue.Size = new Size(150, 35);
            btnGetValue.Click += BtnGetValue_Click;
            this.Controls.Add(btnGetValue);

            // Result label
            lblResult = new Label();
            lblResult.Location = new Point(200, 220);
            lblResult.Size = new Size(270, 60);
            lblResult.Font = new Font("Segoe UI", 10F);
            lblResult.ForeColor = Color.DarkGreen;
            this.Controls.Add(lblResult);
        }

        private void BtnGetValue_Click(object sender, EventArgs e)
        {
            string selectedPlan = "";
            int priceInCents = 0;

            if (radioBasic.Checked)
            {
                selectedPlan = radioBasic.CheckedString;
                priceInCents = radioBasic.CheckedInt;
            }
            else if (radioPro.Checked)
            {
                selectedPlan = radioPro.CheckedString;
                priceInCents = radioPro.CheckedInt;
            }
            else if (radioEnterprise.Checked)
            {
                selectedPlan = radioEnterprise.CheckedString;
                priceInCents = radioEnterprise.CheckedInt;
            }

            decimal price = priceInCents / 100.0m;
            lblResult.Text = $"Selected: {selectedPlan}\n" +
                           $"Price: ${price:F2}/month\n" +
                           $"Annual: ${price * 12:F2}";
        }
    }
}
```

## Best Practices

### AutoHeight Usage

1. **Always set Width**: When using AutoHeight, explicitly set the Width property
2. **Combine with WrapText**: AutoHeight works best with WrapText enabled
3. **Consider Layout**: AutoHeight can affect form layout; use anchoring or docking
4. **Performance**: Recalculation happens automatically but can impact performance with many controls

### Focus Rectangle Guidelines

1. **Accessibility First**: Keep focus rectangles enabled for accessible applications
2. **Keyboard Navigation**: Essential for users who navigate with Tab key
3. **Visual Consistency**: If disabling, ensure alternative focus indicators exist
4. **Testing**: Test with actual keyboard-only navigation

### Event Control

1. **Use CheckedChanged**: For state changes, prefer CheckedChanged over Click
2. **RaiseEventOnClick**: Only disable if you have specific reasons
3. **Event Handling**: Be aware that disabling Click doesn't affect CheckedChanged

### Value Association Tips

1. **Use Meaningful Values**: Choose integer values that relate to the option (e.g., price in cents)
2. **Consistent Units**: Keep units consistent across radio buttons (all cents, all IDs, etc.)
3. **String Values**: Use for display messages or database keys
4. **Null Checks**: Always check if radio button is checked before accessing values

## Behavior Configuration Example

Here's a comprehensive example combining all behavior settings:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ComprehensiveBehaviorDemo
{
    public partial class BehaviorForm : Form
    {
        public BehaviorForm()
        {
            InitializeComponent();
            ConfigureRadioButtons();
        }

        private void ConfigureRadioButtons()
        {
            this.Text = "Comprehensive Behavior Settings";
            this.Size = new Size(550, 400);

            // Radio 1: All features enabled
            var radio1 = new RadioButtonAdv();
            radio1.Text = "Full Features: AutoHeight, Focus Rectangle, Click Events, Values";
            radio1.Location = new Point(30, 30);
            radio1.Width = 450;
            radio1.AutoHeight = true;
            radio1.WrapText = true;
            radio1.DrawFocusRectangle = true;
            radio1.RaiseEventOnClick = true;
            radio1.CheckedInt = 1;
            radio1.CheckedString = "Option 1 Selected";
            radio1.Style = RadioButtonAdvStyle.Office2016Colorful;
            radio1.Click += (s, e) => ShowMessage("Radio 1 clicked");
            this.Controls.Add(radio1);

            // Radio 2: Minimal features
            var radio2 = new RadioButtonAdv();
            radio2.Text = "Minimal: No AutoHeight, No Focus Rectangle, No Click Events";
            radio2.Location = new Point(30, 120);
            radio2.Size = new Size(450, 30);
            radio2.AutoHeight = false;
            radio2.DrawFocusRectangle = false;
            radio2.RaiseEventOnClick = false;
            radio2.Style = RadioButtonAdvStyle.Office2016White;
            radio2.Click += (s, e) => ShowMessage("This won't show");
            this.Controls.Add(radio2);

            // Radio 3: Custom configuration
            var radio3 = new RadioButtonAdv();
            radio3.Text = "Custom: AutoHeight + Values, No Focus Rectangle";
            radio3.Location = new Point(30, 200);
            radio3.Width = 450;
            radio3.AutoHeight = true;
            radio3.WrapText = true;
            radio3.DrawFocusRectangle = false;
            radio3.RaiseEventOnClick = true;
            radio3.CheckedInt = 3;
            radio3.CheckedString = "Custom Option Selected";
            radio3.Style = RadioButtonAdvStyle.Metro;
            radio3.CheckedChanged += Radio3_CheckedChanged;
            this.Controls.Add(radio3);
        }

        private void Radio3_CheckedChanged(object sender, EventArgs e)
        {
            var radio = sender as RadioButtonAdv;
            if (radio != null && radio.Checked)
            {
                ShowMessage($"{radio.CheckedString} (Value: {radio.CheckedInt})");
            }
        }

        private void ShowMessage(string message)
        {
            MessageBox.Show(message, "Behavior Demo", 
                          MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
}
```

## Troubleshooting

### AutoHeight Not Working

If AutoHeight isn't adjusting the control:
1. Verify `AutoHeight = true`
2. Ensure `WrapText = true` if dealing with long text
3. Check that Width is explicitly set
4. Verify text actually requires multiple lines

### Focus Rectangle Not Visible

If focus rectangle isn't showing when expected:
1. Confirm `DrawFocusRectangle = true`
2. Tab to the control to give it focus
3. Check if custom themes override focus rendering
4. Verify control actually has keyboard focus

### Click Event Not Firing

If Click event doesn't fire:
1. Verify `RaiseEventOnClick = true`
2. Check that event handler is properly attached
3. Ensure control is enabled
4. Use CheckedChanged as an alternative for state changes
