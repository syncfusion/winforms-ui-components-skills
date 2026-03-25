# Behavior and Events

## Table of Contents
- [Animation Configuration](#animation-configuration)
- [Collapse and Expand Events](#collapse-and-expand-events)
- [Item Click Handling](#item-click-handling)
- [State Persistence](#state-persistence)
- [Drag and Drop](#drag-and-drop)
- [Event Reference](#event-reference)

## Animation Configuration

Control the smoothness and speed of expand/collapse animations.

### Animation Delay

Set the delay between animation frames (in milliseconds):

```csharp
XPTaskBarBox box = new XPTaskBarBox();

// 50ms delay between frames
box.AnimationDelay = 50;
```

**VB.NET:**

```vb
box.AnimationDelay = 50
```

Lower values = faster animation, higher values = slower animation.

### Animation Steps

Control the number of intermediate positions during animation:

```csharp
// 15 animation steps for smooth motion
box.AnimationPositionsCount = 15;
```

**VB.NET:**

```vb
box.AnimationPositionsCount = 15
```

Higher values = smoother animation but slightly slower. Typical range is 10-25 steps.

### Additional Animation

Animate when items are added or removed:

```csharp
// Enable animation on item addition/removal
box.UseAdditionalAnimation = true;

// Add items - will animate into view
box.Items.Add(new XPTaskBarItem("New Item", System.Drawing.Color.Empty, -1, "new"));
```

**VB.NET:**

```vb
box.UseAdditionalAnimation = True
```

### Configuring Animation Events

Listen to animation events:

```csharp
box.BeforeAnimation += (sender, e) => {
    Console.WriteLine("Animation starting...");
};

box.AfterAnimation += (sender, e) => {
    Console.WriteLine("Animation complete!");
};
```

**VB.NET:**

```vb
AddHandler box.BeforeAnimation, Sub(sender, e)
    Console.WriteLine("Animation starting...")
End Sub

AddHandler box.AfterAnimation, Sub(sender, e)
    Console.WriteLine("Animation complete!")
End Sub
```

## Collapse and Expand Events

### CollapsedStateChanged Event

Fires after a box is collapsed or expanded:

```csharp
box.CollapsedStateChanged += (sender, e) => {
    XPTaskBarBox changedBox = sender as XPTaskBarBox;
    if (changedBox != null) {
        string state = changedBox.Collapsed ? "collapsed" : "expanded";
        Console.WriteLine($"Box '{changedBox.Text}' is now {state}");
    }
};
```

**VB.NET:**

```vb
AddHandler box.CollapsedStateChanged, Sub(sender, e)
    Dim changedBox As XPTaskBarBox = TryCast(sender, XPTaskBarBox)
    If changedBox IsNot Nothing Then
        Dim state As String = If(changedBox.Collapsed, "collapsed", "expanded")
        Console.WriteLine($"Box '{changedBox.Text}' is now {state}")
    End If
End Sub
```

### Programmatic Collapse/Expand

Collapse or expand boxes from code:

```csharp
// Collapse a box
box.Collapsed = true;

// Expand a box
box.Collapsed = false;

// Check current state
if (box.Collapsed) {
    Console.WriteLine("Box is collapsed");
}
```

**VB.NET:**

```vb
box.Collapsed = True
If box.Collapsed Then
    Console.WriteLine("Box is collapsed")
End If
```

### BeforeAnimation Event

Fires just before expand/collapse animation begins:

```csharp
box.BeforeAnimation += (sender, e) => {
    // Prepare UI for animation
    Console.WriteLine("Preparing animation...");
};
```

### AfterAnimation Event

Fires after expand/collapse animation completes:

```csharp
box.AfterAnimation += (sender, e) => {
    // Update UI after animation
    Console.WriteLine("Animation finished!");
};
```

## Item Click Handling

### ItemClick Event

Handle clicks on items using the ItemClick event:

```csharp
box.ItemClick += (sender, e) => {
    XPTaskBarItem clickedItem = e.XPTaskBarItem;
    Console.WriteLine($"Clicked: {clickedItem.Text}");
    Console.WriteLine($"Tag: {clickedItem.Tag}");
};
```

**VB.NET:**

```vb
AddHandler box.ItemClick, Sub(sender, e)
    Dim clickedItem As XPTaskBarItem = e.XPTaskBarItem
    Console.WriteLine($"Clicked: {clickedItem.Text}")
    Console.WriteLine($"Tag: {clickedItem.Tag}")
End Sub
```

### Event Arguments

The `XPTaskBarItemClickArgs` provides:

- **XPTaskBarItem** - The item that was clicked
- **ItemText** - Text of the clicked item (convenience)

### Tag-Based Command Routing

Use the Tag property for clean event routing:

```csharp
box.ItemClick += (sender, e) => {
    string command = e.XPTaskBarItem.Tag as string ?? "";
    
    switch (command) {
        case "file_new":
            OnFileNew();
            break;
        case "file_open":
            OnFileOpen();
            break;
        case "file_save":
            OnFileSave();
            break;
        case "file_exit":
            Application.Exit();
            break;
        default:
            MessageBox.Show($"Unknown command: {command}");
            break;
    }
};

private void OnFileNew() {
    // Implement file creation logic
}

private void OnFileOpen() {
    // Implement file open logic
}

private void OnFileSave() {
    // Implement file save logic
}
```

**VB.NET:**

```vb
AddHandler box.ItemClick, Sub(sender, e)
    Dim command As String = CStr(IIf(e.XPTaskBarItem.Tag IsNot Nothing, e.XPTaskBarItem.Tag, ""))
    
    Select Case command
        Case "file_new"
            OnFileNew()
        Case "file_open"
            OnFileOpen()
        Case "file_save"
            OnFileSave()
        Case "file_exit"
            Application.Exit()
        Case Else
            MessageBox.Show($"Unknown command: {command}")
    End Select
End Sub

Private Sub OnFileNew()
    ' Implement file creation logic
End Sub
```

### Handling Multiple Boxes

Subscribe to all box item clicks:

```csharp
private void SubscribeToAllBoxes() {
    foreach (Control control in xpTaskBar1.Controls) {
        if (control is XPTaskBarBox box) {
            box.ItemClick += HandleItemClick;
        }
    }
}

private void HandleItemClick(object sender, Syncfusion.Windows.Forms.Tools.XPTaskBarItemClickArgs e) {
    XPTaskBarBox box = sender as XPTaskBarBox;
    Console.WriteLine($"Box: {box?.Text}, Item: {e.XPTaskBarItem.Text}");
}
```

## State Persistence

### AutoPersistStates

Automatically save and restore expanded/collapsed states across application sessions:

```csharp
// Enable state persistence
xpTaskBar1.AutoPersistStates = true;
```

**VB.NET:**

```vb
xpTaskBar1.AutoPersistStates = True
```

When enabled, XPTaskBar automatically uses AppStateSerializer to save the state of each box.

### Manual State Management

Manually save and load states:

```csharp
// Save states
box.SaveBoxExpandedStates();

// Load states
box.LoadBoxExpandedStates();
```

**VB.NET:**

```vb
box.SaveBoxExpandedStates()
box.LoadBoxExpandedStates()
```

### Preserve User Preferences

```csharp
// On application startup
protected override void OnLoad(EventArgs e) {
    base.OnLoad(e);
    
    // Restore user's previous layout
    if (xpTaskBar1.AutoPersistStates) {
        foreach (Control control in xpTaskBar1.Controls.OfType<XPTaskBarBox>()) {
            control.LoadBoxExpandedStates();
        }
    }
}

// On application shutdown
protected override void OnFormClosing(FormClosingEventArgs e) {
    base.OnFormClosing(e);
    
    // Save current layout
    foreach (Control control in xpTaskBar1.Controls.OfType<XPTaskBarBox>()) {
        control.SaveBoxExpandedStates();
    }
}
```

## Drag and Drop

### Enabling Drag and Drop

Allow dragging items into the XPTaskBar:

```csharp
// Enable drag-and-drop
xpTaskBar1.AllowDrop = true;
```

**VB.NET:**

```vb
xpTaskBar1.AllowDrop = True
```

### Handling Dropped Items

```csharp
// Handle drag-and-drop events
xpTaskBar1.DragDrop += (sender, e) => {
    if (e.Data.GetDataPresent(DataFormats.FileDrop)) {
        string[] files = (string[])e.Data.GetData(DataFormats.FileDrop);
        foreach (string file in files) {
            Console.WriteLine($"Dropped file: {file}");
        }
    }
};

xpTaskBar1.DragOver += (sender, e) => {
    if (e.Data.GetDataPresent(DataFormats.FileDrop)) {
        e.Effect = DragDropEffects.Copy;
    } else {
        e.Effect = DragDropEffects.None;
    }
};
```

**VB.NET:**

```vb
AddHandler xpTaskBar1.DragDrop, Sub(sender, e)
    If e.Data.GetDataPresent(DataFormats.FileDrop) Then
        Dim files As String() = CType(e.Data.GetData(DataFormats.FileDrop), String())
        For Each file In files
            Console.WriteLine($"Dropped file: {file}")
        Next
    End If
End Sub
```

## MinimumSize Event

### MinimumSizeChanged Event

Fires when the MinimumSize property changes:

```csharp
// Set minimum size
xpTaskBar1.MinimumSize = new System.Drawing.Size(200, 300);

// Handle change event
xpTaskBar1.MinimumSizeChanged += (sender, e) => {
    Console.WriteLine($"New minimum size: {xpTaskBar1.MinimumSize}");
};
```

**VB.NET:**

```vb
xpTaskBar1.MinimumSize = New System.Drawing.Size(200, 300)

AddHandler xpTaskBar1.MinimumSizeChanged, Sub(sender, e)
    Console.WriteLine($"New minimum size: {xpTaskBar1.MinimumSize}")
End Sub
```

## Event Reference

### XPTaskBar Events

| Event | When Fired | Handler Signature |
|-------|-----------|-------------------|
| `MinimumSizeChanged` | MinimumSize property changes | EventHandler(EventArgs) |

### XPTaskBarBox Events

| Event | When Fired | Handler Signature |
|-------|-----------|-------------------|
| `BeforeAnimation` | Before expand/collapse animation | EventHandler(EventArgs) |
| `AfterAnimation` | After expand/collapse animation | EventHandler(EventArgs) |
| `CollapsedStateChanged` | After box is collapsed or expanded | EventHandler(EventArgs) |
| `ItemClick` | When user clicks an item | XPTaskBarItemClickEventHandler(XPTaskBarItemClickArgs) |

### Standard Control Events

Both XPTaskBar and XPTaskBarBox inherit standard .NET control events:

- `DragDrop` - Item dropped on control
- `DragOver` - Item dragged over control
- `DragEnter` - Item enters control bounds
- `MouseClick` - Mouse button clicked
- `MouseHover` - Mouse hovers over control

## Complete Event Handling Example

```csharp
private void InitializeXPTaskBar() {
    var box = new XPTaskBarBox { Text = "Task Actions" };
    
    // Animation events
    box.BeforeAnimation += (s, e) => {
        Console.WriteLine("Animation started");
    };
    
    box.AfterAnimation += (s, e) => {
        Console.WriteLine("Animation finished");
    };
    
    // State change events
    box.CollapsedStateChanged += (s, e) => {
        var changedBox = s as XPTaskBarBox;
        Console.WriteLine($"{changedBox?.Text}: {(changedBox?.Collapsed == true ? "Collapsed" : "Expanded")}");
    };
    
    // Item click events
    box.ItemClick += (s, e) => {
        Console.WriteLine($"Clicked: {e.XPTaskBarItem.Tag}");
    };
    
    // Configure animation
    box.AnimationDelay = 50;
    box.AnimationPositionsCount = 15;
    box.UseAdditionalAnimation = true;
    
    // Add to control
    xpTaskBar1.Controls.Add(box);
}
```

## Next Steps

- See [appearance-customization.md](appearance-customization.md) for brush and brush events
- See [items-and-content.md](items-and-content.md) for item management
- See [padding-spacing-scrolling.md](padding-spacing-scrolling.md) for layout control
