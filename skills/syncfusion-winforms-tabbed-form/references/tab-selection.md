# Tab Selection

This guide covers programmatic tab selection and selection-related events in the Tabbed Form control.

## Overview

Tab selection can be controlled programmatically using properties or handled through events. The control provides both pre-selection and post-selection events, allowing you to validate or respond to tab changes.

## Programmatic Tab Selection

The Tabbed Form provides two ways to select a tab programmatically:

### Using SelectedIndex Property

Select a tab by its zero-based index position:

**C#:**
```csharp
// Select the second tab (index 1)
this.tabbedFormControl.SelectedIndex = 1;
```

**VB.NET:**
```vb
' Select the second tab (index 1)
Me.tabbedFormControl.SelectedIndex = 1
```

### Using SelectedTab Property

Select a tab by its reference:

**C#:**
```csharp
// Select by tab reference
this.tabbedFormControl.SelectedTab = tabPageAdv2;
```

**VB.NET:**
```vb
' Select by tab reference
Me.tabbedFormControl.SelectedTab = tabPageAdv2
```

### Getting Current Selection

Retrieve the currently selected tab:

**C#:**
```csharp
// Get selected index
int currentIndex = this.tabbedFormControl.SelectedIndex;

// Get selected tab reference
TabPageAdv currentTab = this.tabbedFormControl.SelectedTab;

// Get tab text
string tabText = this.tabbedFormControl.SelectedTab?.Text;
```

**VB.NET:**
```vb
' Get selected index
Dim currentIndex As Integer = Me.tabbedFormControl.SelectedIndex

' Get selected tab reference
Dim currentTab As TabPageAdv = Me.tabbedFormControl.SelectedTab

' Get tab text
Dim tabText As String = Me.tabbedFormControl.SelectedTab?.Text
```

## SelectedIndexChanging Event

The `SelectedIndexChanging` event occurs **before** the tab selection changes. This event can be cancelled to prevent the tab change.

### Event Signature

**C#:**
```csharp
this.tabbedFormControl.SelectedIndexChanging += TabbedFormControl_SelectedIndexChanging;

private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
{
    // Event logic here
}
```

**VB.NET:**
```vb
AddHandler Me.tabbedFormControl.SelectedIndexChanging, AddressOf TabbedFormControl_SelectedIndexChanging

Private Sub TabbedFormControl_SelectedIndexChanging(ByVal sender As Object, ByVal args As SelectedIndexChangingEventArgs)
    ' Event logic here
End Sub
```

### SelectedIndexChangingEventArgs Properties

- **OldValue**: The index of the currently selected tab (before change)
- **NewValue**: The index of the tab being selected
- **Cancel**: Set to `true` to prevent the tab change

### Preventing Tab Selection

You can cancel tab selection based on conditions:

**C#:**
```csharp
private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
{
    // Prevent selecting the third tab (index 2)
    if (this.tabbedFormControl.SelectedIndex == 2)
    {
        args.Cancel = true;
        MessageBox.Show("This tab is currently locked.");
    }
}
```

**VB.NET:**
```vb
Private Sub TabbedFormControl_SelectedIndexChanging(ByVal sender As Object, ByVal args As SelectedIndexChangingEventArgs)
    ' Prevent selecting the third tab (index 2)
    If tabbedFormControl.SelectedIndex == 2 Then
        args.Cancel = True
        MessageBox.Show("This tab is currently locked.")
    End If
End Sub
```

### Conditional Tab Access Example

**C#:**
```csharp
private bool isAdminMode = false;

private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
{
    // Get the tab being selected
    TabPageAdv targetTab = this.tabbedFormControl.Tabs[args.NewValue] as TabPageAdv;
    
    // Restrict access to admin tab
    if (targetTab?.Text == "Admin Settings" && !isAdminMode)
    {
        args.Cancel = true;
        MessageBox.Show("Administrator privileges required.", "Access Denied", 
                        MessageBoxButtons.OK, MessageBoxIcon.Warning);
    }
}
```

### Validating Before Tab Change

**C#:**
```csharp
private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
{
    // Validate data in current tab before allowing switch
    if (HasUnsavedChanges())
    {
        DialogResult result = MessageBox.Show(
            "You have unsaved changes. Continue?",
            "Unsaved Changes",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.No)
        {
            args.Cancel = true;
        }
    }
}

private bool HasUnsavedChanges()
{
    // Your validation logic
    return false;
}
```

## SelectedIndexChanged Event

The `SelectedIndexChanged` event occurs **after** the tab selection has changed. Use this to respond to tab changes.

### Event Signature

**C#:**
```csharp
this.tabbedFormControl.SelectedIndexChanged += TabbedFormControl_SelectedIndexChanged;

private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
{
    // Event logic here
}
```

**VB.NET:**
```vb
AddHandler Me.tabbedFormControl.SelectedIndexChanged, AddressOf TabbedFormControl_SelectedIndexChanged

Private Sub TabbedFormControl_SelectedIndexChanged(ByVal sender As Object, ByVal e As EventArgs)
    ' Event logic here
End Sub
```

### Responding to Tab Changes

**C#:**
```csharp
private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
{
    // Log the selection
    var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
    foreach (var tab in tabs)
    {
        if (this.tabbedFormControl.SelectedTab == tab)
        {
            Console.WriteLine("Selected Tab: " + tab.Text);
        }
    }
}
```

