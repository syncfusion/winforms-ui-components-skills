# Selection and Events

This guide covers programmatic selection control and handling selection events in TreeNavigator.

## SelectedItem Property

The `SelectedItem` property gets or sets the currently selected TreeMenuItem in the current hierarchy level.

**Property Type:** `TreeMenuItem`

**Access:** Read/Write

---

## Getting the Selected Item

**C# Example:**

```csharp
// Get currently selected item
TreeMenuItem selected = treeNavigator.SelectedItem;

if (selected != null)
{
    MessageBox.Show($"Selected: {selected.Text}");
}
```

**VB.NET Example:**

```vbnet
' Get currently selected item
Dim selected As TreeMenuItem = treeNavigator.SelectedItem

If selected IsNot Nothing Then
    MessageBox.Show($"Selected: {selected.Text}")
End If
```

---

## Setting the Selected Item Programmatically

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

// Create items
TreeMenuItem item1 = new TreeMenuItem { Text = "Documents" };
TreeMenuItem item2 = new TreeMenuItem { Text = "Downloads" };
TreeMenuItem item3 = new TreeMenuItem { Text = "Pictures" };

treeNavigator.Items.Add(item1);
treeNavigator.Items.Add(item2);
treeNavigator.Items.Add(item3);

// Set selected item programmatically
treeNavigator.SelectedItem = item3; // Pictures is now selected
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

' Create items
Dim item1 As New TreeMenuItem With {.Text = "Documents"}
Dim item2 As New TreeMenuItem With {.Text = "Downloads"}
Dim item3 As New TreeMenuItem With {.Text = "Pictures"}

treeNavigator.Items.Add(item1)
treeNavigator.Items.Add(item2)
treeNavigator.Items.Add(item3)

' Set selected item programmatically
treeNavigator.SelectedItem = item3 ' Pictures is now selected
```

---

## SelectionChanging Event

The **SelectionChanging** event fires **before** the selection changes, allowing you to cancel the selection or perform validation.

### Event Signature

```csharp
void TreeNavigator_SelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
```

### SelectionStateChangingEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| **NewValue** | TreeMenuItem | The item that will be selected |
| **OldValue** | TreeMenuItem | The currently selected item (before change) |
| **Expanded** | bool | True if the selected item is expanded |
| **Cancel** | bool | Set to true to cancel the selection change |

### Basic Usage

**C# Example:**

```csharp
treeNavigator.SelectionChanging += TreeNavigator_SelectionChanging;

void TreeNavigator_SelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
{
    TreeMenuItem newItem = args.NewValue;
    TreeMenuItem oldItem = args.OldValue;
    bool isExpanded = args.Expanded;
    
    // Log selection change
    Console.WriteLine($"Changing from '{oldItem?.Text}' to '{newItem?.Text}'");
    Console.WriteLine($"Is expanded: {isExpanded}");
}
```

**VB.NET Example:**

```vbnet
AddHandler treeNavigator.SelectionChanging, AddressOf TreeNavigator_SelectionChanging

Private Sub TreeNavigator_SelectionChanging(sender As TreeNavigator, args As SelectionStateChangingEventArgs)
    Dim newItem As TreeMenuItem = args.NewValue
    Dim oldItem As TreeMenuItem = args.OldValue
    Dim isExpanded As Boolean = args.Expanded
    
    ' Log selection change
    Console.WriteLine($"Changing from '{If(oldItem?.Text, "None")}' to '{If(newItem?.Text, "None")}'")
    Console.WriteLine($"Is expanded: {isExpanded}")
