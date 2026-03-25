# Events

The Navigation Drawer provides four key events that allow you to respond to drawer state changes during transitions. These events enable you to implement custom logic, perform validation, or update UI elements based on drawer activity.

## Event Overview

| Event | Timing | Cancelable | Description |
|-------|--------|-----------|-------------|
| `Opening` | Before expand | Yes | Fires when expand transition begins |
| `Opened` | After expand | No | Fires when expand transition completes |
| `Closing` | Before collapse | Yes | Fires when collapse transition begins |
| `Closed` | After collapse | No | Fires when collapse transition completes |

## Opening Event

The `Opening` event occurs when the drawer expand transition begins. This event can be used to prepare the drawer content or perform validation before opening.

### Event Declaration

```csharp
// Raises when expand transition begins
public event OpeningEventHandler Opening;
```

### Hooking the Opening Event

```csharp
// Subscribe to Opening event
navigationDrawer1.Opening += NavigationDrawer1_Opening;

private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    MessageBox.Show("Drawer is opening...");
}
```

**VB.NET:**
```vb
' Subscribe to Opening event
AddHandler navigationDrawer1.Opening, AddressOf NavigationDrawer1_Opening

Private Sub NavigationDrawer1_Opening(ByVal sender As Object, ByVal e As OpeningEventArgs)
    MessageBox.Show("Drawer is opening...")
End Sub
```

### Common Use Cases

**Prepare drawer content:**
```csharp
private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    // Update menu items with latest data
    RefreshMenuItems();
    
    // Show loading indicator
    loadingPanel.Visible = true;
}
```

**Log drawer activity:**
```csharp
private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    Logger.Log($"Drawer opening at {DateTime.Now}");
}
```

**Update UI elements:**
```csharp
private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    // Change hamburger icon appearance
    hamburgerButton.BackColor = Color.LightBlue;
    hamburgerButton.Text = "✖"; // Change to close icon
}
```

## Opened Event

The `Opened` event occurs when the drawer expand transition completes. Use this event to perform actions after the drawer is fully visible.

### Event Declaration

```csharp
// Raises when expand transition ends
public event OpenedEventHandler Opened;
```

### Hooking the Opened Event

```csharp
// Subscribe to Opened event
navigationDrawer1.Opened += NavigationDrawer1_Opened;

private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    MessageBox.Show("Drawer is now fully open");
}
```

**VB.NET:**
```vb
' Subscribe to Opened event
AddHandler navigationDrawer1.Opened, AddressOf NavigationDrawer1_Opened

Private Sub NavigationDrawer1_Opened(ByVal sender As Object, ByVal e As EventArgs)
    MessageBox.Show("Drawer is now fully open")
End Sub
```

### Common Use Cases

**Load data after animation:**
```csharp
private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    // Load heavy data only after drawer is fully open
    LoadNavigationData();
    
    // Hide loading indicator
    loadingPanel.Visible = false;
}
```

**Set focus to first menu item:**
```csharp
private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    // Set focus to first interactive item
    if (navigationDrawer1.Items.Count > 0)
    {
        var firstMenuItem = navigationDrawer1.Items
            .OfType<DrawerMenuItem>()
            .FirstOrDefault();
        
        firstMenuItem?.Focus();
    }
}
```

**Track analytics:**
```csharp
private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    Analytics.TrackEvent("DrawerOpened", new Dictionary<string, string>
    {
        { "Position", navigationDrawer1.Position.ToString() },
        { "Transition", navigationDrawer1.Transition.ToString() }
    });
}
```

## Closing Event

The `Closing` event occurs when the drawer collapse transition begins. This event is cancelable, allowing you to prevent the drawer from closing under certain conditions.

### Event Declaration

```csharp
// Raises when collapse transition begins
public event ClosingEventHandler Closing;
```

### Hooking the Closing Event

```csharp
// Subscribe to Closing event
navigationDrawer1.Closing += NavigationDrawer1_Closing;

private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    MessageBox.Show("Drawer is closing...");
}
```

**VB.NET:**
```vb
' Subscribe to Closing event
AddHandler navigationDrawer1.Closing, AddressOf NavigationDrawer1_Closing

Private Sub NavigationDrawer1_Closing(ByVal sender As Object, ByVal e As CancelEventArgs)
    MessageBox.Show("Drawer is closing...")
End Sub
```

### Canceling the Closing Operation

```csharp
private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    // Prevent closing if unsaved changes exist
    if (HasUnsavedChanges())
    {
        var result = MessageBox.Show(
            "You have unsaved changes. Close anyway?",
            "Confirm Close",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Warning);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true; // Prevent drawer from closing
        }
    }
}
```

