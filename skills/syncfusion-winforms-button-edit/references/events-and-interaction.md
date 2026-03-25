# Events & Interaction

## Table of Contents
- [ButtonClicked Event](#buttonclicked-event)
- [Border Change Events](#border-change-events)
- [Child Button Events](#child-button-events)
- [Mouse Events](#mouse-events)
- [Real-World Example: Date Picker](#real-world-example-date-picker)

## ButtonClicked Event

The ButtonClicked event fires when any child button is clicked. It provides access to which button was clicked.

### Basic Implementation

```csharp
buttonEdit.ButtonClicked += ButtonEdit_ButtonClicked;

private void ButtonEdit_ButtonClicked(object sender, ButtonClickedEventArgs args)
{
    // args.ClickedButton contains the ButtonEditChildButton that was clicked
    MessageBox.Show("Button clicked: " + args.ClickedButton.Text);
}
```

### Identifying Which Button

```csharp
ButtonEditChildButton btn1 = new ButtonEditChildButton() { Text = "Browse" };
ButtonEditChildButton btn2 = new ButtonEditChildButton() { Text = "Clear" };

buttonEdit.Buttons.Add(btn1);
buttonEdit.Buttons.Add(btn2);

buttonEdit.ButtonClicked += (s, e) => 
{
    if (e.ClickedButton == btn1)
    {
        // Handle browse button
        BrowseForFile();
    }
    else if (e.ClickedButton == btn2)
    {
        // Handle clear button
        buttonEdit.TextBox.Text = "";
    }
};
```

### Using Button Index

```csharp
buttonEdit.ButtonClicked += (s, e) => 
{
    int buttonIndex = buttonEdit.Buttons.IndexOf(e.ClickedButton);
    
    switch (buttonIndex)
    {
        case 0:
            // First button clicked
            break;
        case 1:
            // Second button clicked
            break;
    }
};
```

## Border Change Events

Events fire when border properties change.

### Border3DStyleChanged Event

```csharp
buttonEdit.Border3DStyleChanged += (s, e) => 
{
    Console.WriteLine("Border 3D style changed to: " + buttonEdit.Border3DStyle);
};

// Trigger the event
buttonEdit.Border3DStyle = Border3DStyle.Raised;
```

### BorderSidesChanged Event

```csharp
buttonEdit.BorderSidesChanged += (s, e) => 
{
    Console.WriteLine("Border sides changed to: " + buttonEdit.BorderSides);
};

// Trigger the event
buttonEdit.BorderSides = BorderSides.All;
```

### Complete Border Event Example

```csharp
public Form1()
{
    InitializeComponent();
    
    buttonEdit.Border3DStyleChanged += ButtonEdit_Border3DStyleChanged;
    buttonEdit.BorderSidesChanged += ButtonEdit_BorderSidesChanged;
}

private void ButtonEdit_Border3DStyleChanged(object sender, EventArgs e)
{
    statusLabel.Text = "Border 3D style changed";
}

private void ButtonEdit_BorderSidesChanged(object sender, EventArgs e)
{
    statusLabel.Text = "Border sides changed";
}
```

## Child Button Events

Individual child buttons support various events for interaction.

### Click Event

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton() { Text = "Action" };

btn.Click += (s, e) => 
{
    // Handle button click
    MessageBox.Show("Button clicked!");
};

buttonEdit.Buttons.Add(btn);
```

### TextChanged Event

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

btn.TextChanged += (s, e) => 
{
    Console.WriteLine("Button text changed to: " + btn.Text);
};

buttonEdit.Buttons.Add(btn);
```

### BackColorChanged Event

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

btn.BackColorChanged += (s, e) => 
{
    Console.WriteLine("Background color changed");
};

buttonEdit.Buttons.Add(btn);
```

## Mouse Events

Handle mouse interactions on child buttons.

### MouseEnter & MouseLeave

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

btn.MouseEnter += (s, e) => 
{
    btn.BackColor = Color.LightBlue;  // Highlight on hover
};

btn.MouseLeave += (s, e) => 
{
    btn.BackColor = SystemColors.Control;  // Reset
};

buttonEdit.Buttons.Add(btn);
```

### MouseDown & MouseUp

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

btn.MouseDown += (s, e) => 
{
    if (e.Button == MouseButtons.Right)
    {
        // Right-click pressed
    }
};

btn.MouseUp += (s, e) => 
{
    if (e.Button == MouseButtons.Left)
    {
        // Left-click released
    }
};

buttonEdit.Buttons.Add(btn);
```

### MouseHover

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

btn.MouseHover += (s, e) => 
{
    toolTip.Show("Click to perform action", btn);
};

buttonEdit.Buttons.Add(btn);
```

## Real-World Example: Date Picker

Create a date picker using ButtonEdit with a calendar popup.

### Complete Implementation

```csharp
public partial class DatePickerForm : Form
{
    private ButtonEdit dateEdit;
    private CalendarPopup calendarPopup;
    private MonthCalendarAdv monthCalendar;
    private ButtonEditChildButton calendarBtn;

    public DatePickerForm()
    {
        InitializeComponent();
        SetupDatePicker();
    }

    private void SetupDatePicker()
    {
        // Create ButtonEdit
        dateEdit = new ButtonEdit();
        dateEdit.Location = new Point(50, 50);
        dateEdit.Size = new Size(200, 21);
        dateEdit.TextBox.ReadOnly = true;  // Date picker, not editable
        
        // Create calendar button
        calendarBtn = new ButtonEditChildButton();
        calendarBtn.Text = "📅";  // Calendar emoji or icon
        calendarBtn.ButtonAlign = ButtonAlignment.Right;
        calendarBtn.PreferredWidth = 30;
        
        dateEdit.Buttons.Add(calendarBtn);
        
        // Create calendar popup
        monthCalendar = new MonthCalendarAdv();
        monthCalendar.DateSelected += MonthCalendar_DateSelected;
        
        calendarPopup = new CalendarPopup();
        calendarPopup.AutoSize = false;
        calendarPopup.Visible = false;
        calendarPopup.Size = new Size(250, 200);
        calendarPopup.Controls.Add(monthCalendar);
        
        // Handle calendar button click
        dateEdit.ButtonClicked += (s, e) => 
        {
            if (e.ClickedButton == calendarBtn)
            {
                calendarPopup.Visible = !calendarPopup.Visible;
            }
        };
        
        // Add controls to form
        this.Controls.Add(dateEdit);
        this.Controls.Add(calendarPopup);
    }

    private void MonthCalendar_DateSelected(object sender, EventArgs e)
    {
        // Set selected date in textbox
        dateEdit.TextBox.Text = monthCalendar.Value.ToString("yyyy-MM-dd");
        calendarPopup.Visible = false;
    }
}
```

### Simplified Date Picker

```csharp
ButtonEdit dateEdit = new ButtonEdit();
dateEdit.Size = new Size(200, 21);
dateEdit.TextBox.ReadOnly = true;

ButtonEditChildButton dateBtn = new ButtonEditChildButton();
dateBtn.Text = "...";
dateEdit.Buttons.Add(dateBtn);

dateEdit.ButtonClicked += (s, e) => 
{
    using (var dialog = new DatePickerDialog())
    {
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            dateEdit.TextBox.Text = dialog.SelectedDate.ToString("yyyy-MM-dd");
        }
    }
};
```

### Expandable Calendar Popup

```csharp
public void CreateExpandableCalendar()
{
    // Create ButtonEdit in first row
    ButtonEdit dateEdit = new ButtonEdit();
    dateEdit.TextBox.ReadOnly = true;
    
    ButtonEditChildButton calBtn = new ButtonEditChildButton();
    calBtn.Text = "▼";
    dateEdit.Buttons.Add(calBtn);
    
    // Create calendar in second row
    MonthCalendarAdv calendar = new MonthCalendarAdv();
    
    // Toggle visibility
    dateEdit.ButtonClicked += (s, e) => 
    {
        calendar.Visible = !calendar.Visible;
    };
    
    calendar.DateSelected += (s, e) => 
    {
        dateEdit.TextBox.Text = calendar.Value.ToString("MM/dd/yyyy");
    };
}
```

## Complete Event Handling Example

```csharp
public partial class Form1 : Form
{
    private ButtonEdit fileEdit;
    private ButtonEditChildButton browseBtn;
    private ButtonEditChildButton clearBtn;

    public Form1()
    {
        InitializeComponent();
        SetupControls();
    }

    private void SetupControls()
    {
        fileEdit = new ButtonEdit();
        fileEdit.Location = new Point(50, 50);
        fileEdit.Size = new Size(400, 21);
        
        // Browse button - left
        browseBtn = new ButtonEditChildButton();
        browseBtn.Text = "Browse";
        browseBtn.ButtonAlign = ButtonAlignment.Left;
        browseBtn.PreferredWidth = 60;
        
        // Clear button - right
        clearBtn = new ButtonEditChildButton();
        clearBtn.Text = "Clear";
        clearBtn.ButtonAlign = ButtonAlignment.Right;
        clearBtn.PreferredWidth = 50;
        
        fileEdit.Buttons.Add(browseBtn);
        fileEdit.Buttons.Add(clearBtn);
        
        // Main button click event
        fileEdit.ButtonClicked += FileEdit_ButtonClicked;
        
        // Individual button events
        browseBtn.MouseEnter += (s, e) => 
        {
            browseBtn.BackColor = Color.AliceBlue;
        };
        
        browseBtn.MouseLeave += (s, e) => 
        {
            browseBtn.BackColor = SystemColors.Control;
        };
        
        clearBtn.Click += (s, e) => 
        {
            fileEdit.TextBox.Text = "";
            fileEdit.TextBox.Focus();
        };
        
        this.Controls.Add(fileEdit);
    }

    private void FileEdit_ButtonClicked(object sender, ButtonClickedEventArgs e)
    {
        if (e.ClickedButton == browseBtn)
        {
            using (OpenFileDialog dialog = new OpenFileDialog())
            {
                dialog.Title = "Select a file";
                if (dialog.ShowDialog() == DialogResult.OK)
                {
                    fileEdit.TextBox.Text = dialog.FileName;
                }
            }
        }
    }
}
```

## Event Interaction Flow

```
User Action → Event Triggered → Handler Executes → State Changes

1. User clicks button
   ↓
2. ButtonClicked event fires with ButtonClickedEventArgs
   ↓
3. Event handler checks which button
   ↓
4. Appropriate action executed
   ↓
5. Control state updates (text, colors, visibility, etc.)
```

## Best Practices

- Always check `e.ClickedButton` to identify which button was clicked
- Use meaningful button names for readability
- Keep event handlers concise and focused
- Consider using lambdas for simple one-line handlers
- Unsubscribe from events if removing buttons dynamically
- Use try-catch for file operations in button handlers
