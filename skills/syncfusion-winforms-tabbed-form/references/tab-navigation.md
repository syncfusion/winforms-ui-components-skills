# Tab Navigation

This guide covers the built-in navigation controls for the Tabbed Form, including navigation buttons and related events.

## Overview

Tabbed Form provides a set of built-in navigation buttons (First, Last, Next, Previous, and DropDown) that help users navigate through tabs, especially when there are many tabs or when tabs overflow the visible area.

## TabPrimitiveMode Property

The `TabPrimitiveMode` property controls which navigation buttons are visible. This property uses a flag enumeration, allowing you to combine multiple navigation options.

### Available Navigation Options

- **FirstTab**: Button to jump to the first tab
- **LastTab**: Button to jump to the last tab
- **NextTab**: Button to navigate to the next tab
- **PreviousTab**: Button to navigate to the previous tab
- **DropDown**: Dropdown menu showing all tabs
- **None**: No navigation buttons (default)

### Basic Configuration

**C#:**
```csharp
// Add all navigation buttons
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown | 
                                     TabPrimitiveMode.FirstTab | 
                                     TabPrimitiveMode.LastTab | 
                                     TabPrimitiveMode.NextTab | 
                                     TabPrimitiveMode.PreviousTab;
```

**VB.NET:**
```vb
' Add all navigation buttons
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown Or 
                                     TabPrimitiveMode.FirstTab Or 
                                     TabPrimitiveMode.LastTab Or 
                                     TabPrimitiveMode.NextTab Or 
                                     TabPrimitiveMode.PreviousTab
```

### Common Configurations

**Minimal Navigation (DropDown only):**
```csharp
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown;
```

**Previous/Next Navigation:**
```csharp
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.NextTab | 
                                     TabPrimitiveMode.PreviousTab;
```

**First/Last Navigation:**
```csharp
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.FirstTab | 
                                     TabPrimitiveMode.LastTab;
```

**Full Navigation with DropDown:**
```csharp
tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.FirstTab | 
                                     TabPrimitiveMode.PreviousTab | 
                                     TabPrimitiveMode.NextTab | 
                                     TabPrimitiveMode.LastTab | 
                                     TabPrimitiveMode.DropDown;
```

## Complete Setup Example

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
        
        // Add multiple tabs
        for (int i = 1; i <= 15; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Document {i}";
            tabbedFormControl.Tabs.Add(tab);
        }
        
        // Enable all navigation options
        tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown | 
                                             TabPrimitiveMode.FirstTab | 
                                             TabPrimitiveMode.LastTab | 
                                             TabPrimitiveMode.NextTab | 
                                             TabPrimitiveMode.PreviousTab;
        
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
        
        ' Add multiple tabs
        For i As Integer = 1 To 15
            Dim tab As New TabPageAdv()
            tab.Text = $"Document {i}"
            tabbedFormControl.Tabs.Add(tab)
        Next i
        
        ' Enable all navigation options
        tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown Or 
                                             TabPrimitiveMode.FirstTab Or 
                                             TabPrimitiveMode.LastTab Or 
                                             TabPrimitiveMode.NextTab Or 
                                             TabPrimitiveMode.PreviousTab
        
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

## TabPrimitiveClick Event

The `TabPrimitiveClick` event fires when a user clicks any of the navigation buttons. This event allows you to track navigation actions or customize behavior.

### Event Signature

**C#:**
```csharp
this.tabbedFormControl.TabPrimitiveClick += TabbedFormControl_TabPrimitiveClick;

private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
{
    // Event logic here
}
```

**VB.NET:**
```vb
AddHandler Me.tabbedFormControl.TabPrimitiveClick, AddressOf TabbedFormControl_TabPrimitiveClick

Private Sub TabbedFormControl_TabPrimitiveClick(ByVal sender As Object, ByVal e As TabPrimitiveClickEventArgs)
    ' Event logic here
End Sub
```

### TabPrimitiveClickEventArgs Properties

- **TabPrimitive**: Gets the `TabPrimitive` object that was clicked
- **TabPrimitive.TabPrimitiveType**: Gets the type of navigation button that was clicked

### TabPrimitiveType Enumeration

The `TabPrimitiveType` indicates which navigation button was clicked:
- **FirstTab**: First tab button
- **LastTab**: Last tab button
- **NextTab**: Next tab button
- **PreviousTab**: Previous tab button
- **DropDown**: Dropdown button