End Sub
```

### Cancelling Selection Change

You can cancel selection changes by setting `args.Cancel = true`.

**C# Example:**

```csharp
void TreeNavigator_SelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
{
    TreeMenuItem newItem = args.NewValue;
    
    // Prevent selection of items with "Locked" in the name
    if (newItem != null && newItem.Text.Contains("Locked"))
    {
        args.Cancel = true;
        MessageBox.Show("This item is locked and cannot be selected.");
    }
}
```

**VB.NET Example:**

```vbnet
Private Sub TreeNavigator_SelectionChanging(sender As TreeNavigator, args As SelectionStateChangingEventArgs)
    Dim newItem As TreeMenuItem = args.NewValue
    
    ' Prevent selection of items with "Locked" in the name
    If newItem IsNot Nothing AndAlso newItem.Text.Contains("Locked") Then
        args.Cancel = True
        MessageBox.Show("This item is locked and cannot be selected.")
    End If
End Sub
```

---

## SelectionChanged Event

The **SelectionChanged** event fires **after** the selection has changed. Use this event to respond to completed selection changes.

### Event Signature

```csharp
void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
```

### SelectionStateChangedEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| **SelectedItem** | TreeMenuItem | The currently selected item (after change) |
| **Expanded** | bool | True if the selected item is expanded |

### Basic Usage

**C# Example:**

```csharp
treeNavigator.SelectionChanged += TreeNavigator_SelectionChanged;

void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
{
    TreeMenuItem selected = e.SelectedItem;
    bool isExpanded = e.Expanded;
    
    if (selected != null)
    {
        MessageBox.Show($"Selected: {selected.Text}\nExpanded: {isExpanded}");
    }
}
```

**VB.NET Example:**

```vbnet
AddHandler treeNavigator.SelectionChanged, AddressOf TreeNavigator_SelectionChanged

Private Sub TreeNavigator_SelectionChanged(sender As TreeNavigator, e As SelectionStateChangedEventArgs)
    Dim selected As TreeMenuItem = e.SelectedItem
    Dim isExpanded As Boolean = e.Expanded
    
    If selected IsNot Nothing Then
        MessageBox.Show($"Selected: {selected.Text}" & vbNewLine & $"Expanded: {isExpanded}")
    End If
End Sub
```

---

## Practical Examples

### Example 1: Update Content Panel Based on Selection

```csharp
private Panel contentPanel;

void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
{
    TreeMenuItem selected = e.SelectedItem;
    
    if (selected == null) return;
    
    // Clear existing content
    contentPanel.Controls.Clear();
    
    // Load content based on selection
    switch (selected.Text)
    {
        case "Dashboard":
            LoadDashboardView();
            break;
        case "Reports":
            LoadReportsView();
            break;
        case "Settings":
            LoadSettingsView();
            break;
        default:
            LoadDefaultView(selected.Text);
            break;
    }
}

private void LoadDashboardView()
{
    Label label = new Label 
    { 
        Text = "Dashboard Content",
        Dock = DockStyle.Fill,
        Font = new Font("Arial", 16, FontStyle.Bold)
    };
    contentPanel.Controls.Add(label);
}
```

### Example 2: Validation Before Selection

```csharp
private bool hasUnsavedChanges = false;

void TreeNavigator_SelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
{
    if (hasUnsavedChanges)
    {
        DialogResult result = MessageBox.Show(
            "You have unsaved changes. Do you want to discard them?",
            "Unsaved Changes",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Warning
        );
        
        if (result == DialogResult.No)
        {
            // Cancel the selection change
            args.Cancel = true;
        }
        else
        {
            // Allow change and reset flag
            hasUnsavedChanges = false;
        }
    }
}
```

### Example 3: Track Navigation History

```csharp
private Stack<TreeMenuItem> navigationHistory = new Stack<TreeMenuItem>();

void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        // Add to history
        navigationHistory.Push(e.SelectedItem);
        
        // Update breadcrumb or navigation trail
        UpdateBreadcrumb();
    }
}

private void UpdateBreadcrumb()
{
    // Display navigation history
    string trail = string.Join(" > ", navigationHistory.Reverse().Select(item => item.Text));
    lblBreadcrumb.Text = trail;
}

