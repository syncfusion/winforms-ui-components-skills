# CheckBoxAdv Events

This guide covers event handling for the CheckBoxAdv control, including state change events and best practices.

## Available Events

The CheckBoxAdv provides two primary events for monitoring state changes:

| Event | Description | When Raised |
|-------|-------------|-------------|
| CheckStateChanged | Fires when CheckState property changes | On any state transition |
| CheckedChanged | Fires when Checked property changes | When state becomes checked/unchecked |

## CheckStateChanged Event

The `CheckStateChanged` event fires whenever the `CheckState` property changes, including transitions to and from the indeterminate state.

### Event Signature

```csharp
public event EventHandler CheckStateChanged;
```

### Subscribing to the Event

```csharp
// Subscribe to the event
checkBoxAdv1.CheckStateChanged += CheckBoxAdv1_CheckStateChanged;

// Event handler
private void CheckBoxAdv1_CheckStateChanged(object sender, EventArgs e)
{
    CheckBoxAdv checkBox = sender as CheckBoxAdv;
    Console.WriteLine($"CheckState changed to: {checkBox.CheckState}");
}
```

```vb
' Subscribe to the event
AddHandler checkBoxAdv1.CheckStateChanged, AddressOf CheckBoxAdv1_CheckStateChanged

' Event handler
Private Sub CheckBoxAdv1_CheckStateChanged(sender As Object, e As EventArgs)
    Dim checkBox As CheckBoxAdv = TryCast(sender, CheckBoxAdv)
    Console.WriteLine($"CheckState changed to: {checkBox.CheckState}")
End Sub
```

### Using Lambda Expression

```csharp
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    CheckBoxAdv cb = (CheckBoxAdv)sender;
    MessageBox.Show($"State is now: {cb.CheckState}");
};
```

### Detecting All Three States

```csharp
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    CheckBoxAdv cb = (CheckBoxAdv)sender;
    
    switch (cb.CheckState)
    {
        case CheckState.Checked:
            Console.WriteLine("Checkbox is checked");
            break;
            
        case CheckState.Unchecked:
            Console.WriteLine("Checkbox is unchecked");
            break;
            
        case CheckState.Indeterminate:
            Console.WriteLine("Checkbox is indeterminate");
            break;
    }
};
```

### Practical Example: Enabling/Disabling Controls

```csharp
CheckBoxAdv enableFeaturesCheckBox = new CheckBoxAdv();
enableFeaturesCheckBox.Text = "Enable Advanced Features";
enableFeaturesCheckBox.Tristate = true;

enableFeaturesCheckBox.CheckStateChanged += (sender, e) =>
{
    switch (enableFeaturesCheckBox.CheckState)
    {
        case CheckState.Checked:
            // Enable all features
            textBox1.Enabled = true;
            button1.Enabled = true;
            comboBox1.Enabled = true;
            break;
            
        case CheckState.Unchecked:
            // Disable all features
            textBox1.Enabled = false;
            button1.Enabled = false;
            comboBox1.Enabled = false;
            break;
            
        case CheckState.Indeterminate:
            // Enable some features
            textBox1.Enabled = true;
            button1.Enabled = false;
            comboBox1.Enabled = true;
            break;
    }
};
```

## CheckedChanged Event

The `CheckedChanged` event fires when the `Checked` property changes. Note that the indeterminate state is treated as "checked" for this event.

### Event Signature

```csharp
public event EventHandler CheckedChanged;
```

### Subscribing to the Event

```csharp
// Subscribe to the event
checkBoxAdv1.CheckedChanged += CheckBoxAdv1_CheckedChanged;

// Event handler
private void CheckBoxAdv1_CheckedChanged(object sender, EventArgs e)
{
    CheckBoxAdv checkBox = sender as CheckBoxAdv;
    
    if (checkBox.Checked)
    {
        MessageBox.Show("Checkbox is checked");
    }
    else
    {
        MessageBox.Show("Checkbox is unchecked");
    }
}
```