### Logging Navigation Actions

**C#:**
```csharp
private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
{
    Console.WriteLine("TabPrimitive Type: " + e.TabPrimitive.TabPrimitiveType);
    
    // Log with timestamp
    string action = e.TabPrimitive.TabPrimitiveType.ToString();
    Console.WriteLine($"{DateTime.Now}: User clicked {action}");
}
```

**VB.NET:**
```vb
Private Sub TabbedFormControl_TabPrimitiveClick(ByVal sender As Object, ByVal e As TabPrimitiveClickEventArgs)
    Console.WriteLine("TabPrimitive Type:" & e.TabPrimitive.TabPrimitiveType)
    
    ' Log with timestamp
    Dim action As String = e.TabPrimitive.TabPrimitiveType.ToString()
    Console.WriteLine($"{DateTime.Now}: User clicked {action}")
End Sub
```

### Responding to Specific Navigation Actions

**C#:**
```csharp
private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
{
    switch (e.TabPrimitive.TabPrimitiveType)
    {
        case TabPrimitiveType.FirstTab:
            Console.WriteLine("Navigated to first tab");
            break;
            
        case TabPrimitiveType.LastTab:
            Console.WriteLine("Navigated to last tab");
            break;
            
        case TabPrimitiveType.NextTab:
            Console.WriteLine("Navigated to next tab");
            break;
            
        case TabPrimitiveType.PreviousTab:
            Console.WriteLine("Navigated to previous tab");
            break;
            
        case TabPrimitiveType.DropDown:
            Console.WriteLine("Dropdown menu opened");
            break;
    }
}
```

### Tracking Navigation Statistics

**C#:**
```csharp
private Dictionary<TabPrimitiveType, int> navigationStats = new Dictionary<TabPrimitiveType, int>();

private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
{
    TabPrimitiveType type = e.TabPrimitive.TabPrimitiveType;
    
    if (!navigationStats.ContainsKey(type))
        navigationStats[type] = 0;
    
    navigationStats[type]++;
    
    Console.WriteLine($"Navigation stats: {type} used {navigationStats[type]} times");
}

private void ShowNavigationStats()
{
    Console.WriteLine("\nNavigation Statistics:");
    foreach (var kvp in navigationStats)
    {
        Console.WriteLine($"{kvp.Key}: {kvp.Value} clicks");
    }
}
```

## Complete Example with Event Handling

**C#:**
```csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    private Label statusLabel;
    
    public Form1()
    {
        InitializeComponent();
        InitializeStatusLabel();
        InitializeTabbedForm();
        AttachEvents();
    }
    
    private void InitializeStatusLabel()
    {
        statusLabel = new Label();
        statusLabel.Dock = DockStyle.Bottom;
        statusLabel.Height = 25;
        statusLabel.Text = "Ready";
        this.Controls.Add(statusLabel);
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        // Add tabs
        for (int i = 1; i <= 20; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Tab {i}";
            tabbedFormControl.Tabs.Add(tab);
        }
        
        // Enable navigation
        tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.FirstTab | 
                                             TabPrimitiveMode.PreviousTab | 
                                             TabPrimitiveMode.NextTab | 
                                             TabPrimitiveMode.LastTab | 
                                             TabPrimitiveMode.DropDown;
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
    
    private void AttachEvents()
    {
        tabbedFormControl.TabPrimitiveClick += TabbedFormControl_TabPrimitiveClick;
        tabbedFormControl.SelectedIndexChanged += TabbedFormControl_SelectedIndexChanged;
    }
    
    private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
    {
        string buttonType = e.TabPrimitive.TabPrimitiveType.ToString();
        statusLabel.Text = $"Navigation: {buttonType} clicked";
    }
    
    private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
    {
        int current = tabbedFormControl.SelectedIndex + 1;
        int total = tabbedFormControl.Tabs.Count;
        statusLabel.Text = $"Tab {current} of {total}";
    }
}
```

## Dynamic Navigation Control

You can enable or disable navigation buttons dynamically based on conditions:

