# Context Menu

## Table of Contents
- [Overview](#overview)
- [Basic Context Menu Setup](#basic-context-menu-setup)
- [ContextMenuOpening Event](#contextmenuopening-event)
- [Browser-Style Context Menu](#browser-style-context-menu)
- [Complete Implementation](#complete-implementation)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Tabbed Form supports customizable context menus that appear when users right-click on tabs. This feature enables browser-style tab management with options like Close, Close All, Close Others, etc.

## Basic Context Menu Setup

Set a custom context menu using the `TabContextMenu` property.

### Creating a Simple Context Menu

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    tabbedFormControl = new SfTabbedFormControl();
    
    // Add tabs
    for (int i = 1; i <= 5; i++)
        tabbedFormControl.Tabs.Add(new TabPageAdv() { Text = "Document" + i });
    
    this.Controls.Add(tabbedFormControl);
    this.TabbedFormControl = tabbedFormControl;
    
    // Setup context menu
    ContextMenuStrip tabContextMenu = new ContextMenuStrip();
    tabContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
    
    tabbedFormControl.TabContextMenu = tabContextMenu;
}

private void OnCloseMenuClicked(object sender, EventArgs e)
{
    // Close logic here
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    tabbedFormControl = New SfTabbedFormControl()
    
    ' Add tabs
    For i As Integer = 1 To 5
        tabbedFormControl.Tabs.Add(New TabPageAdv() With {.Text = "Document" & i})
    Next i
    
    Me.Controls.Add(tabbedFormControl)
    Me.TabbedFormControl = tabbedFormControl
    
    ' Setup context menu
    Dim tabContextMenu As New ContextMenuStrip()
    tabContextMenu.Items.Add("Close", Nothing, AddressOf OnCloseMenuClicked)
    
    tabbedFormControl.TabContextMenu = tabContextMenu
End Sub

Private Sub OnCloseMenuClicked(ByVal sender As Object, ByVal e As EventArgs)
    ' Close logic here
End Sub
```

### Context Menu with Multiple Options

**C#:**
```csharp
private TabPageAdv clickedTab;

public Form1()
{
    InitializeComponent();
    
    tabbedFormControl = new SfTabbedFormControl();
    
    // Add tabs
    for (int i = 1; i <= 15; i++)
        tabbedFormControl.Tabs.Add(new TabPageAdv() { Text = "Document" + i });
    
    this.Controls.Add(tabbedFormControl);
    this.TabbedFormControl = tabbedFormControl;
    
    // Create context menu with multiple items
    ContextMenuStrip tabContextMenu = new ContextMenuStrip();
    tabContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
    tabContextMenu.Items.Add("Close All", null, OnCloseAllMenuClicked);
    tabContextMenu.Items.Add("-"); // Separator
    tabContextMenu.Items.Add("Refresh", null, OnRefreshMenuClicked);
    
    tabbedFormControl.TabContextMenu = tabContextMenu;
}
```

## ContextMenuOpening Event

The `ContextMenuOpening` event fires before the context menu is displayed, allowing you to customize menu items dynamically based on the clicked tab.

### Event Signature

**C#:**
```csharp
tabbedFormControl.ContextMenuOpening += TabbedFormControl_ContextMenuOpening;

private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
{
    // Event logic here
}
```

**VB.NET:**
```vb
AddHandler tabbedFormControl.ContextMenuOpening, AddressOf TabbedFormControl_ContextMenuOpening

Private Sub TabbedFormControl_ContextMenuOpening(ByVal sender As Object, ByVal e As ContextMenuOpeningEventArgs)
    ' Event logic here
End Sub
```

### ContextMenuOpeningEventArgs Properties

- **Tab**: The `TabPageAdv` that was right-clicked
- **ContextMenu**: The `ContextMenuStrip` that will be displayed (can be modified)

### Dynamic Menu Customization

**C#:**
```csharp
private TabPageAdv clickedTab;

private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
{
    // Store the clicked tab for use in menu handlers
    clickedTab = e.Tab;
    
    // Customize menu based on tab properties
    if (e.Tab.Text == "Home")
    {
        // Disable "Close" for Home tab
        e.ContextMenu.Items[0].Enabled = false;
    }
    else
    {
        e.ContextMenu.Items[0].Enabled = true;
    }
}
```

### Conditional Menu Items

**C#:**
```csharp
private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
{
    clickedTab = e.Tab;
    
    var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
    int tabCount = tabs.Count();
    
    // Enable/disable based on context
    bool isOnlyTab = tabCount == 1;
    bool isFirstTab = e.Tab.TabIndex == 0;
    bool isLastTab = e.Tab.TabIndex == tabCount - 1;
    
    // Find menu items (assuming specific order)
    e.ContextMenu.Items[0].Enabled = !isOnlyTab; // Close
    e.ContextMenu.Items[1].Enabled = !isOnlyTab; // Close Others
}
```

## Browser-Style Context Menu

Create a web browser-style context menu with Close, Close All But This, and Close Tabs to Right options.

### Complete Browser-Style Implementation

**C#:**
```csharp
using System;
using System.Collections.ObjectModel;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private TabPageAdv clickedTab;
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        InitializeTabbedForm();
        SetupContextMenu();
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        for (int i = 1; i <= 10; i++)
            tabbedFormControl.Tabs.Add(new TabPageAdv() { Text = "Document" + i });
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
    
    private void SetupContextMenu()
    {
        ContextMenuStrip tabContextMenu = new ContextMenuStrip();
        tabContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
        tabContextMenu.Items.Add("Close all but this", null, OnCloseAllButThisMenuClicked);
        tabContextMenu.Items.Add("Close tabs to the right", null, OnCloseTabsToRightMenuClicked);
        
        tabbedFormControl.TabContextMenu = tabContextMenu;
        tabbedFormControl.ContextMenuOpening += TabbedFormControl_ContextMenuOpening;
    }
    
    private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
    {
        clickedTab = e.Tab;
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
        var tabsExistsInRight = tabs.Any(tab => tab.TabIndex > e.Tab.TabIndex);
        
        if (tabs.Count() == 1)
        {
            // Only one tab - disable all close options
            e.ContextMenu.Items[0].Enabled = false; // Close
            e.ContextMenu.Items[1].Enabled = false; // Close all but this
            e.ContextMenu.Items[2].Enabled = false; // Close tabs to the right
        }
        else if (!tabsExistsInRight)
        {
            // Last tab - disable "close tabs to the right"
            e.ContextMenu.Items[0].Enabled = true;
            e.ContextMenu.Items[1].Enabled = true;
            e.ContextMenu.Items[2].Enabled = false;
        }
        else
        {
            // Enable all options
            e.ContextMenu.Items[0].Enabled = true;
            e.ContextMenu.Items[1].Enabled = true;
            e.ContextMenu.Items[2].Enabled = true;
        }
    }
    
    private void OnCloseMenuClicked(object sender, EventArgs e)
    {
        if (clickedTab != null)
        {
            if (this.TabbedFormControl.Tabs.OfType<TabPageAdv>().Count() == 1)
            {
                // Last tab - close the form
                this.Close();
            }
            else
            {
                clickedTab.Close();
            }
        }
    }
    
    private void OnCloseAllButThisMenuClicked(object sender, EventArgs e)
    {
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
        var removedTabs = new ObservableCollection<TabPageAdv>();
        
        foreach (var tab in tabs)
        {
            if (clickedTab != null && tab != clickedTab)
                removedTabs.Add(tab);
        }
        
        foreach (var tab in removedTabs)
        {
            tab.Close();
        }
    }
    
    private void OnCloseTabsToRightMenuClicked(object sender, EventArgs e)
    {
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
        var removedTabs = new ObservableCollection<TabPageAdv>();
        
        foreach (var tab in tabs)
        {
            if (clickedTab != null && tab.TabIndex > clickedTab.TabIndex)
                removedTabs.Add(tab);
        }
        
        foreach (var tab in removedTabs)
        {
            tab.Close();
        }
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Collections.ObjectModel
Imports System.Linq
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Partial Public Class Form1
    Inherits SfTabbedForm
    Private clickedTab As TabPageAdv
    Private tabbedFormControl As SfTabbedFormControl
    
    Public Sub New()
        InitializeComponent()
        InitializeTabbedForm()
        SetupContextMenu()
    End Sub
    
    Private Sub InitializeTabbedForm()
        tabbedFormControl = New SfTabbedFormControl()
        
        For i As Integer = 1 To 10
            tabbedFormControl.Tabs.Add(New TabPageAdv() With {.Text = "Document" & i})
        Next i
        
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
    
    Private Sub SetupContextMenu()
        Dim tabContextMenu As New ContextMenuStrip()
        tabContextMenu.Items.Add("Close", Nothing, AddressOf OnCloseMenuClicked)
        tabContextMenu.Items.Add("Close all but this", Nothing, AddressOf OnCloseAllButThisMenuClicked)
        tabContextMenu.Items.Add("Close tabs to the right", Nothing, AddressOf OnCloseTabsToRightMenuClicked)
        
        tabbedFormControl.TabContextMenu = tabContextMenu
        AddHandler tabbedFormControl.ContextMenuOpening, AddressOf TabbedFormControl_ContextMenuOpening
    End Sub
    
    Private Sub TabbedFormControl_ContextMenuOpening(ByVal sender As Object, ByVal e As ContextMenuOpeningEventArgs)
        clickedTab = e.Tab
        Dim tabs = tabbedFormControl.Tabs.OfType(Of TabPageAdv)()
        Dim tabsExistsInRight = tabs.Any(Function(tab) tab.TabIndex > e.Tab.TabIndex)
        
        If tabs.Count() = 1 Then
            ' Only one tab - disable all close options
            e.ContextMenu.Items(0).Enabled = False
            e.ContextMenu.Items(1).Enabled = False
            e.ContextMenu.Items(2).Enabled = False
        ElseIf Not tabsExistsInRight Then
            ' Last tab - disable "close tabs to the right"
            e.ContextMenu.Items(0).Enabled = True
            e.ContextMenu.Items(1).Enabled = True
            e.ContextMenu.Items(2).Enabled = False
        Else
            ' Enable all options
            e.ContextMenu.Items(0).Enabled = True
            e.ContextMenu.Items(1).Enabled = True
            e.ContextMenu.Items(2).Enabled = True
        End If
    End Sub
    
    Private Sub OnCloseMenuClicked(ByVal sender As Object, ByVal e As EventArgs)
        If clickedTab IsNot Nothing Then
            If Me.TabbedFormControl.Tabs.OfType(Of TabPageAdv)().Count() = 1 Then
                ' Last tab - close the form
                Me.Close()
            Else
                clickedTab.Close()
            End If
        End If
    End Sub
    
    Private Sub OnCloseAllButThisMenuClicked(ByVal sender As Object, ByVal e As EventArgs)
        Dim tabs = tabbedFormControl.Tabs.OfType(Of TabPageAdv)()
        Dim removedTabs = New ObservableCollection(Of TabPageAdv)()
        
        For Each tab In tabs
            If clickedTab IsNot Nothing AndAlso tab IsNot clickedTab Then
                removedTabs.Add(tab)
            End If
        Next tab
        
        For Each tab In removedTabs
            tab.Close()
        Next tab
    End Sub
    
    Private Sub OnCloseTabsToRightMenuClicked(ByVal sender As Object, ByVal e As EventArgs)
        Dim tabs = tabbedFormControl.Tabs.OfType(Of TabPageAdv)()
        Dim removedTabs = New ObservableCollection(Of TabPageAdv)()
        
        For Each tab In tabs
            If clickedTab IsNot Nothing AndAlso tab.TabIndex > clickedTab.TabIndex Then
                removedTabs.Add(tab)
            End If
        Next tab
        
        For Each tab In removedTabs
            tab.Close()
        Next tab
    End Sub
End Class
```

## Complete Implementation

### Full-Featured Context Menu Example

**C#:**
```csharp
using System;
using System.Collections.ObjectModel;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private TabPageAdv clickedTab;
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        InitializeTabbedForm();
        SetupContextMenu();
    }
    
    private void InitializeTabbedForm()
    {
        tabbedFormControl = new SfTabbedFormControl();
        
        for (int i = 1; i <= 10; i++)
            tabbedFormControl.Tabs.Add(new TabPageAdv() { Text = $"Document {i}" });
        
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
    
    private void SetupContextMenu()
    {
        ContextMenuStrip tabContextMenu = new ContextMenuStrip();
        
        // Add menu items
        tabContextMenu.Items.Add("New Tab", null, OnNewTabClicked);
        tabContextMenu.Items.Add("-"); // Separator
        tabContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
        tabContextMenu.Items.Add("Close all but this", null, OnCloseAllButThisMenuClicked);
        tabContextMenu.Items.Add("Close tabs to the right", null, OnCloseTabsToRightMenuClicked);
        tabContextMenu.Items.Add("Close all tabs", null, OnCloseAllTabsClicked);
        tabContextMenu.Items.Add("-"); // Separator
        tabContextMenu.Items.Add("Refresh", null, OnRefreshClicked);
        tabContextMenu.Items.Add("Duplicate", null, OnDuplicateClicked);
        
        tabbedFormControl.TabContextMenu = tabContextMenu;
        tabbedFormControl.ContextMenuOpening += TabbedFormControl_ContextMenuOpening;
    }
    
    private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
    {
        clickedTab = e.Tab;
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
        int tabCount = tabs.Count();
        bool hasTabsToRight = tabs.Any(tab => tab.TabIndex > e.Tab.TabIndex);
        
        // Menu item indices
        const int NEW_TAB = 0;
        const int CLOSE = 2;
        const int CLOSE_ALL_BUT_THIS = 3;
        const int CLOSE_TABS_TO_RIGHT = 4;
        const int CLOSE_ALL = 5;
        const int REFRESH = 7;
        const int DUPLICATE = 8;
        
        // Update menu item states
        e.ContextMenu.Items[CLOSE].Enabled = tabCount > 1;
        e.ContextMenu.Items[CLOSE_ALL_BUT_THIS].Enabled = tabCount > 1;
        e.ContextMenu.Items[CLOSE_TABS_TO_RIGHT].Enabled = hasTabsToRight;
        e.ContextMenu.Items[CLOSE_ALL].Enabled = tabCount > 0;
    }
    
    private void OnNewTabClicked(object sender, EventArgs e)
    {
        int nextNum = tabbedFormControl.Tabs.Count + 1;
        TabPageAdv newTab = new TabPageAdv() { Text = $"Document {nextNum}" };
        tabbedFormControl.Tabs.Add(newTab);
        tabbedFormControl.SelectedTab = newTab;
    }
    
    private void OnCloseMenuClicked(object sender, EventArgs e)
    {
        if (clickedTab != null)
        {
            if (tabbedFormControl.Tabs.Count == 1)
                this.Close();
            else
                clickedTab.Close();
        }
    }
    
    private void OnCloseAllButThisMenuClicked(object sender, EventArgs e)
    {
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>().ToList();
        foreach (var tab in tabs)
        {
            if (tab != clickedTab)
                tab.Close();
        }
    }
    
    private void OnCloseTabsToRightMenuClicked(object sender, EventArgs e)
    {
        if (clickedTab == null) return;
        
        var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>()
            .Where(tab => tab.TabIndex > clickedTab.TabIndex)
            .ToList();
        
        foreach (var tab in tabs)
        {
            tab.Close();
        }
    }
    
    private void OnCloseAllTabsClicked(object sender, EventArgs e)
    {
        DialogResult result = MessageBox.Show(
            "Close all tabs?",
            "Confirm",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.Yes)
        {
            this.Close();
        }
    }
    
    private void OnRefreshClicked(object sender, EventArgs e)
    {
        if (clickedTab != null)
        {
            MessageBox.Show($"Refreshing: {clickedTab.Text}");
            // Add your refresh logic here
        }
    }
    
    private void OnDuplicateClicked(object sender, EventArgs e)
    {
        if (clickedTab != null)
        {
            TabPageAdv duplicateTab = new TabPageAdv();
            duplicateTab.Text = clickedTab.Text + " (Copy)";
            
            int insertIndex = clickedTab.TabIndex + 1;
            tabbedFormControl.Tabs.Insert(insertIndex, duplicateTab);
            tabbedFormControl.SelectedTab = duplicateTab;
        }
    }
}
```

## Common Patterns

### Pattern 1: Tab-Specific Menu Items

**C#:**
```csharp
private void TabbedFormControl_ContextMenuOpening(object sender, ContextMenuOpeningEventArgs e)
{
    clickedTab = e.Tab;
    
    // Add tab-specific items dynamically
    e.ContextMenu.Items.Clear();
    
    if (e.Tab.Text.StartsWith("Home"))
    {
        e.ContextMenu.Items.Add("Pin Home", null, OnPinClicked);
    }
    else
    {
        e.ContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
        e.ContextMenu.Items.Add("Rename", null, OnRenameClicked);
    }
}
```

### Pattern 2: Icon Support

**C#:**
```csharp
private void SetupContextMenu()
{
    ContextMenuStrip tabContextMenu = new ContextMenuStrip();
    
    ToolStripMenuItem closeItem = new ToolStripMenuItem("Close");
    closeItem.Click += OnCloseMenuClicked;
    closeItem.ShortcutKeys = Keys.Control | Keys.W;
    closeItem.ShowShortcutKeys = true;
    
    tabContextMenu.Items.Add(closeItem);
    tabbedFormControl.TabContextMenu = tabContextMenu;
}
```

### Pattern 3: Confirmation Dialogs

**C#:**
```csharp
private void OnCloseAllButThisMenuClicked(object sender, EventArgs e)
{
    var tabs = tabbedFormControl.Tabs.OfType<TabPageAdv>();
    int closeCount = tabs.Count() - 1;
    
    DialogResult result = MessageBox.Show(
        $"Close {closeCount} tabs?",
        "Confirm",
        MessageBoxButtons.YesNo,
        MessageBoxIcon.Question);
    
    if (result == DialogResult.Yes)
    {
        // Close tabs logic
    }
}
```

## Best Practices

1. **Store Clicked Tab**: Always store the clicked tab reference in `ContextMenuOpening` for use in menu handlers.

2. **Dynamic Menu States**: Enable/disable menu items based on context (e.g., disable "Close Others" if only one tab).

3. **Handle Last Tab**: When closing the last tab, consider closing the entire form instead.

4. **Use ObservableCollection**: When removing multiple tabs, collect them first to avoid collection modification errors.

5. **Provide Feedback**: Show confirmation dialogs for destructive operations like "Close All".

6. **Keyboard Shortcuts**: Consider adding keyboard shortcuts to menu items for power users.

## Troubleshooting

### Issue: clickedTab is Null in Menu Handlers

**Solution**: Ensure you're setting `clickedTab` in the `ContextMenuOpening` event before the menu is displayed.

### Issue: Collection Modified Exception

**Solution**: Create a list of tabs to remove first, then iterate that list:
```csharp
var tabsToRemove = tabs.Where(/* condition */).ToList();
foreach (var tab in tabsToRemove) tab.Close();
```

### Issue: Menu Items Not Updating

**Solution**: Use the `ContextMenuOpening` event to update menu item states dynamically each time the menu opens.

### Issue: Close Not Working on Last Tab

**Solution**: Check if it's the last tab and close the form instead:
```csharp
if (tabbedFormControl.Tabs.Count == 1)
    this.Close();
else
    clickedTab.Close();
```
