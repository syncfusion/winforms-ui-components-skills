# Customization

This guide covers appearance and behavior customization options for the Tabbed Form control.

## Overview

The Tabbed Form provides various customization options to control the appearance and behavior of tabs, including title bar integration, tab styling, and visual customization.

## Title Bar Integration

### ExtendTabsToTitleBar Property

The `ExtendTabsToTitleBar` property controls whether tabs are integrated into the window's title bar or displayed below it.

**Default Value:** `true` (tabs appear in title bar)

#### Display Tabs in Title Bar (Default)

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Tabs will extend into title bar (default behavior)
    this.ExtendTabsToTitleBar = true;
    
    // Setup tabbed control
    InitializeTabbedForm();
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Tabs will extend into title bar (default behavior)
    Me.ExtendTabsToTitleBar = True
    
    ' Setup tabbed control
    InitializeTabbedForm()
End Sub
```

**Visual Result:** Tabs appear integrated with the window title bar, creating a modern, browser-like appearance.

#### Display Tabs Below Title Bar

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Display tabs below title bar
    this.ExtendTabsToTitleBar = false;
    
    // Setup tabbed control
    InitializeTabbedForm();
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Display tabs below title bar
    Me.ExtendTabsToTitleBar = False
    
    ' Setup tabbed control
    InitializeTabbedForm()
End Sub
```

**Visual Result:** Tabs appear as a separate row below the standard Windows title bar.

### When to Use Each Option

**Use ExtendTabsToTitleBar = true when:**
- Creating modern, browser-style applications
- Maximizing vertical screen space
- Building applications similar to Visual Studio, Chrome, or Edge

**Use ExtendTabsToTitleBar = false when:**
- Maintaining traditional Windows application appearance
- When title bar text needs to be clearly visible

## Tab Appearance

### Tab Text Customization

**C#:**
```csharp
// Set tab text
tabPageAdv1.Text = "My Document";

// Change tab text dynamically
private void UpdateTabText(TabPageAdv tab, string newText)
{
    tab.Text = newText;
}

// Example: Add modified indicator
if (documentHasUnsavedChanges)
{
    currentTab.Text = currentTab.Text + " *";
}
```

### Tab ToolTips

**C#:**
```csharp
TabPageAdv tab = new TabPageAdv();
tab.Text = "Document1";
tab.ToolTipText = "C:\\Documents\\Document1.txt";
tabbedFormControl.Tabs.Add(tab);
```

## TabbedFormControl Properties

### Close Button Configuration

**C#:**
```csharp
// Show close button on tabs
tabbedFormControl.ShowTabCloseButton = true;

// Hide close button
tabbedFormControl.ShowTabCloseButton = false;
```

