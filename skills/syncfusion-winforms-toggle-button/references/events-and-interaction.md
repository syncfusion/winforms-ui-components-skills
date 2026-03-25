# Events and Interaction

Learn how to handle user interactions with the Toggle Button, including click events, state changes, and keyboard interactions.

## Toggle Button Events

The Toggle Button raises several events during interaction:

| Event | Triggered | Purpose |
|-------|-----------|---------|
| `Click` | When button is clicked | Respond to mouse clicks |
| `MouseEnter` | When mouse enters button | Show hover effects |
| `MouseLeave` | When mouse leaves button | Remove hover effects |
| `MouseDown` | When mouse button is pressed | Handle button down |
| `MouseUp` | When mouse button is released | Handle button up |
| `KeyDown` | When key is pressed (has focus) | Handle keyboard input |
| `KeyUp` | When key is released (has focus) | Handle keyboard release |
| `Enter` | When control receives focus | Show focus state |
| `Leave` | When control loses focus | Remove focus state |

## Click Events

### Handling Click Events

The most common event is the `Click` event, which fires when the user clicks the button:

```csharp
// In Form1 constructor or designer-generated code
toggleButton1.Click += ToggleButton1_Click;

private void ToggleButton1_Click(object sender, EventArgs e)
{
    // Handle button click
    if (toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        MessageBox.Show("Toggle is now Active");
    }
    else
    {
        MessageBox.Show("Toggle is now Inactive");
    }
}
```

### Visual Basic Click Handler

```vb
Public Sub New()
    InitializeComponent()
    AddHandler toggleButton1.Click, AddressOf ToggleButton1_Click
End Sub

Private Sub ToggleButton1_Click(sender As Object, e As EventArgs)
    If toggleButton1.ToggleState = ToggleButtonState.Active Then
        MessageBox.Show("Toggle is now Active")
    Else
        MessageBox.Show("Toggle is now Inactive")
    End If
End Sub
```

### Example: Feature Toggle Handler

```csharp
private void EnableDisableFeature_Click(object sender, EventArgs e)
{
    if (toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        // Feature is ON
        EnableFeature();
        statusLabel.Text = "Feature Enabled";
    }
    else
    {
        // Feature is OFF
        DisableFeature();
        statusLabel.Text = "Feature Disabled";
    }
}

private void EnableFeature()
{
    // Enable feature logic
    textBox1.Enabled = true;
    button1.Enabled = true;
}

private void DisableFeature()
{
    // Disable feature logic
    textBox1.Enabled = false;
    button1.Enabled = false;
}
```

## Keyboard Interaction

### Space Key Toggling

When the Toggle Button has keyboard focus, pressing the Space key automatically toggles the state:

```csharp
// Space key toggles automatically (no code needed)
// To detect when space is pressed:

private void toggleButton1_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Space)
    {
        e.Handled = true;
        // Optional: Add additional logic
        OnToggleSpaceKeyPressed();
    }
}

private void OnToggleSpaceKeyPressed()
{
    // Called when user presses Space key
    PlaySound("toggle.wav");
}
```

### Focus Management

To give the button keyboard focus and enable Space key interaction:

```csharp
// Set focus to toggle button
private void Form1_Load(object sender, EventArgs e)
{
    toggleButton1.Focus();
}

// Or on demand
private void button_Click(object sender, EventArgs e)
{
    toggleButton1.Focus();
}
```

### Handling Tab Navigation

```csharp
// Tab key should work automatically
// To handle when user tabs to the button:

private void toggleButton1_Enter(object sender, EventArgs e)
{
    statusLabel.Text = "Toggle button focused. Press Space to toggle.";
}

private void toggleButton1_Leave(object sender, EventArgs e)
{
    statusLabel.Text = "Ready";
}
```

## Mouse Interaction Events

### Click and Double-Click

```csharp
// Single click
toggleButton1.Click += (sender, e) =>
{
    LogToggleAction("Single Click", toggleButton1.ToggleState);
};

// Double-click detection
int clickCount = 0;
private void toggleButton1_Click(object sender, EventArgs e)
{
    clickCount++;
    
    if (clickCount == 2)
    {
        LogToggleAction("Double Click", toggleButton1.ToggleState);
        clickCount = 0;
    }
}

private void LogToggleAction(string action, ToggleButtonState state)
{
    string log = $"{action}: {state}";
    listBox1.Items.Add(log);
}
```

### Mouse Enter and Leave

```csharp
// Hover effects
toggleButton1.MouseEnter += (sender, e) =>
{
    // Change appearance on hover (if not using custom renderer)
    toggleButton1.BackColor = Color.LightBlue;
};

toggleButton1.MouseLeave += (sender, e) =>
{
    // Restore appearance when mouse leaves
    toggleButton1.BackColor = SystemColors.Control;
};
```

### Mouse Down and Up

```csharp
// Detect button press and release
toggleButton1.MouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Left)
    {
        statusLabel.Text = "Toggle button pressed";
    }
};

toggleButton1.MouseUp += (sender, e) =>
{
    if (e.Button == MouseButtons.Left)
    {
        statusLabel.Text = "Toggle button released";
    }
};
```

## State Change Patterns

### Pattern 1: Conditional Logic on Toggle