**VB.NET:**
```vb
Private Sub TabbedFormControl_SelectedIndexChanged(ByVal sender As Object, ByVal e As EventArgs)
    ' Log the selection
    Dim tabs = tabbedFormControl.Tabs.OfType(Of TabPageAdv)()
    For Each tab In tabs
        If Me.tabbedFormControl.SelectedTab Is tab Then
            Console.WriteLine("Selected Tab:" & tab.Text)
        End If
    Next tab
End Sub
```

### Updating UI Based on Selection

**C#:**
```csharp
private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
{
    // Update status bar
    int selectedIndex = this.tabbedFormControl.SelectedIndex;
    int totalTabs = this.tabbedFormControl.Tabs.Count;
    string selectedTabText = this.tabbedFormControl.SelectedTab?.Text;
    
    statusLabel.Text = $"Tab {selectedIndex + 1} of {totalTabs}: {selectedTabText}";
    
    // Load tab-specific data
    LoadDataForTab(selectedIndex);
}

private void LoadDataForTab(int tabIndex)
{
    // Your data loading logic
}
```

### Tracking Tab Visit History

**C#:**
```csharp
private List<string> tabHistory = new List<string>();

private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
{
    string currentTab = this.tabbedFormControl.SelectedTab?.Text;
    if (!string.IsNullOrEmpty(currentTab))
    {
        tabHistory.Add($"{DateTime.Now}: {currentTab}");
        Console.WriteLine($"Tab history: {tabHistory.Count} entries");
    }
}
```

## Complete Selection Management Example

Here's a comprehensive example combining both events:

**C#:**
```csharp
using System;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        InitializeTabbedForm();
        AttachSelectionEvents();
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        // Add sample tabs
        string[] tabNames = { "Home", "Settings", "Admin", "Reports" };
        foreach (string name in tabNames)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = name;
            tabbedFormControl.Tabs.Add(tab);
        }
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
    
    private void AttachSelectionEvents()
    {
        tabbedFormControl.SelectedIndexChanging += TabbedFormControl_SelectedIndexChanging;
        tabbedFormControl.SelectedIndexChanged += TabbedFormControl_SelectedIndexChanged;
    }
    
    private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
    {
        // Get target tab
        TabPageAdv targetTab = tabbedFormControl.Tabs[args.NewValue] as TabPageAdv;
        
        // Restrict admin tab access
        if (targetTab?.Text == "Admin")
        {
            DialogResult result = MessageBox.Show(
                "Administrator access required. Continue?",
                "Confirm Access",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Question);
            
            if (result == DialogResult.No)
            {
                args.Cancel = true;
            }
        }
        
        // Log the attempted change
        TabPageAdv currentTab = tabbedFormControl.Tabs[args.OldValue] as TabPageAdv;
        Console.WriteLine($"Changing from '{currentTab?.Text}' to '{targetTab?.Text}'");
    }
    
    private void TabbedFormControl_SelectedIndexChanged(object sender, EventArgs e)
    {
        // Update title bar
        string selectedTab = tabbedFormControl.SelectedTab?.Text;
        this.Text = $"Tabbed Form Demo - {selectedTab}";
        
        // Log successful change
        Console.WriteLine($"Now viewing: {selectedTab}");
    }
}
```

## Common Patterns

### Pattern 1: Cycle Through Tabs

**C#:**
```csharp
private void NextTab()
{
    int nextIndex = (tabbedFormControl.SelectedIndex + 1) % tabbedFormControl.Tabs.Count;
    tabbedFormControl.SelectedIndex = nextIndex;
}

private void PreviousTab()
{
    int prevIndex = tabbedFormControl.SelectedIndex - 1;
    if (prevIndex < 0)
        prevIndex = tabbedFormControl.Tabs.Count - 1;
    tabbedFormControl.SelectedIndex = prevIndex;
}
```

### Pattern 2: Select Tab by Name

**C#:**
```csharp
private bool SelectTabByName(string tabName)
{
    var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
    var targetTab = tabs.FirstOrDefault(t => t.Text == tabName);
    
    if (targetTab != null)
    {
        tabbedFormControl.SelectedTab = targetTab;
        return true;
    }
    return false;
}

// Usage
SelectTabByName("Settings");
```

### Pattern 3: Lock Specific Tabs

**C#:**
```csharp
private HashSet<int> lockedTabs = new HashSet<int> { 2, 3 };

private void TabbedFormControl_SelectedIndexChanging(object sender, SelectedIndexChangingEventArgs args)
{
    if (lockedTabs.Contains(args.NewValue))
    {
        args.Cancel = true;
        MessageBox.Show("This tab is locked.");
    }
}
```

## Tips and Best Practices

1. **Use SelectedIndexChanging for Validation**: Always use the `SelectedIndexChanging` event when you need to validate or prevent tab changes.

2. **Prefer SelectedTab Over SelectedIndex**: When working with tab references, using `SelectedTab` is more readable and maintainable.

3. **Handle Null Cases**: Always check for null when accessing `SelectedTab` properties.

4. **Avoid Recursive Changes**: Be careful not to change the selected tab within the selection event handlers, as this can cause infinite loops.

5. **Defer Heavy Operations**: If loading data on tab change, consider using async methods to prevent UI freezing.

## Troubleshooting

### Issue: Event Not Firing

**Solution**: Ensure the event is attached after creating the `TabbedFormControl` but before any tab selection occurs.

### Issue: Cancel Not Working

**Solution**: Make sure you're using `SelectedIndexChanging` (not `SelectedIndexChanged`) to cancel the selection. The `Changed` event occurs after the change completes.

### Issue: Null Reference Exception

**Solution**: Always check if `SelectedTab` is null before accessing its properties:
```csharp
string text = tabbedFormControl.SelectedTab?.Text ?? "No tab selected";
```