### Common Use Cases

**Validate before closing:**
```csharp
private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    // Ensure required selection is made
    if (!IsNavigationItemSelected())
    {
        MessageBox.Show("Please select a navigation item before closing.");
        e.Cancel = true;
    }
}
```

**Confirm important actions:**
```csharp
private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    // Confirm if critical operation is in progress
    if (IsCriticalOperationInProgress())
    {
        var result = MessageBox.Show(
            "An operation is in progress. Closing the drawer may interrupt it. Continue?",
            "Operation In Progress",
            MessageBoxButtons.YesNo);
        
        e.Cancel = (result == DialogResult.No);
    }
}
```

**Update UI state:**
```csharp
private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    // Prepare UI for drawer closing
    if (!e.Cancel)
    {
        hamburgerButton.BackColor = SystemColors.Control;
        statusLabel.Text = "Closing navigation...";
    }
}
```

## Closed Event

The `Closed` event occurs when the drawer collapse transition completes. Use this event to perform cleanup or update UI after the drawer is fully hidden.

### Event Declaration

```csharp
// Raises when collapse transition ends
public event ClosedEventHandler Closed;
```

### Hooking the Closed Event

```csharp
// Subscribe to Closed event
navigationDrawer1.Closed += NavigationDrawer1_Closed;

private void NavigationDrawer1_Closed(object sender, EventArgs e)
{
    MessageBox.Show("Drawer is now fully closed");
}
```

**VB.NET:**
```vb
' Subscribe to Closed event
AddHandler navigationDrawer1.Closed, AddressOf NavigationDrawer1_Closed

Private Sub NavigationDrawer1_Closed(ByVal sender As Object, ByVal e As EventArgs)
    MessageBox.Show("Drawer is now fully closed")
End Sub
```

### Common Use Cases

**Reset UI elements:**
```csharp
private void NavigationDrawer1_Closed(object sender, EventArgs e)
{
    // Reset hamburger button appearance
    hamburgerButton.Text = "☰";
    hamburgerButton.BackColor = SystemColors.Control;
    
    // Clear status message
    statusLabel.Text = "Ready";
}
```

**Release resources:**
```csharp
private void NavigationDrawer1_Closed(object sender, EventArgs e)
{
    // Unload drawer data to free memory
    ClearDrawerCache();
    
    // Stop any background updates
    StopDrawerDataRefresh();
}
```

**Navigate or perform action:**
```csharp
private DrawerMenuItem selectedItem = null;

private void NavigationDrawer1_Closed(object sender, EventArgs e)
{
    // Navigate to selected page after drawer closes
    if (selectedItem != null)
    {
        NavigateToPage(selectedItem.Text);
        selectedItem = null;
    }
}
```

## Complete Event Handling Example

This example demonstrates all four events working together:

```csharp
public partial class MainForm : Form
{
    private bool isLoading = false;
    private DateTime openedTime;
    
    public MainForm()
    {
        InitializeComponent();
        InitializeDrawerEvents();
    }
    
    private void InitializeDrawerEvents()
    {
        // Subscribe to all drawer events
        navigationDrawer1.Opening += NavigationDrawer1_Opening;
        navigationDrawer1.Opened += NavigationDrawer1_Opened;
        navigationDrawer1.Closing += NavigationDrawer1_Closing;
        navigationDrawer1.Closed += NavigationDrawer1_Closed;
    }
    
    private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
    {
        // Prepare drawer
        Console.WriteLine("Opening: Preparing drawer content...");
        isLoading = true;
        
        // Update hamburger button
        hamburgerButton.BackColor = Color.LightBlue;
        hamburgerButton.Text = "✖";
        
        // Show loading indicator
        loadingIndicator.Visible = true;
    }
    
    private void NavigationDrawer1_Opened(object sender, EventArgs e)
    {
        // Complete loading
        Console.WriteLine("Opened: Drawer is fully visible");
        isLoading = false;
        openedTime = DateTime.Now;
        
        // Hide loading indicator
        loadingIndicator.Visible = false;
        
        // Set focus to first menu item
        var firstItem = navigationDrawer1.Items.OfType<DrawerMenuItem>().FirstOrDefault();
        firstItem?.Focus();
        
        // Log analytics
        Analytics.TrackEvent("DrawerOpened");
    }
    
    private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
    {
        Console.WriteLine("Closing: Validating before close...");
        
        // Prevent closing if loading
        if (isLoading)
        {
            MessageBox.Show("Please wait for content to load.");
            e.Cancel = true;
            return;
        }
        
        // Confirm if unsaved changes
        if (HasUnsavedChanges())
        {
            var result = MessageBox.Show(
                "You have unsaved changes. Close drawer anyway?",
                "Unsaved Changes",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Warning);
            
            e.Cancel = (result == DialogResult.No);
        }
    }
    
    private void NavigationDrawer1_Closed(object sender, EventArgs e)
    {
        Console.WriteLine("Closed: Drawer is fully hidden");
        
        // Calculate how long drawer was open
        TimeSpan openDuration = DateTime.Now - openedTime;
        Console.WriteLine($"Drawer was open for {openDuration.TotalSeconds:F1} seconds");
        
        // Reset hamburger button
        hamburgerButton.BackColor = SystemColors.Control;
        hamburgerButton.Text = "☰";
        
        // Clear any selections
        ClearMenuSelections();
        
        // Log analytics
        Analytics.TrackEvent("DrawerClosed", new Dictionary<string, string>
        {
            { "DurationSeconds", openDuration.TotalSeconds.ToString("F1") }
        });
    }
    
    private bool HasUnsavedChanges()
    {
        // Check for unsaved changes (implementation specific)
        return false;
    }
    
    private void ClearMenuSelections()
    {
        // Reset menu item selections
        foreach (var item in navigationDrawer1.Items.OfType<DrawerMenuItem>())
        {
            item.BackColor = item.DefaultColor;
        }
    }
}
```