```csharp
private void toggleButton1_Click(object sender, EventArgs e)
{
    switch (toggleButton1.ToggleState)
    {
        case ToggleButtonState.Active:
            HandleActiveState();
            break;
        case ToggleButtonState.Inactive:
            HandleInactiveState();
            break;
    }
}

private void HandleActiveState()
{
    label1.Text = "Status: Active";
    groupBox1.Enabled = true;
}

private void HandleInactiveState()
{
    label1.Text = "Status: Inactive";
    groupBox1.Enabled = false;
}
```

### Pattern 2: Chained Toggle Buttons

```csharp
private void toggleButton1_Click(object sender, EventArgs e)
{
    // When toggle 1 is activated, deactivate toggle 2
    if (toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        toggleButton2.ToggleState = ToggleButtonState.Inactive;
    }
}

private void toggleButton2_Click(object sender, EventArgs e)
{
    // When toggle 2 is activated, deactivate toggle 1
    if (toggleButton2.ToggleState == ToggleButtonState.Active)
    {
        toggleButton1.ToggleState = ToggleButtonState.Inactive;
    }
}
```

### Pattern 3: State Synchronization

```csharp
private bool _isSynchronizing = false;

private void toggleButton1_Click(object sender, EventArgs e)
{
    if (!_isSynchronizing)
    {
        _isSynchronizing = true;
        
        // Sync other controls to match this toggle
        toggleButton2.ToggleState = toggleButton1.ToggleState;
        toggleButton3.ToggleState = toggleButton1.ToggleState;
        
        _isSynchronizing = false;
    }
}
```

### Pattern 4: Data Binding

```csharp
private class Settings
{
    public bool IsEnabled { get; set; }
}

private Settings _settings = new Settings { IsEnabled = false };

private void Form1_Load(object sender, EventArgs e)
{
    // Load setting into toggle
    toggleButton1.ToggleState = _settings.IsEnabled 
        ? ToggleButtonState.Active 
        : ToggleButtonState.Inactive;
}

private void toggleButton1_Click(object sender, EventArgs e)
{
    // Save toggle state to settings
    _settings.IsEnabled = toggleButton1.ToggleState == ToggleButtonState.Active;
    SaveSettings(_settings);
}

private void SaveSettings(Settings settings)
{
    // Persist settings (file, database, etc.)
}
```

## Event Debouncing

### Prevent Multiple Rapid Toggles

```csharp
private bool _isProcessing = false;
private const int ProcessingDelayMs = 500;

private async void toggleButton1_Click(object sender, EventArgs e)
{
    if (_isProcessing)
        return;
    
    _isProcessing = true;
    
    try
    {
        // Process the toggle
        await ProcessToggleAsync();
    }
    finally
    {
        // Re-enable after delay
        await Task.Delay(ProcessingDelayMs);
        _isProcessing = false;
    }
}

private async Task ProcessToggleAsync()
{
    // Perform async operations
    await Task.Run(() => PerformToggleAction());
}

private void PerformToggleAction()
{
    // Your logic here
}
```

## Multiple Toggle Button Coordination

### Radio-Button Style Behavior

```csharp
private ToggleButton[] _toggleButtons;

public void InitializeToggleGroup()
{
    _toggleButtons = new[] { toggleButton1, toggleButton2, toggleButton3 };
    
    foreach (ToggleButton button in _toggleButtons)
    {
        button.Click += ToggleButton_Click;
    }
}

private void ToggleButton_Click(object sender, EventArgs e)
{
    ToggleButton clickedButton = sender as ToggleButton;
    
    if (clickedButton.ToggleState == ToggleButtonState.Active)
    {
        // Deactivate all others
        foreach (ToggleButton button in _toggleButtons)
        {
            if (button != clickedButton)
            {
                button.ToggleState = ToggleButtonState.Inactive;
            }
        }
    }
}
```

## Form Interaction Events

### Toggle Button with Validation

```csharp
private void toggleButton1_Click(object sender, EventArgs e)
{
    if (toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        if (!ValidateFormData())
        {
            // Revert toggle
            toggleButton1.ToggleState = ToggleButtonState.Inactive;
            MessageBox.Show("Please fix validation errors");
            return;
        }
        
        SaveFormData();
    }
}

private bool ValidateFormData()
{
    if (string.IsNullOrEmpty(textBox1.Text))
        return false;
    
    if (!int.TryParse(textBox2.Text, out _))
        return false;
    
    return true;
}

private void SaveFormData()
{
    // Save data logic
}
```

### Toggle Button with Confirmation

```csharp
private void toggleButton1_Click(object sender, EventArgs e)
{
    if (toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        DialogResult result = MessageBox.Show(
            "Are you sure?",
            "Confirm Action",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.No)
        {
            // Revert toggle
            toggleButton1.ToggleState = ToggleButtonState.Inactive;
        }
    }
}
```

## Best Practices

1. **Check State Explicitly**: Always verify the state when handling events
2. **Provide Feedback**: Give visual or audio feedback on toggle
3. **Avoid Infinite Loops**: Use flags when updating multiple toggles
4. **Validate Before Toggling**: Check conditions before allowing state change
5. **Handle Both Interactions**: Consider mouse and keyboard interactions
6. **Log Important Events**: Track toggle state changes for debugging
7. **Respect User Intent**: Don't force state changes against user action