private void GoBack()
{
    if (navigationHistory.Count > 1)
    {
        navigationHistory.Pop(); // Remove current
        TreeMenuItem previous = navigationHistory.Peek();
        treeNavigator.SelectedItem = previous;
    }
}
```

### Example 4: Load Data on Selection

```csharp
void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
{
    TreeMenuItem selected = e.SelectedItem;
    
    if (selected == null) return;
    
    // Show loading indicator
    lblStatus.Text = "Loading...";
    Application.DoEvents();
    
    // Simulate data loading
    Task.Run(() => 
    {
        // Load data based on selection
        var data = LoadDataForItem(selected.Text);
        
        // Update UI on main thread
        this.Invoke((Action)(() => 
        {
            DisplayData(data);
            lblStatus.Text = "Ready";
        }));
    });
}

private List<string> LoadDataForItem(string itemName)
{
    // Simulate database query or API call
    System.Threading.Thread.Sleep(1000);
    return new List<string> { $"Data 1 for {itemName}", $"Data 2 for {itemName}" };
}
```

### Example 5: Conditional Selection Based on Permissions

```csharp
private Dictionary<string, bool> permissions = new Dictionary<string, bool>
{
    { "Admin Settings", false },
    { "User Management", false },
    { "System Logs", false },
    { "General Settings", true }
};

void TreeNavigator_SelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
{
    TreeMenuItem newItem = args.NewValue;
    
    if (newItem != null && permissions.ContainsKey(newItem.Text))
    {
        bool hasPermission = permissions[newItem.Text];
        
        if (!hasPermission)
        {
            args.Cancel = true;
            MessageBox.Show(
                $"You don't have permission to access '{newItem.Text}'.",
                "Access Denied",
                MessageBoxButtons.OK,
                MessageBoxIcon.Warning
            );
        }
    }
}
```

### Example 6: Highlight Related Items on Selection

```csharp
void TreeNavigator_SelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
{
    TreeMenuItem selected = e.SelectedItem;
    
    if (selected == null) return;
    
    // Reset all items to default color
    foreach (TreeMenuItem item in treeNavigator.Items)
    {
        item.ItemBackColor = Color.White;
    }
    
    // Highlight selected item and related items
    selected.ItemBackColor = Color.LightBlue;
    
    // Highlight parent items (if applicable)
    HighlightParentChain(selected);
}

private void HighlightParentChain(TreeMenuItem item)
{
    // Logic to highlight parent items in the hierarchy
    // This depends on how you track parent-child relationships
}
```

---

## Touch Scroll Behavior

The `UseTouchScrollBehavior` property enables touch-friendly scrolling with an auto-hide scrollbar.

**Behavior:**
- Scrollbar appears only when hovering or touching near the scroll area
- Provides a cleaner, more modern interface
- Ideal for touch-enabled devices

**C# Example:**

```csharp
// Enable touch scroll behavior
treeNavigator.UseTouchScrollBehavior = true;
```

**VB.NET Example:**

```vbnet
' Enable touch scroll behavior
treeNavigator.UseTouchScrollBehavior = True
```

**When to use:**
- Touch-enabled applications
- Tablet or hybrid device interfaces
- Modern, minimal UI designs
- Applications where screen space is limited

---

## Complete Event Handling Example

```csharp
public class NavigationManager
{
    private TreeNavigator navigator;
    private Panel contentArea;
    private Label statusLabel;
    private Stack<TreeMenuItem> history = new Stack<TreeMenuItem>();
    
    public NavigationManager(TreeNavigator nav, Panel content, Label status)
    {
        navigator = nav;
        contentArea = content;
        statusLabel = status;
        
        // Subscribe to events
        navigator.SelectionChanging += OnSelectionChanging;
        navigator.SelectionChanged += OnSelectionChanged;
    }
    
    private void OnSelectionChanging(TreeNavigator sender, SelectionStateChangingEventArgs args)
    {
        // Validate selection
        if (args.NewValue != null && args.NewValue.Text.Contains("[Disabled]"))
        {
            args.Cancel = true;
            statusLabel.Text = "This item is currently disabled.";
            return;
        }
        
        // Check for unsaved changes
        if (HasUnsavedChanges())
        {
            var result = MessageBox.Show(
                "Unsaved changes will be lost. Continue?",
                "Warning",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Warning
            );
            
            if (result == DialogResult.No)
            {
                args.Cancel = true;
            }
        }
    }
    
