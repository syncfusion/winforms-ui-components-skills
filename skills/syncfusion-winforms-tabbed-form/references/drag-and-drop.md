# Drag and Drop Tabs

This guide covers the drag-and-drop functionality for tab reordering in the Tabbed Form control.

## Overview

The Tabbed Form allows users to reorder tabs by dragging them to different positions. This feature provides an intuitive way for users to organize their workspace according to their preferences.

## Enabling Drag and Drop

Enable drag-and-drop by setting the `AllowDraggingTabs` property to `true`.

### Basic Configuration

**C#:**
```csharp
tabbedFormControl.AllowDraggingTabs = true;
```

**VB.NET:**
```vb
tabbedFormControl.AllowDraggingTabs = True
```

### Complete Setup Example

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        InitializeTabbedForm();
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        // Add tabs
        for (int i = 1; i <= 5; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Document {i}";
            tabbedFormControl.Tabs.Add(tab);
        }
        
        // Enable drag and drop
        tabbedFormControl.AllowDraggingTabs = true;
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Partial Public Class Form1
    Inherits SfTabbedForm
    Private tabbedFormControl As SfTabbedFormControl
    
    Public Sub New()
        InitializeComponent()
        InitializeTabbedForm()
    End Sub
    
    Private Sub InitializeTabbedForm()
        tabbedFormControl = New SfTabbedFormControl()
        
        ' Add tabs
        For i As Integer = 1 To 5
            Dim tab As New TabPageAdv()
            tab.Text = $"Document {i}"
            tabbedFormControl.Tabs.Add(tab)
        Next i
        
        ' Enable drag and drop
        tabbedFormControl.AllowDraggingTabs = True
        
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

## TabDragging Event

The `TabDragging` event fires during drag-and-drop operations, allowing you to control the dragging behavior and cancel operations if needed.

### Event Signature

**C#:**
```csharp
this.tabbedFormControl.TabDragging += TabbedFormControl_TabDragging;

private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    // Event logic here
}
```

**VB.NET:**
```vb
AddHandler Me.tabbedFormControl.TabDragging, AddressOf TabbedFormControl_TabDragging

Private Sub TabbedFormControl_TabDragging(ByVal sender As Object, ByVal e As TabDraggingEventArgs)
    ' Event logic here
End Sub
```

### TabDraggingEventArgs Properties

- **From**: The original index position of the tab being dragged
- **To**: The target index position where the tab will be dropped
- **Action**: The current drag action (DragStarting or DragDropping)
- **Cancel**: Set to `true` to cancel the drag operation

### TabDraggingAction Enumeration

The `Action` property indicates the stage of the drag operation:

- **DragStarting**: Fires when the user starts dragging a tab (before the drag begins)
- **DragDropping**: Fires when the user is about to drop the tab (before the reorder completes)

## Cancel Tab Dragging

You can prevent specific tabs from being dragged by handling the `TabDragging` event and setting `e.Cancel = true` when `e.Action` is `TabDraggingAction.DragStarting`.

### Preventing Specific Tab from Being Dragged

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    // Prevent dragging the second tab (index 1)
    if (e.From == 1 && e.Action == TabDraggingAction.DragStarting)
    {
        e.Cancel = true;
        MessageBox.Show("This tab cannot be moved.", "Locked Tab", 
                        MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
}
```

**VB.NET:**
```vb
Private Sub TabbedFormControl_TabDragging(ByVal sender As Object, ByVal e As TabDraggingEventArgs)
    ' Prevent dragging the second tab (index 1)
    If e.From = 1 AndAlso e.Action = TabDraggingAction.DragStarting Then
        e.Cancel = True
        MessageBox.Show("This tab cannot be moved.", "Locked Tab", 
                        MessageBoxButtons.OK, MessageBoxIcon.Information)
    End If
End Sub
```

### Locking Multiple Tabs

**C#:**
```csharp
private HashSet<int> lockedTabs = new HashSet<int> { 0, 2, 4 };

private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    if (e.Action == TabDraggingAction.DragStarting && lockedTabs.Contains(e.From))
    {
        e.Cancel = true;
        TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
        MessageBox.Show($"'{tab?.Text}' is locked and cannot be moved.");
    }
}
```

### Locking by Tab Properties

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    if (e.Action == TabDraggingAction.DragStarting)
    {
        TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
        
        // Lock tabs with specific names
        if (tab?.Text == "Home" || tab?.Text == "Settings")
        {
            e.Cancel = true;
            MessageBox.Show($"'{tab.Text}' cannot be moved from its position.");
        }
    }
}
```

## Cancel Tab Reordering

You can prevent a tab from being dropped at a specific position by handling the `TabDragging` event and setting `e.Cancel = true` when `e.Action` is `TabDraggingAction.Dropping`.