**C#:**
```csharp
private void UpdateNavigationButtons()
{
    int selectedIndex = tabbedFormControl.SelectedIndex;
    int tabCount = tabbedFormControl.Tabs.Count;
    
    TabPrimitiveMode mode = TabPrimitiveMode.DropDown;
    
    // Enable first/previous only if not on first tab
    if (selectedIndex > 0)
    {
        mode |= TabPrimitiveMode.FirstTab | TabPrimitiveMode.PreviousTab;
    }
    
    // Enable next/last only if not on last tab
    if (selectedIndex < tabCount - 1)
    {
        mode |= TabPrimitiveMode.NextTab | TabPrimitiveMode.LastTab;
    }
    
    tabbedFormControl.TabPrimitiveMode = mode;
}

// Call this when selection changes
private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
{
    UpdateNavigationButtons();
}
```

## Common Patterns

### Pattern 1: Keyboard Shortcuts for Navigation

**C#:**
```csharp
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    switch (keyData)
    {
        case Keys.Control | Keys.Home:
            tabbedFormControl.SelectedIndex = 0;
            return true;
            
        case Keys.Control | Keys.End:
            tabbedFormControl.SelectedIndex = tabbedFormControl.Tabs.Count - 1;
            return true;
            
        case Keys.Control | Keys.PageUp:
            if (tabbedFormControl.SelectedIndex > 0)
                tabbedFormControl.SelectedIndex--;
            return true;
            
        case Keys.Control | Keys.PageDown:
            if (tabbedFormControl.SelectedIndex < tabbedFormControl.Tabs.Count - 1)
                tabbedFormControl.SelectedIndex++;
            return true;
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

### Pattern 2: Custom Navigation Toolbar

**C#:**
```csharp
private void AddCustomNavigationToolbar()
{
    ToolStrip toolbar = new ToolStrip();
    toolbar.Items.Add("First", null, (s, e) => tabbedFormControl.SelectedIndex = 0);
    toolbar.Items.Add("Previous", null, (s, e) => NavigatePrevious());
    toolbar.Items.Add("Next", null, (s, e) => NavigateNext());
    toolbar.Items.Add("Last", null, (s, e) => 
        tabbedFormControl.SelectedIndex = tabbedFormControl.Tabs.Count - 1);
    
    this.Controls.Add(toolbar);
}

private void NavigatePrevious()
{
    if (tabbedFormControl.SelectedIndex > 0)
        tabbedFormControl.SelectedIndex--;
}

private void NavigateNext()
{
    if (tabbedFormControl.SelectedIndex < tabbedFormControl.Tabs.Count - 1)
        tabbedFormControl.SelectedIndex++;
}
```

### Pattern 3: Conditional Navigation Based on Tab State

**C#:**
```csharp
private void TabbedFormControl_TabPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
{
    // Skip locked tabs when navigating
    if (e.TabPrimitive.TabPrimitiveType == TabPrimitiveType.NextTab)
    {
        int nextIndex = tabbedFormControl.SelectedIndex + 1;
        while (nextIndex < tabbedFormControl.Tabs.Count && IsTabLocked(nextIndex))
        {
            nextIndex++;
        }
        
        if (nextIndex < tabbedFormControl.Tabs.Count)
        {
            tabbedFormControl.SelectedIndex = nextIndex;
        }
    }
}

private bool IsTabLocked(int index)
{
    // Your logic to determine if tab is locked
    return false;
}
```

## Best Practices

1. **Enable Navigation for Many Tabs**: Always enable navigation buttons when you have more than 5-7 tabs, as users may not be able to see all tabs at once.

2. **DropDown is Essential**: The dropdown menu is particularly useful when tabs overflow, as it shows all tabs regardless of visibility.

3. **Combine with Drag-Drop**: Navigation buttons work well with drag-and-drop functionality for complete tab management.

4. **Update Status**: Use the `TabPrimitiveClick` event to update status bars or provide user feedback.

5. **Keyboard Support**: Consider adding keyboard shortcuts in addition to navigation buttons for power users.

## Troubleshooting

### Issue: Navigation Buttons Not Visible

**Solution**: Ensure the `TabPrimitiveMode` property is set with the desired flags using the bitwise OR operator (`|`).

### Issue: Event Not Firing

**Solution**: Verify the event handler is attached after creating the `TabbedFormControl` instance.

### Issue: Buttons Disabled

**Solution**: Navigation buttons automatically disable at boundaries (e.g., "Previous" on first tab). This is expected behavior and not an error.