    private void OnSelectionChanged(TreeNavigator sender, SelectionStateChangedEventArgs e)
    {
        TreeMenuItem selected = e.SelectedItem;
        
        if (selected == null) return;
        
        // Add to history
        history.Push(selected);
        
        // Update status
        statusLabel.Text = $"Selected: {selected.Text}";
        
        // Load content
        LoadContentFor(selected);
        
        // Log navigation
        LogNavigation(selected, e.Expanded);
    }
    
    private bool HasUnsavedChanges()
    {
        // Check if there are unsaved changes
        return false; // Placeholder
    }
    
    private void LoadContentFor(TreeMenuItem item)
    {
        contentArea.Controls.Clear();
        
        // Load content based on item
        Label content = new Label
        {
            Text = $"Content for: {item.Text}",
            Dock = DockStyle.Fill,
            Font = new Font("Arial", 14)
        };
        
        contentArea.Controls.Add(content);
    }
    
    private void LogNavigation(TreeMenuItem item, bool expanded)
    {
        Console.WriteLine($"[{DateTime.Now:HH:mm:ss}] Navigated to '{item.Text}' (Expanded: {expanded})");
    }
    
    public void GoBack()
    {
        if (history.Count > 1)
        {
            history.Pop(); // Remove current
            TreeMenuItem previous = history.Peek();
            navigator.SelectedItem = previous;
        }
    }
}
```

---

## Best Practices

### Event Handling

1. **Always Check for Null**: Both `SelectedItem` and event args properties can be null
2. **Use SelectionChanging for Validation**: Cancel invalid selections before they occur
3. **Use SelectionChanged for Actions**: Respond to completed selections for loading data or updating UI
4. **Avoid Recursive Calls**: Don't programmatically set `SelectedItem` inside event handlers unless necessary

### Performance

1. **Defer Heavy Operations**: Use async/await for data loading in SelectionChanged
2. **Batch UI Updates**: Update multiple controls at once to reduce flicker
3. **Unsubscribe When Done**: Remove event handlers when disposing controls

### User Experience

1. **Provide Feedback**: Update status labels or progress indicators during selection
2. **Handle Errors Gracefully**: Show user-friendly messages for selection errors
3. **Maintain Context**: Track navigation history for back/forward functionality
4. **Visual Feedback**: Update colors or indicators based on selection state

---

## Troubleshooting

### SelectionChanged Not Firing

**Problem:** SelectionChanged event doesn't fire when clicking items.

**Solution:**
1. Verify event handler is subscribed: `treeNavigator.SelectionChanged += handler`
2. Check if SelectionChanging is cancelling the change: Set breakpoint in SelectionChanging
3. Ensure item is actually clickable (not disabled or overlapped)

### Cannot Select Item Programmatically

**Problem:** Setting `SelectedItem` doesn't work.

**Solution:**
1. Ensure item exists in Items collection
2. Item must be at the current hierarchy level (not nested in collapsed parent)
3. Check if SelectionChanging is cancelling the change
4. Verify item reference is valid (not null or disposed)

### Event Fires Multiple Times

**Problem:** SelectionChanged fires more than once for a single selection.

**Solution:**
1. Check if event handler is subscribed multiple times (remove duplicate subscriptions)
2. Avoid recursively setting SelectedItem inside event handlers
3. Use event debouncing for rapid selections

### Cancel Not Working in SelectionChanging

**Problem:** Setting `args.Cancel = true` doesn't prevent selection.

**Solution:**
1. Verify you're using SelectionChanging, not SelectionChanged
2. Ensure Cancel is set before the method returns
3. Check that no other code is overriding the selection after the event

---

## Next Steps

- **Navigation Modes**: Configure Default or Extended navigation behavior
- **Appearance**: Customize visual styles and colors
- **TreeMenuItem Management**: Build and manipulate complex hierarchies
