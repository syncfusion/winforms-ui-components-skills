---
name: syncfusion-winforms-tabbed-form
description: Guide for implementing Syncfusion Windows Forms Tabbed Form (SfTabbedForm) with tabbed user interface and title bar integration. Use this skill when implementing tabbed forms, multi-document interfaces, or tab navigation in Windows Forms. Covers SfTabbedForm setup, TabPageAdv configuration, drag-and-drop functionality, tab selection, and context menus.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Tabbed Form

The Syncfusion Tabbed Form (SfTabbedForm) provides a tabbed user interface for Windows Forms applications. Users can add any number of tabs, with each tab containing any number of controls. Tabs can be loaded into the form's title bar or displayed below the title bar.

## When to Use This Skill

Use this skill when you need to:
- Create tabbed user interfaces in Windows Forms applications
- Convert standard forms to tabbed forms with title bar integration
- Implement multi-document interfaces (MDI) alternatives
- Add tab management features (add, close, reorder tabs)
- Handle tab selection and navigation programmatically
- Enable drag-and-drop tab reordering
- Customize context menus for tabs (like web browsers)

## Key Features

- **Easy Navigation**: Built-in navigation buttons (first, last, next, previous, dropdown)
- **Tab Management**: Close tabs using close button, programmatic tab control
- **Drag and Drop**: Reorder tabs by dragging
- **Title Bar Integration**: Display tabs in title bar or below it
- **Context Menus**: Customizable right-click menus on tabs

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and dependencies
- Converting standard form to SfTabbedForm
- Loading TabbedFormControl to the form
- Adding tabs (TabPageAdv) to the control
- Configuring title bar integration (ExtendTabsToTitleBar)
- Basic tab setup and initialization

### Tab Selection
📄 **Read:** [references/tab-selection.md](references/tab-selection.md)
- Programmatic selection (SelectedIndex, SelectedTab properties)
- SelectedIndexChanging event (with cancellation support)
- SelectedIndexChanged event
- Event handling patterns
- Restricting tab selection

### Tab Navigation
📄 **Read:** [references/tab-navigation.md](references/tab-navigation.md)
- TabPrimitiveMode property configuration
- Built-in navigation buttons (FirstTab, LastTab, NextTab, PreviousTab, DropDown)
- TabPrimitiveClick event handling
- Navigation control customization
- Handling tab overflow scenarios

### Drag and Drop
📄 **Read:** [references/drag-and-drop.md](references/drag-and-drop.md)
- Enabling drag and drop (AllowDraggingTabs property)
- TabDragging event and TabDraggingEventArgs
- TabDraggingAction states (DragStarting, DragDropping)
- Canceling tab dragging for specific tabs
- Preventing tab reordering to specific positions

### Context Menu
📄 **Read:** [references/context-menu.md](references/context-menu.md)
- TabContextMenu property
- Creating custom context menus with ContextMenuStrip
- ContextMenuOpening event for dynamic menu customization
- Browser-style context menus (Close, Close All But This, Close Tabs to Right)
- Conditional menu item enabling/disabling

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- ExtendTabsToTitleBar property for title bar integration
- Tab appearance and styling options
- TabbedFormControl properties and configuration
- Customizing tab headers

## Quick Start Example

### Basic Tabbed Form Setup

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create and configure the tabbed form control
        tabbedFormControl = new SfTabbedFormControl();
        
        // Add tabs
        TabPageAdv tabPageAdv1 = new TabPageAdv();
        TabPageAdv tabPageAdv2 = new TabPageAdv();
        tabPageAdv1.Text = "Document1";
        tabPageAdv2.Text = "Document2";
        
        tabbedFormControl.Tabs.Add(tabPageAdv1);
        tabbedFormControl.Tabs.Add(tabPageAdv2);
        
        // Add control to form
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
}
```

### VB.NET Example

```vb
Imports Syncfusion.Windows.Forms.Tools

Partial Public Class Form1
    Inherits SfTabbedForm
    Private tabbedFormControl As SfTabbedFormControl
    
    Public Sub New()
        InitializeComponent()
        
        ' Create and configure the tabbed form control
        tabbedFormControl = New SfTabbedFormControl()
        
        ' Add tabs
        Dim tabPageAdv1 As New TabPageAdv()
        Dim tabPageAdv2 As New TabPageAdv()
        tabPageAdv1.Text = "Document1"
        tabPageAdv2.Text = "Document2"
        
        tabbedFormControl.Tabs.Add(tabPageAdv1)
        tabbedFormControl.Tabs.Add(tabPageAdv2)
        
        ' Add control to form
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Tabs Below Title Bar

```csharp
public Form1()
{
    InitializeComponent();
    
    // Disable extending tabs to title bar
    this.ExtendTabsToTitleBar = false;
    
    // Setup tabbed control as usual
    tabbedFormControl = new SfTabbedFormControl();
    // ... add tabs
}
```

### Pattern 2: Enable Drag and Drop with Navigation

```csharp
public Form1()
{
    InitializeComponent();
    
    tabbedFormControl = new SfTabbedFormControl();
    
    // Enable drag and drop
    tabbedFormControl.AllowDraggingTabs = true;
    
    // Add navigation buttons
    tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown | 
                                         TabPrimitiveMode.FirstTab | 
                                         TabPrimitiveMode.LastTab;
    
    // ... add tabs
}
```

### Pattern 3: Programmatic Tab Selection

```csharp
// Select by index
this.tabbedFormControl.SelectedIndex = 1;

// Or select by tab reference
this.tabbedFormControl.SelectedTab = tabPageAdv2;
```

### Pattern 4: Context Menu with Close Options

```csharp
public Form1()
{
    InitializeComponent();
    
    tabbedFormControl = new SfTabbedFormControl();
    
    // Setup context menu
    ContextMenuStrip tabContextMenu = new ContextMenuStrip();
    tabContextMenu.Items.Add("Close", null, OnCloseMenuClicked);
    tabContextMenu.Items.Add("Close all but this", null, OnCloseAllMenuClicked);
    
    tabbedFormControl.TabContextMenu = tabContextMenu;
    tabbedFormControl.ContextMenuOpening += TabbedFormControl_ContextMenuOpening;
    
    // ... add tabs
}
```

## Key Properties and Events

### Core Properties
- **TabbedFormControl**: Main control containing tabs
- **ExtendTabsToTitleBar**: Boolean to show tabs in title bar (default: true)
- **SelectedIndex**: Zero-based index of selected tab
- **SelectedTab**: Reference to selected TabPageAdv
- **AllowDraggingTabs**: Enable/disable tab drag and drop
- **TabPrimitiveMode**: Configure navigation buttons
- **TabContextMenu**: Custom context menu for tabs

### Key Events
- **SelectedIndexChanging**: Before tab selection changes (cancellable)
- **SelectedIndexChanged**: After tab selection changes
- **TabPrimitiveClick**: When navigation button is clicked
- **TabDragging**: During tab drag operations
- **ContextMenuOpening**: Before context menu displays

## Common Use Cases

1. **Multi-Document Interface**: Create applications with multiple documents in tabs
2. **Settings/Options Dialogs**: Organize settings across multiple tabbed pages
3. **Data Entry Forms**: Split complex forms into logical tabbed sections
4. **Report Viewers**: Display multiple reports in separate tabs
5. **Code Editors**: Implement IDE-style tabbed file editing
6. **Browser-Style Applications**: Create web browser-like tab management

## Assembly Reference

Add reference to `Syncfusion.Tools.Windows` and `Syncfusion.Shared.Base` assembly or install via NuGet:

```powershell
Install-Package Syncfusion.Tools.Windows
```