```vb
' Subscribe to the event
AddHandler checkBoxAdv1.CheckedChanged, AddressOf CheckBoxAdv1_CheckedChanged

' Event handler
Private Sub CheckBoxAdv1_CheckedChanged(sender As Object, e As EventArgs)
    Dim checkBox As CheckBoxAdv = TryCast(sender, CheckBoxAdv)
    
    If checkBox.Checked Then
        MessageBox.Show("Checkbox is checked")
    Else
        MessageBox.Show("Checkbox is unchecked")
    End If
End Sub
```

### Using Lambda Expression

```csharp
checkBoxAdv1.CheckedChanged += (sender, e) =>
{
    if (!checkBoxAdv1.Checked)
        Console.WriteLine("Unchecked");
    else
        Console.WriteLine("Checked or Indeterminate");
};
```

### Important Behavior Note

When the checkbox is in the **Indeterminate** state:
- `Checked` property returns **true**
- `CheckedChanged` event fires when transitioning to/from indeterminate

Example behavior:
```csharp
checkBoxAdv1.CheckState = CheckState.Indeterminate;
Console.WriteLine(checkBoxAdv1.Checked); // Output: True
```

## Event Timing and Order

### When Both Events Fire

Both events fire when the checkbox state changes, but in a specific order:

1. **CheckStateChanged** fires first
2. **CheckedChanged** fires second (if Checked property actually changed)

```csharp
checkBoxAdv1.CheckStateChanged += (s, e) =>
{
    Console.WriteLine("1. CheckStateChanged fired");
};

checkBoxAdv1.CheckedChanged += (s, e) =>
{
    Console.WriteLine("2. CheckedChanged fired");
};

// Output when clicking:
// 1. CheckStateChanged fired
// 2. CheckedChanged fired
```

### When Only CheckStateChanged Fires

```csharp
// Transition from Checked to Indeterminate
checkBoxAdv1.CheckState = CheckState.Checked;
checkBoxAdv1.CheckState = CheckState.Indeterminate;
// CheckStateChanged fires
// CheckedChanged does NOT fire (both states have Checked = true)
```

## Common Event Patterns

### Pattern 1: Validation on State Change

```csharp
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    if (checkBoxAdv1.CheckState == CheckState.Indeterminate)
    {
        MessageBox.Show("Please select a definite option (Yes or No).");
        checkBoxAdv1.CheckState = CheckState.Unchecked;
    }
};
```

### Pattern 2: Cascading Checkboxes

```csharp
CheckBoxAdv parentCheckBox = new CheckBoxAdv();
parentCheckBox.Text = "Select All";
parentCheckBox.Tristate = true;

List<CheckBox> childCheckBoxes = new List<CheckBox>
{
    checkBox1, checkBox2, checkBox3
};

// Parent controls children
parentCheckBox.CheckStateChanged += (sender, e) =>
{
    if (parentCheckBox.CheckState != CheckState.Indeterminate)
    {
        bool isChecked = (parentCheckBox.CheckState == CheckState.Checked);
        foreach (var child in childCheckBoxes)
        {
            child.Checked = isChecked;
        }
    }
};

// Children update parent
foreach (var child in childCheckBoxes)
{
    child.CheckedChanged += (sender, e) =>
    {
        UpdateParentState();
    };
}

void UpdateParentState()
{
    int checkedCount = childCheckBoxes.Count(cb => cb.Checked);
    
    if (checkedCount == 0)
        parentCheckBox.CheckState = CheckState.Unchecked;
    else if (checkedCount == childCheckBoxes.Count)
        parentCheckBox.CheckState = CheckState.Checked;
    else
        parentCheckBox.CheckState = CheckState.Indeterminate;
}
```

### Pattern 3: Logging State Changes

```csharp
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    string timestamp = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
    string logEntry = $"[{timestamp}] CheckBox state changed to: {checkBoxAdv1.CheckState}";
    
    LogToFile(logEntry);
    Console.WriteLine(logEntry);
};
```

### Pattern 4: Conditional Actions