### Preventing Drop at Specific Position

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    // Prevent dropping at the second position (index 1)
    if (e.To == 1 && e.Action == TabDraggingAction.Dropping)
    {
        e.Cancel = true;
        MessageBox.Show("Tabs cannot be placed at this position.", "Invalid Position",
                        MessageBoxButtons.OK, MessageBoxIcon.Warning);
    }
}
```

**VB.NET:**
```vb
Private Sub TabbedFormControl_TabDragging(ByVal sender As Object, ByVal e As TabDraggingEventArgs)
    ' Prevent dropping at the second position (index 1)
    If e.To = 1 AndAlso e.Action = TabDraggingAction.Dropping Then
        e.Cancel = True
        MessageBox.Show("Tabs cannot be placed at this position.", "Invalid Position",
                        MessageBoxButtons.OK, MessageBoxIcon.Warning)
    End If
End Sub
```

### Restricting First Position

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    // Keep the first tab always in first position
    if (e.Action == TabDraggingAction.Dropping)
    {
        // Don't allow any tab to be dropped at position 0
        if (e.To == 0 && e.From != 0)
        {
            e.Cancel = true;
            MessageBox.Show("The first position is reserved.", "Position Reserved",
                            MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
        
        // Don't allow the first tab to be moved
        if (e.From == 0 && e.To != 0)
        {
            e.Cancel = true;
            MessageBox.Show("The home tab must remain first.", "Home Tab Fixed",
                            MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
}
```

