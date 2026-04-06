# Events and Event Handling

## Table of Contents
- [Edit Events](#edit-events)
- [Selection Events](#selection-events)
- [Tab Primitive Events](#tab-primitive-events)
- [Drawing Events](#drawing-events)
- [Tab Page Events](#tab-page-events)
- [Tab Movement Events](#tab-movement-events)
- [Complete Example](#complete-event-handling-example)
- [Best Practices](#best-practices)

Comprehensive guide to handling events in TabControlAdv for edit operations, selection changes, custom rendering, and more.

## Edit Events

### BeforeEdit Event

Fired when tab text enters edit mode.

```csharp
tabControlAdv1.BeforeEdit += (sender, e) =>
{
    Console.WriteLine($"Starting to edit: {e.EditText}");
    
    // Note: Cannot cancel BeforeEdit
    // Use LabelEdit property to prevent editing entirely
};
```

**EditEventArgs Properties:**
- `EditText` - The current text being edited

### AfterEdit Event

Fired after text editing is completed, even if no changes were made.

```csharp
tabControlAdv1.AfterEdit += (sender, e) =>
{
    string newText = e.EditText.Trim();
    
    // Validate new text
    if (string.IsNullOrWhiteSpace(newText))
    {
        MessageBox.Show("Tab name cannot be empty", "Invalid Name");
        // Restore original name or set default
    }
    else if (IsDuplicateName(newText))
    {
        MessageBox.Show("Tab name already exists", "Duplicate Name");
    }
    else
    {
        Console.WriteLine($"Tab renamed to: {newText}");
        SaveTabNames();
    }
};

private bool IsDuplicateName(string name)
{
    return tabControlAdv1.TabPages.Cast<TabPageAdv>()
        .Count(t => t.Text == name) > 1;
}
```

### LabelEditTextChanged Event

Fired when the text is changed during editing.

```csharp
tabControlAdv1.LabelEditTextChanged += (sender, e) =>
{
    Console.WriteLine("Tab text is being modified");
};
```

## Selection Events

### SelectedIndexChanging Event

Fired before the selected tab changes. Can be cancelled.

```csharp
tabControlAdv1.SelectedIndexChanging += (sender, e) =>
{
    Console.WriteLine($"Changing from tab {e.OldSelectedIndex} to {e.NewSelectedIndex}");
    
    // Prevent switching to specific tab
    if (e.NewSelectedIndex == 2)
    {
        e.Cancel = true;
        MessageBox.Show("This tab is currently disabled");
        return;
    }
    
    // Check for unsaved changes
    if (HasUnsavedChanges(e.OldSelectedIndex))
    {
        var result = MessageBox.Show(
            "You have unsaved changes. Continue?",
            "Unsaved Changes",
            MessageBoxButtons.YesNo);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true;
        }
    }
};

private bool HasUnsavedChanges(int tabIndex)
{
    // Check if tab has unsaved changes
    return false; // Implement your logic
}
```

**SelectedIndexChangingEventArgs Properties:**
- `OldSelectedIndex` - Previously selected tab index
- `NewSelectedIndex` - Tab index being selected
- `Cancel` - Set to true to prevent selection change

### SelectedIndexChanged Event

Fired after the selected tab has changed.

```csharp
tabControlAdv1.SelectedIndexChanged += (sender, e) =>
{
    var selectedTab = tabControlAdv1.SelectedTab;
    Console.WriteLine($"Now viewing: {selectedTab.Text}");
    
    // Load content for selected tab
    LoadTabContent(selectedTab);
    
    // Update UI state
    UpdateStatusBar($"Viewing {selectedTab.Text}");
    UpdateToolbar(selectedTab);
};

private void LoadTabContent(TabPageAdv tab)
{
    // Lazy load tab content
    if (tab.Controls.Count == 0)
    {
        // Load content dynamically
        Console.WriteLine($"Loading content for {tab.Text}");
    }
}
```

## Tab Primitive Events

### TabPrimitiveClick Event

Fired when a TabPrimitive (navigation button) is clicked.

```csharp
tabControlAdv1.TabPrimitiveClick += (sender, e) =>
{
    Console.WriteLine($"Primitive clicked: {e.TabPrimitive.Name}");
    
    // Handle custom primitives
    if (e.TabPrimitive.Name == "CustomSave")
    {
        SaveAllTabs();
        e.Cancel = true; // Cancel default behavior
        return;
    }
    
    if (e.TabPrimitive.Name == "CustomPrint")
    {
        PrintCurrentTab();
        e.Cancel = true;
        return;
    }
    
    // Allow default behavior for standard primitives
};
```

**TabPrimitiveClickEventArgs Properties:**
- `TabPrimitive` - The clicked TabPrimitive
- `Cancel` - Set to true to prevent default action

## Drawing Events

### DrawItem Event

Fired when a tab needs to be drawn. Allows custom rendering.

```csharp
tabControlAdv1.DrawItem += (sender, drawItemInfo) =>
{
    // Draw background
    drawItemInfo.DrawBackground();
    
    // Draw interior (content area)
    drawItemInfo.DrawInterior();
    
    // Custom drawing
    CustomDrawTab(drawItemInfo);
};

private void CustomDrawTab(DrawTabEventArgs e)
{
    Graphics g = e.Graphics;
    Rectangle bounds = e.Bounds;
    bool isSelected = e.IsSelected;
    
    // Custom gradient background
    using (LinearGradientBrush brush = new LinearGradientBrush(
        bounds,
        isSelected ? Color.LightBlue : Color.LightGray,
        isSelected ? Color.Blue : Color.Gray,
        LinearGradientMode.Vertical))
    {
        g.FillRectangle(brush, bounds);
    }
    
    // Custom text
    string text = tabControlAdv1.TabPages[e.Index].Text;
    using (StringFormat sf = new StringFormat())
    {
        sf.Alignment = StringAlignment.Center;
        sf.LineAlignment = StringAlignment.Center;
        
        using (Brush textBrush = new SolidBrush(
            isSelected ? Color.White : Color.Black))
        {
            g.DrawString(text, tabControlAdv1.Font, textBrush, bounds, sf);
        }
    }
}
```

**DrawTabEventArgs Properties:**
- `Graphics` - Graphics object for drawing
- `Bounds` - Bounding rectangle of the tab
- `Index` - Tab index
- `IsSelected` - Whether tab is selected
- `DrawBackground()` - Method to draw default background
- `DrawInterior()` - Method to draw default interior

## Tab Page Events

### Closed Event (TabPageAdv)

Fired after a tab page is closed.

```csharp
tabPageAdv1.Closed += (sender, e) =>
{
    var tab = sender as TabPageAdv;
    Console.WriteLine($"Tab '{tab.Text}' was closed");
    CleanupTabResources(tab);
};
```

### Closing Event (TabPageAdv)

Fired before a tab page is closed. Can be cancelled.

```csharp
tabPageAdv1.Closing += (sender, e) =>
{
    var tab = sender as TabPageAdv;
    
    if (TabHasUnsavedChanges(tab))
    {
        var result = MessageBox.Show(
            $"Save changes to '{tab.Text}'?",
            "Save Changes",
            MessageBoxButtons.YesNoCancel);
        
        if (result == DialogResult.Cancel)
        {
            e.Cancel = true;
            return;
        }
        else if (result == DialogResult.Yes)
        {
            SaveTabChanges(tab);
        }
    }
};
```

**TabPageAdvClosingEventArgs Properties:**
- `Cancel` - Set to true to prevent closing

## Tab Movement Events

### TabsOrderChanged Event

Fired when tab order changes (after drag-and-drop).

```csharp
tabControlAdv1.TabsOrderChanged += (sender, e) =>
{
    Console.WriteLine("Tab order changed");
    SaveTabOrder();
};
```

### TabMoving Event

Fired when a tab is being moved. Can be cancelled.

```csharp
tabControlAdv1.TabMoving += (sender, e) =>
{
    // Prevent moving first tab
    if (e.From == 0 || e.Target == 0)
    {
        e.Cancel = true;
        MessageBox.Show("The first tab cannot be moved");
    }
};
```

**TabMovingEventArgs Properties:**
- `From` - Original tab index
- `Target` - Target position index
- `Cancel` - Set to true to prevent move

## Complete Event Handling Example

```csharp
public class EventsExampleForm : Form
{
    private TabControlAdv tabControl;
    private ListBox eventLog;
    
    public EventsExampleForm()
    {
        InitializeControls();
        AttachAllEvents();
        AddTabs();
    }
    
    private void InitializeControls()
    {
        this.Text = "TabControlAdv Events Demo";
        this.Size = new Size(900, 600);
        
        // Setup tab control
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Left;
        tabControl.Width = 600;
        tabControl.LabelEdit = true;
        tabControl.UserMoveTabs = true;
        tabControl.ShowTabCloseButton = true;
        
        // Setup event log
        eventLog = new ListBox();
        eventLog.Dock = DockStyle.Fill;
        eventLog.Font = new Font("Consolas", 9);
        
        this.Controls.Add(tabControl);
        this.Controls.Add(eventLog);
    }
    
    private void AttachAllEvents()
    {
        // Edit events
        tabControl.BeforeEdit += (s, e) => LogEvent($"BeforeEdit: {e.EditText}");
        tabControl.AfterEdit += (s, e) => LogEvent($"AfterEdit: {e.EditText}");
        
        // Selection events
        tabControl.SelectedIndexChanging += (s, e) => 
            LogEvent($"Changing: {e.OldSelectedIndex} → {e.NewSelectedIndex}");
        tabControl.SelectedIndexChanged += (s, e) => 
            LogEvent($"Changed: {tabControl.SelectedIndex}");
        
        // Movement events
        tabControl.TabMoving += (s, e) => LogEvent($"Moving: {e.From} → {e.Target}");
        tabControl.TabsOrderChanged += (s, e) => LogEvent("Order Changed");
    }
    
    private void AddTabs()
    {
        for (int i = 1; i <= 5; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Tab {i}";
            
            tab.Closing += (s, e) => LogEvent($"Tab.Closing: {((TabPageAdv)s).Text}");
            tab.Closed += (s, e) => LogEvent($"Tab.Closed: {((TabPageAdv)s).Text}");
            
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void LogEvent(string message)
    {
        eventLog.Items.Insert(0, $"[{DateTime.Now:HH:mm:ss}] {message}");
        if (eventLog.Items.Count > 100)
            eventLog.Items.RemoveAt(eventLog.Items.Count - 1);
    }
}
```

## Best Practices

### Event Handling
- Always check for null when accessing event args
- Use lambda expressions for simple handlers
- Extract complex logic to separate methods
- Unsubscribe from events when disposing controls

### Performance
- Avoid heavy processing in frequently-fired events (Paint, DrawItem)
- Use BeginInvoke for long-running operations
- Cache values instead of recalculating in events

### Common Patterns
- Validate in "Changing" events, act in "Changed" events
- Use "Closing" to prompt for save, "Closed" to cleanup
- Handle exceptions gracefully in event handlers

## Troubleshooting

- **Event Not Firing:** Check if event is properly subscribed and control is initialized
- **Event Fires Multiple Times:** Check for duplicate subscriptions
- **Cannot Cancel Event:** Only events with Cancel property can be cancelled (BeforeEdit cannot be cancelled - use LabelEdit property instead)