```csharp
checkBoxAdv1.CheckedChanged += (sender, e) =>
{
    if (checkBoxAdv1.Checked)
    {
        // Perform action when checked or indeterminate
        EnableAdvancedMode();
        ShowAdvancedPanel();
    }
    else
    {
        // Perform action when unchecked
        DisableAdvancedMode();
        HideAdvancedPanel();
    }
};
```

### Pattern 5: Confirmation Dialog

```csharp
private CheckState previousState = CheckState.Unchecked;

checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    if (checkBoxAdv1.CheckState == CheckState.Checked)
    {
        DialogResult result = MessageBox.Show(
            "Are you sure you want to enable this feature?",
            "Confirm",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question
        );
        
        if (result == DialogResult.No)
        {
            // Revert to previous state
            checkBoxAdv1.CheckState = previousState;
            return;
        }
    }
    
    // Update previous state
    previousState = checkBoxAdv1.CheckState;
};
```

### Pattern 6: Debouncing Rapid Changes

```csharp
private System.Threading.Timer debounceTimer;

checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    // Cancel previous timer
    debounceTimer?.Dispose();
    
    // Create new timer that fires after 500ms
    debounceTimer = new System.Threading.Timer((state) =>
    {
        // This runs only if no changes occur for 500ms
        this.Invoke((MethodInvoker)delegate
        {
            ProcessCheckBoxChange();
        });
    }, null, 500, System.Threading.Timeout.Infinite);
};
```

## Best Practices

### 1. Choose the Right Event

**Use CheckStateChanged when:**
- You need to distinguish between all three states
- Working with Tristate checkboxes
- Implementing "Select All" functionality

**Use CheckedChanged when:**
- You only care about checked vs unchecked
- Simple boolean logic is sufficient
- Indeterminate state is not relevant

### 2. Avoid Infinite Loops

Be careful when modifying the checkbox state within its own event handler:

```csharp
// WRONG - Can cause infinite loop
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    checkBoxAdv1.CheckState = CheckState.Checked; // May loop forever
};

// CORRECT - Use a flag to prevent recursion
private bool isUpdating = false;

checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    if (isUpdating) return;
    
    isUpdating = true;
    // Safe to modify state here
    checkBoxAdv1.CheckState = CheckState.Checked;
    isUpdating = false;
};
```

### 3. Handle Exceptions

```csharp
checkBoxAdv1.CheckStateChanged += (sender, e) =>
{
    try
    {
        // Your logic here
        ProcessStateChange();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error processing state change: {ex.Message}");
        // Optionally revert state
        checkBoxAdv1.CheckState = previousState;
    }
};
```

### 4. Unsubscribe When Done

```csharp
// Store the handler
EventHandler handler = null;
handler = (sender, e) =>
{
    // Do work
    ProcessCheckBox();
    
    // Unsubscribe after first use
    checkBoxAdv1.CheckStateChanged -= handler;
};

checkBoxAdv1.CheckStateChanged += handler;
```

### 5. Use Sender Parameter

```csharp
// Good - works with multiple checkboxes
private void SharedCheckBoxHandler(object sender, EventArgs e)
{
    CheckBoxAdv cb = (CheckBoxAdv)sender;
    Console.WriteLine($"{cb.Text}: {cb.CheckState}");
}

checkBoxAdv1.CheckStateChanged += SharedCheckBoxHandler;
checkBoxAdv2.CheckStateChanged += SharedCheckBoxHandler;
checkBoxAdv3.CheckStateChanged += SharedCheckBoxHandler;
```

## Troubleshooting

### Event Not Firing

**Possible causes:**
1. Event not subscribed correctly
2. ReadOnlyMode is enabled (events still fire, but state doesn't change from UI)
3. Programmatic changes in a loop preventing UI updates

### Event Fires Multiple Times

**Possible causes:**
1. Event subscribed multiple times (check InitializeComponent)
2. Cascading updates between related checkboxes
3. Data binding causing multiple updates

**Solution:**
```csharp
// Unsubscribe before subscribing
checkBoxAdv1.CheckStateChanged -= Handler;
checkBoxAdv1.CheckStateChanged += Handler;
```