### Creating Tab Groups (Zones)

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    if (e.Action == TabDraggingAction.Dropping)
    {
        // Define zones: 0-2 are "System" tabs, 3+ are "User" tabs
        bool fromSystemZone = e.From <= 2;
        bool toSystemZone = e.To <= 2;
        
        // Prevent moving between zones
        if (fromSystemZone != toSystemZone)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot move tabs between System and User zones.",
                            "Zone Restriction", MessageBoxButtons.OK, MessageBoxIcon.Warning);
        }
    }
}
```

## Complete Example with Drag Control

**C#:**
```csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    private HashSet<int> pinnedTabs = new HashSet<int>();
    
    public Form1()
    {
        InitializeComponent();
        InitializeTabbedForm();
        AttachEvents();
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        // Add tabs
        string[] tabNames = { "Home", "Document1", "Document2", "Settings", "Help" };
        foreach (string name in tabNames)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = name;
            tabbedFormControl.Tabs.Add(tab);
        }
        
        // Pin first and last tabs
        pinnedTabs.Add(0); // Home
        pinnedTabs.Add(4); // Help
        
        // Enable drag and drop
        tabbedFormControl.AllowDraggingTabs = true;
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
    
    private void AttachEvents()
    {
        tabbedFormControl.TabDragging += TabbedFormControl_TabDragging;
    }
    
    private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
    {
        // Handle drag starting
        if (e.Action == TabDraggingAction.DragStarting)
        {
            // Prevent dragging pinned tabs
            if (pinnedTabs.Contains(e.From))
            {
                e.Cancel = true;
                TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
                MessageBox.Show($"'{tab?.Text}' is pinned and cannot be moved.",
                                "Pinned Tab", MessageBoxButtons.OK, MessageBoxIcon.Information);
                return;
            }
            
            // Log drag start
            Console.WriteLine($"Started dragging tab from position {e.From}");
        }
        
        // Handle dropping
        if (e.Action == TabDraggingAction.Dropping)
        {
            // Prevent dropping on pinned positions
            if (pinnedTabs.Contains(e.To))
            {
                e.Cancel = true;
                MessageBox.Show("Cannot place tabs at this pinned position.",
                                "Position Reserved", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                return;
            }
            
            // Log successful drop
            TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
            Console.WriteLine($"Moving '{tab?.Text}' from position {e.From} to {e.To}");
        }
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Collections.Generic
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Partial Public Class Form1
    Inherits SfTabbedForm
    Private tabbedFormControl As SfTabbedFormControl
    Private pinnedTabs As New HashSet(Of Integer)()
    
    Public Sub New()
        InitializeComponent()
        InitializeTabbedForm()
        AttachEvents()
    End Sub
    
    Private Sub InitializeTabbedForm()
        tabbedFormControl = New SfTabbedFormControl()
        
        ' Add tabs
        Dim tabNames() As String = {"Home", "Document1", "Document2", "Settings", "Help"}
        For Each name As String In tabNames
            Dim tab As New TabPageAdv()
            tab.Text = name
            tabbedFormControl.Tabs.Add(tab)
        Next name
        
        ' Pin first and last tabs
        pinnedTabs.Add(0) ' Home
        pinnedTabs.Add(4) ' Help
        
        ' Enable drag and drop
        tabbedFormControl.AllowDraggingTabs = True
        
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
    
    Private Sub AttachEvents()
        AddHandler tabbedFormControl.TabDragging, AddressOf TabbedFormControl_TabDragging
    End Sub
    
    Private Sub TabbedFormControl_TabDragging(ByVal sender As Object, ByVal e As TabDraggingEventArgs)
        ' Handle drag starting
        If e.Action = TabDraggingAction.DragStarting Then
            ' Prevent dragging pinned tabs
            If pinnedTabs.Contains(e.From) Then
                e.Cancel = True
                Dim tab As TabPageAdv = TryCast(tabbedFormControl.Tabs(e.From), TabPageAdv)
                MessageBox.Show($"'{tab?.Text}' is pinned and cannot be moved.",
                                "Pinned Tab", MessageBoxButtons.OK, MessageBoxIcon.Information)
                Return
            End If
            
            ' Log drag start
            Console.WriteLine($"Started dragging tab from position {e.From}")
        End If
        
        ' Handle dropping
        If e.Action = TabDraggingAction.Dropping Then
            ' Prevent dropping on pinned positions
            If pinnedTabs.Contains(e.To) Then
                e.Cancel = True
                MessageBox.Show("Cannot place tabs at this pinned position.",
                                "Position Reserved", MessageBoxButtons.OK, MessageBoxIcon.Warning)
                Return
            End If
            
            ' Log successful drop
            Dim tab As TabPageAdv = TryCast(tabbedFormControl.Tabs(e.From), TabPageAdv)
            Console.WriteLine($"Moving '{tab?.Text}' from position {e.From} to {e.To}")
        End If
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Logging Drag Operations

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
    
    if (e.Action == TabDraggingAction.DragStarting)
    {
        Console.WriteLine($"[{DateTime.Now}] User started dragging '{tab?.Text}'");
    }
    else if (e.Action == TabDraggingAction.Dropping)
    {
        Console.WriteLine($"[{DateTime.Now}] '{tab?.Text}' moved from {e.From} to {e.To}");
    }
}
```

### Pattern 2: Confirmation Before Reorder

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    if (e.Action == TabDraggingAction.Dropping)
    {
        TabPageAdv tab = tabbedFormControl.Tabs[e.From] as TabPageAdv;
        
        DialogResult result = MessageBox.Show(
            $"Move '{tab?.Text}' to position {e.To + 1}?",
            "Confirm Reorder",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true;
        }
    }
}
```

### Pattern 3: Save Tab Order on Drag

**C#:**
```csharp
private void TabbedFormControl_TabDragging(object sender, TabDraggingEventArgs e)
{
    if (e.Action == TabDraggingAction.Dropping && !e.Cancel)
    {
        // Save the new order after drag completes
        BeginInvoke(new Action(() => SaveTabOrder()));
    }
}

private void SaveTabOrder()
{
    List<string> tabOrder = new List<string>();
    foreach (TabPageAdv tab in tabbedFormControl.Tabs)
    {
        tabOrder.Add(tab.Text);
    }
    
    // Save to settings/database
    Console.WriteLine("Saved tab order: " + string.Join(", ", tabOrder));
}
```

## Combining with Other Features

### Drag and Drop with Navigation Buttons

**C#:**
```csharp
tabbedFormControl.AllowDraggingTabs = true;
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown | 
                                     TabPrimitiveMode.FirstTab | 
                                     TabPrimitiveMode.LastTab;
```

### Drag and Drop with Context Menu

**C#:**
```csharp
// Enable both features
tabbedFormControl.AllowDraggingTabs = true;

ContextMenuStrip contextMenu = new ContextMenuStrip();
contextMenu.Items.Add("Pin Tab", null, OnPinTab);
contextMenu.Items.Add("Unpin Tab", null, OnUnpinTab);
tabbedFormControl.TabContextMenu = contextMenu;
```

## Best Practices

1. **Provide Visual Feedback**: The control handles visual feedback automatically, showing where the tab will be dropped.

2. **Lock Important Tabs**: Use pinning or locking for tabs that should maintain fixed positions (like Home or Settings).

3. **Save User Preferences**: Persist the tab order so users' arrangements are preserved across sessions.

4. **Inform Users**: If dragging is disabled for certain tabs, provide clear messaging explaining why.

5. **Test Edge Cases**: Ensure dragging works correctly with the first and last tabs.

6. **Combine Features**: Drag-and-drop works well with navigation buttons and context menus for complete tab management.

## Troubleshooting

### Issue: Dragging Not Working

**Solution**: Verify that `AllowDraggingTabs = true` is set and the control has been added to the form.

### Issue: Cancel Not Preventing Drag

**Solution**: Ensure you're checking the correct `Action` (DragStarting vs DragDropping) for your use case.

### Issue: Tab Order Not Updating

**Solution**: The tab order updates automatically when drag completes. If you need to persist it, save the order after the TabDragging event with Action = DragDropping.

### Issue: Event Firing Multiple Times

**Solution**: The event fires twice per drag operation (once for DragStarting, once for DragDropping). Check the `Action` property to handle each phase appropriately.