## Complete Customization Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : SfTabbedForm
{
    private SfTabbedFormControl tabbedFormControl;
    
    public Form1()
    {
        InitializeComponent();
        InitializeCustomizedTabbedForm();
    }
    
    private void InitializeCustomizedTabbedForm()
    {
        // Create tabbed form control
        tabbedFormControl = new SfTabbedFormControl();
        
        // Configure title bar integration
        this.ExtendTabsToTitleBar = true;
        
        // Configure tab appearance
        tabbedFormControl.ShowTabCloseButton = true;
        
        // Enable features
        tabbedFormControl.AllowDraggingTabs = true;
        tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown | 
                                             TabPrimitiveMode.FirstTab | 
                                             TabPrimitiveMode.LastTab;
        
        // Add styled tabs
        for (int i = 1; i <= 8; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Document {i}";
            tab.ToolTipText = $"Path: C:\\Documents\\Document{i}.txt";
            
            // Add content to tab
            Label label = new Label();
            label.Text = $"Content for Document {i}";
            label.AutoSize = true;
            label.Location = new Point(20, 20);
            label.Font = new Font("Segoe UI", 11);
            tab.Controls.Add(label);
            
            tabbedFormControl.Tabs.Add(tab);
        }
        
        // Setup context menu
        SetupContextMenu();
        
        // Add to form
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
        
        // Configure form
        this.Text = "Customized Tabbed Form";
        this.Size = new Size(1024, 768);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void SetupContextMenu()
    {
        ContextMenuStrip contextMenu = new ContextMenuStrip();
        contextMenu.Items.Add("Close", null, OnCloseTab);
        contextMenu.Items.Add("Close Others", null, OnCloseOthers);
        tabbedFormControl.TabContextMenu = contextMenu;
    }
    
    private TabPageAdv clickedTab;
    
    private void OnCloseTab(object sender, EventArgs e)
    {
        if (clickedTab != null && tabbedFormControl.Tabs.Count > 1)
        {
            clickedTab.Close();
        }
    }
    
    private void OnCloseOthers(object sender, EventArgs e)
    {
        // Implementation
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Partial Public Class Form1
    Inherits SfTabbedForm
    Private tabbedFormControl As SfTabbedFormControl
    
    Public Sub New()
        InitializeComponent()
        InitializeCustomizedTabbedForm()
    End Sub
    
    Private Sub InitializeCustomizedTabbedForm()
        ' Create tabbed form control
        tabbedFormControl = New SfTabbedFormControl()
        
        ' Configure title bar integration
        Me.ExtendTabsToTitleBar = True
        
        ' Configure tab appearance
        tabbedFormControl.ShowTabCloseButton = True
        
        ' Enable features
        tabbedFormControl.AllowDraggingTabs = True
        tabbedFormControl.TabPrimitiveMode = TabPrimitiveMode.DropDown Or 
                                             TabPrimitiveMode.FirstTab Or 
                                             TabPrimitiveMode.LastTab
        
        ' Add styled tabs
        For i As Integer = 1 To 8
            Dim tab As New TabPageAdv()
            tab.Text = $"Document {i}"
            tab.ToolTipText = $"Path: C:\Documents\Document{i}.txt"
            
            ' Add content to tab
            Dim label As New Label()
            label.Text = $"Content for Document {i}"
            label.AutoSize = True
            label.Location = New Point(20, 20)
            label.Font = New Font("Segoe UI", 11)
            tab.Controls.Add(label)
            
            tabbedFormControl.Tabs.Add(tab)
        Next i
        
        ' Setup context menu
        SetupContextMenu()
        
        ' Add to form
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
        
        ' Configure form
        Me.Text = "Customized Tabbed Form"
        Me.Size = New Size(1024, 768)
        Me.StartPosition = FormStartPosition.CenterScreen
    End Sub
    
    Private Sub SetupContextMenu()
        Dim contextMenu As New ContextMenuStrip()
        contextMenu.Items.Add("Close", Nothing, AddressOf OnCloseTab)
        contextMenu.Items.Add("Close Others", Nothing, AddressOf OnCloseOthers)
        tabbedFormControl.TabContextMenu = contextMenu
    End Sub
    
    Private clickedTab As TabPageAdv
    
    Private Sub OnCloseTab(ByVal sender As Object, ByVal e As EventArgs)
        If clickedTab IsNot Nothing AndAlso tabbedFormControl.Tabs.Count > 1 Then
            clickedTab.Close()
        End If
    End Sub
    
    Private Sub OnCloseOthers(ByVal sender As Object, ByVal e As EventArgs)
        ' Implementation
    End Sub
End Class
```

## Common Customization Patterns

### Pattern 1: Browser-Style Appearance

**C#:**
```csharp
private void ConfigureBrowserStyle()
{
    this.ExtendTabsToTitleBar = true;
    tabbedFormControl.ShowTabCloseButton = true;
    tabbedFormControl.AllowDraggingTabs = true;
    
    // Add context menu
    SetupBrowserContextMenu();
}
```

### Pattern 2: Traditional MDI Style

**C#:**
```csharp
private void ConfigureMDIStyle()
{
    this.ExtendTabsToTitleBar = false;
    tabbedFormControl.ShowTabCloseButton = true;
}
```

### Pattern 3: Fixed Tabs (Non-Closable)

**C#:**
```csharp
private void ConfigureFixedTabs()
{
    tabbedFormControl.ShowTabCloseButton = false;
    tabbedFormControl.AllowDraggingTabs = false;
    this.ExtendTabsToTitleBar = false;
}
```

### Pattern 4: Dynamic Tab Styling

**C#:**
```csharp
private void StyleTabByState(TabPageAdv tab, bool isModified)
{
    if (isModified)
    {
        tab.Text = tab.Text.TrimEnd('*') + " *";
    }
    else
    {
        tab.Text = tab.Text.TrimEnd(' ', '*');
    }
}
```

## Best Practices

1. **Choose Title Bar Mode Early**: Decide on `ExtendTabsToTitleBar` setting during initial design, as it significantly affects the UI.

2. **Enable Features Together**: Drag-and-drop and context menus work well together for complete tab management.

3. **Tooltips for Long Names**: Add tooltips showing full file paths or descriptions for tabs with truncated text.

## Troubleshooting

### Issue: Tabs Not Showing in Title Bar

**Solution**: Ensure `ExtendTabsToTitleBar = true` is set on the **form** (not the control) before adding the TabbedFormControl.

### Issue: Close Button Not Visible

**Solution**: Set `tabbedFormControl.ShowTabCloseButton = true`.

## Additional Properties

### Form-Level Properties

```csharp
// Control border style
this.FormBorderStyle = FormBorderStyle.Sizable;

// Window state
this.WindowState = FormWindowState.Maximized;

// Icon
this.Icon = new Icon("app.ico");
```

## Summary

The Tabbed Form provides extensive customization options:
- **Title bar integration** for modern or traditional appearance
- **Tab styling** with tooltips, and text customization
- **Behavior control** for close buttons, dragging, and navigation
- **Flexible layout** options for various use cases

Choose customization options based on your application requirements and user expectations.
