# Events and Event Handling

## Table of Contents
- [Edit Events](#edit-events)
- [Selection Events](#selection-events)
- [Tab Primitive Events](#tab-primitive-events)
- [Drawing Events](#drawing-events)
- [Tab Page Events](#tab-page-events)
- [Appearance Events](#appearance-events)
- [Control Events](#control-events)
- [Tab Movement Events](#tab-movement-events)
- [Input Events](#input-events)

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
    // Real-time validation could go here
};
```

### LabelEditChanged Event

Fired when the LabelEdit property value changes.

```csharp
tabControlAdv1.LabelEditChanged += (sender, e) =>
{
    Console.WriteLine($"Label editing is now: {tabControlAdv1.LabelEdit}");
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
    
    // Cleanup resources
    CleanupTabResources(tab);
    
    // Check if all tabs are closed
    if (tabControlAdv1.TabPages.Count == 0)
    {
        AddDefaultTab();
    }
};
```

### Closing Event (TabPageAdv)

Fired before a tab page is closed. Can be cancelled.

```csharp
tabPageAdv1.Closing += (sender, e) =>
{
    var tab = sender as TabPageAdv;
    
    // Prompt to save
    if (TabHasUnsavedChanges(tab))
    {
        var result = MessageBox.Show(
            $"Save changes to '{tab.Text}'?",
            "Save Changes",
            MessageBoxButtons.YesNoCancel,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.Cancel)
        {
            e.Cancel = true; // Cancel closing
            return;
        }
        else if (result == DialogResult.Yes)
        {
            SaveTabChanges(tab);
        }
    }
    
    Console.WriteLine($"Closing tab: {tab.Text}");
};
```

**TabPageAdvClosingEventArgs Properties:**
- `Cancel` - Set to true to prevent closing

## Appearance Events

### BackColorChanged Event

Fired when the BackColor property changes.

```csharp
tabControlAdv1.BackColorChanged += (sender, e) =>
{
    Console.WriteLine("Background color changed");
    UpdateRelatedColors();
};
```

### BackgroundImageChanged Event

Fired when the BackgroundImage property changes.

```csharp
tabControlAdv1.BackgroundImageChanged += (sender, e) =>
{
    Console.WriteLine("Background image changed");
};
```

### BackgroundImageLayoutChanged Event

Fired when the BackgroundImageLayout property changes.

```csharp
tabControlAdv1.BackgroundImageLayoutChanged += (sender, e) =>
{
    Console.WriteLine($"Background layout: {tabControlAdv1.BackgroundImageLayout}");
};
```

### ForeColorChanged Event

Fired when the ForeColor property changes.

```csharp
tabControlAdv1.ForeColorChanged += (sender, e) =>
{
    Console.WriteLine("Foreground color changed");
};
```

### PaddingChanged Event

Fired when the Padding property changes.

```csharp
tabControlAdv1.PaddingChanged += (sender, e) =>
{
    Console.WriteLine($"Padding changed to: {tabControlAdv1.Padding}");
};
```

## Control Events

### ControlAdded Event

Fired when a control (including TabPageAdv) is added.

```csharp
tabControlAdv1.ControlAdded += (sender, e) =>
{
    Console.WriteLine($"Control added: {e.Control.Name}");
    
    if (e.Control is TabPageAdv tab)
    {
        Console.WriteLine($"New tab: {tab.Text}");
        SetupNewTab(tab);
    }
};
```

**ControlEventArgs Properties:**
- `Control` - The control that was added

### ControlRemoved Event

Fired when a control is removed.

```csharp
tabControlAdv1.ControlRemoved += (sender, e) =>
{
    Console.WriteLine($"Control removed: {e.Control.Name}");
    
    if (e.Control is TabPageAdv tab)
    {
        Console.WriteLine($"Tab removed: {tab.Text}");
    }
};
```

## Tab Movement Events

### TabsOrderChanged Event

Fired when tab order changes (after drag-and-drop).

```csharp
tabControlAdv1.TabsOrderChanged += (sender, e) =>
{
    Console.WriteLine("Tab order changed:");
    
    for (int i = 0; i < tabControlAdv1.TabPages.Count; i++)
    {
        Console.WriteLine($"  Position {i}: {tabControlAdv1.TabPages[i].Text}");
    }
    
    // Save new order
    SaveTabOrder();
};
```

### TabMoving Event

Fired when a tab is being moved. Can be cancelled.

```csharp
tabControlAdv1.TabMoving += (sender, e) =>
{
    Console.WriteLine($"Moving tab from {e.From} to {e.Target}");
    
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

## Input Events

### Paint Event

Fired when the control needs repainting.

```csharp
tabControlAdv1.Paint += (sender, e) =>
{
    // Custom painting
    if (tabControlAdv1.ClientRectangle.Width > 0 && 
        tabControlAdv1.ClientRectangle.Height > 0)
    {
        // Draw custom background
        using (LinearGradientBrush brush = new LinearGradientBrush(
            tabControlAdv1.ClientRectangle,
            SystemColors.Control,
            SystemColors.ControlDark,
            LinearGradientMode.Horizontal))
        {
            e.Graphics.FillRectangle(brush, tabControlAdv1.ClientRectangle);
        }
    }
};
```

### PreviewKeyDown Event

Fired before KeyDown when a key is pressed.

```csharp
tabControlAdv1.PreviewKeyDown += (sender, e) =>
{
    Console.WriteLine($"Key pressed: {e.KeyCode}");
    Console.WriteLine($"Modifiers: {e.Modifiers}");
    
    // Handle custom keyboard shortcuts
    if (e.Control && e.KeyCode == Keys.W)
    {
        // Ctrl+W to close current tab
        CloseCurrentTab();
    }
};
```

**PreviewKeyDownEventArgs Properties:**
- `KeyCode` - The key pressed
- `KeyValue` - Integer value of key
- `KeyData` - Key data with modifiers
- `Modifiers` - Modifier keys (Ctrl, Alt, Shift)
- `Alt`, `Control`, `Shift` - Boolean for each modifier

### RegionChanged Event

Fired when the Region property changes.

```csharp
tabControlAdv1.RegionChanged += (sender, e) =>
{
    Console.WriteLine("Control region changed");
};
```

### TextChanged Event

Fired when the Text property changes.

```csharp
tabControlAdv1.TextChanged += (sender, e) =>
{
    Console.WriteLine($"Text changed to: {tabControlAdv1.Text}");
};
```

## Complete Event Handling Example

```csharp
public class EventsExampleForm : Form
{
    private TabControlAdv tabControl;
    private ListBox eventLog;
    
    public EventsExampleForm()
    {
        InitializeForm();
        SetupTabControl();
        SetupEventLog();
        AttachAllEvents();
        AddTabs();
    }
    
    private void InitializeForm()
    {
        this.Text = "TabControlAdv Events Demo";
        this.Size = new Size(900, 600);
    }
    
    private void SetupTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Left;
        tabControl.Width = 600;
        tabControl.LabelEdit = true;
        tabControl.UserMoveTabs = true;
        tabControl.ShowTabCloseButton = true;
        this.Controls.Add(tabControl);
    }
    
    private void SetupEventLog()
    {
        eventLog = new ListBox();
        eventLog.Dock = DockStyle.Fill;
        eventLog.Font = new Font("Consolas", 9);
        
        Panel panel = new Panel();
        panel.Dock = DockStyle.Fill;
        panel.Padding = new Padding(5);
        
        Label title = new Label();
        title.Text = "Event Log";
        title.Dock = DockStyle.Top;
        title.Font = new Font("Segoe UI", 10, FontStyle.Bold);
        
        panel.Controls.Add(eventLog);
        panel.Controls.Add(title);
        this.Controls.Add(panel);
    }
    
    private void AttachAllEvents()
    {
        // Edit events
        tabControl.BeforeEdit += (s, e) => 
            LogEvent($"BeforeEdit: {e.EditText}");
        tabControl.AfterEdit += (s, e) => 
            LogEvent($"AfterEdit: {e.EditText}");
        
        // Selection events
        tabControl.SelectedIndexChanging += (s, e) => 
            LogEvent($"SelectedIndexChanging: {e.OldSelectedIndex} → {e.NewSelectedIndex}");
        tabControl.SelectedIndexChanged += (s, e) => 
            LogEvent($"SelectedIndexChanged: {tabControl.SelectedIndex}");
        
        // Tab primitive events
        tabControl.TabPrimitiveClick += (s, e) => 
            LogEvent($"TabPrimitiveClick: {e.TabPrimitive.Name}");
        
        // Movement events
        tabControl.TabMoving += (s, e) => 
            LogEvent($"TabMoving: {e.From} → {e.Target}");
        tabControl.TabsOrderChanged += (s, e) => 
            LogEvent("TabsOrderChanged");
        
        // Control events
        tabControl.ControlAdded += (s, e) => 
            LogEvent($"ControlAdded: {e.Control.GetType().Name}");
        tabControl.ControlRemoved += (s, e) => 
            LogEvent($"ControlRemoved: {e.Control.GetType().Name}");
        
        // Appearance events
        tabControl.BackColorChanged += (s, e) => 
            LogEvent("BackColorChanged");
    }
    
    private void AddTabs()
    {
        for (int i = 1; i <= 5; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Tab {i}";
            
            // Attach tab-specific events
            tab.Closing += (s, e) =>
            {
                var t = s as TabPageAdv;
                LogEvent($"Tab.Closing: {t.Text}");
            };
            
            tab.Closed += (s, e) =>
            {
                var t = s as TabPageAdv;
                LogEvent($"Tab.Closed: {t.Text}");
            };
            
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void LogEvent(string message)
    {
        string timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
        eventLog.Items.Insert(0, $"[{timestamp}] {message}");
        
        // Keep log manageable
        if (eventLog.Items.Count > 100)
        {
            eventLog.Items.RemoveAt(eventLog.Items.Count - 1);
        }
    }
}
```

## Best Practices

### Event Handling
- Always check for null when accessing event args
- Use lambda expressions for simple handlers
- Extract complex logic to separate methods
- Unsubscribe from events when appropriate

### Performance
- Avoid heavy processing in frequently-fired events (Paint, DrawItem)
- Use BeginInvoke for long-running operations
- Cache values instead of recalculating in events

### User Experience
- Provide feedback for cancelled operations
- Show progress for slow operations
- Handle exceptions gracefully
- Log events for debugging

### Common Patterns
- Validate in "Changing" events, act in "Changed" events
- Use "Closing" to prompt for save, "Closed" to cleanup
- Combine related events in single handler when possible
- Document custom event behavior

## Troubleshooting

### Event Not Firing
- Check if event is properly subscribed
- Verify control is initialized
- Ensure action actually triggers the event

### Event Fires Multiple Times
- Check for duplicate subscriptions
- Verify you're not programmatically triggering the event
- Use event handlers carefully in loops

### Cannot Cancel Event
- Only some events can be cancelled (those with Cancel property)
- BeforeEdit cannot be cancelled - use LabelEdit property instead
- Check documentation for each event's capabilities