## Event Handling Best Practices

### Avoid Heavy Operations During Transitions

```csharp
// ❌ Bad: Heavy operation during animation
private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    LoadLargeDataset(); // This will cause stuttering
}

// ✅ Good: Heavy operations after animation completes
private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    LoadLargeDataset(); // Smooth animation, then load data
}
```

### Proper Error Handling

```csharp
private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    try
    {
        LoadNavigationData();
    }
    catch (Exception ex)
    {
        Logger.LogError("Failed to load navigation data", ex);
        MessageBox.Show("Failed to load menu data. Please try again.",
            "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
```

### Async Operations

```csharp
private async void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    try
    {
        loadingIndicator.Visible = true;
        await LoadNavigationDataAsync();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error: {ex.Message}");
    }
    finally
    {
        loadingIndicator.Visible = false;
    }
}

private async Task LoadNavigationDataAsync()
{
    await Task.Run(() =>
    {
        // Load data asynchronously
        Thread.Sleep(1000); // Simulate data loading
    });
}
```

### Memory Management

```csharp
// Unsubscribe from events when disposing
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        navigationDrawer1.Opening -= NavigationDrawer1_Opening;
        navigationDrawer1.Opened -= NavigationDrawer1_Opened;
        navigationDrawer1.Closing -= NavigationDrawer1_Closing;
        navigationDrawer1.Closed -= NavigationDrawer1_Closed;
    }
    
    base.Dispose(disposing);
}
```

## Troubleshooting

### Event Not Firing

**Problem:** Events don't fire when drawer opens/closes.

**Solution:**
```csharp
// Ensure events are subscribed after control initialization
this.Load += (s, e) =>
{
    navigationDrawer1.Opening += NavigationDrawer1_Opening;
    navigationDrawer1.Opened += NavigationDrawer1_Opened;
    navigationDrawer1.Closing += NavigationDrawer1_Closing;
    navigationDrawer1.Closed += NavigationDrawer1_Closed;
};
```

### Cancel Not Working

**Problem:** Setting `e.Cancel = true` doesn't prevent drawer from closing.

**Solution:** Ensure you're using the `Closing` event (which is cancelable), not `Closed`:
```csharp
// ✅ Correct: Closing event is cancelable
navigationDrawer1.Closing += (s, e) => { e.Cancel = true; };

// ❌ Wrong: Closed event is NOT cancelable
navigationDrawer1.Closed += (s, e) => { /* Too late to cancel */ };
```

### Multiple Event Firings

**Problem:** Events fire multiple times for a single action.

**Solution:** Unsubscribe before subscribing to avoid duplicates:
```csharp
// Safely subscribe (removes existing handler first)
navigationDrawer1.Opening -= NavigationDrawer1_Opening;
navigationDrawer1.Opening += NavigationDrawer1_Opening;
```

## Next Steps

- **Getting started:** See [getting-started.md](getting-started.md) for basic setup
- **Drawer features:** See [drawer-features.md](drawer-features.md) for transitions and positioning
- **Advanced scenarios:** See [advanced-usage.md](advanced-usage.md) for complex event patterns
