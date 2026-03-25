# Auto-Scroll Configuration

This guide explains how to enable and configure automatic scrolling in PopupControlContainer when the content exceeds the available display area.

## Overview

The AutoScroll feature automatically displays scrollbars when child controls are positioned or sized beyond the popup's visible area. This improves the user experience by ensuring all content remains accessible without manually resizing the popup.

## When to Use AutoScroll

Enable AutoScroll when:
- The popup contains many child controls that might exceed the visible area
- Content size varies dynamically based on user input or data
- You want to maintain a fixed popup size while accommodating varying content
- Child controls might be positioned outside the initial popup bounds
- Building forms, lists, or grids within the popup

## AutoScroll Property

### Enabling AutoScroll

**Property:**
```csharp
public bool AutoScroll { get; set; }
```

**Default Value:** `false`

**Enable scrolling:**
```csharp
this.popupControlContainer1.AutoScroll = true;
```

**VB.NET:**
```vb
Me.popupControlContainer1.AutoScroll = True
```

When enabled, scrollbars appear automatically when child controls extend beyond the popup's visible region.

## AutoScrollMargin Property

The `AutoScrollMargin` property sets the margin space around the scrollable content. This creates padding between the scrollable region and the edges of the popup.

**Property:**
```csharp
public Size AutoScrollMargin { get; set; }
```

**Default Value:** `Size(0, 0)`

**Set margin:**
```csharp
// Set 2-pixel margin on all sides
this.popupControlContainer1.AutoScrollMargin = new System.Drawing.Size(2, 2);
```

**VB.NET:**
```vb
' Set 2-pixel margin on all sides
Me.popupControlContainer1.AutoScrollMargin = New System.Drawing.Size(2, 2)
```

**Different horizontal and vertical margins:**
```csharp
// 5 pixels horizontal, 10 pixels vertical
this.popupControlContainer1.AutoScrollMargin = new Size(5, 10);
```

## AutoScrollMinSize Property

The `AutoScrollMinSize` property defines the minimum size of the scrollable region. Scrollbars appear when content would require more space than this minimum size allows.

**Property:**
```csharp
public Size AutoScrollMinSize { get; set; }
```

**Default Value:** `Size(0, 0)`

**Set minimum scroll region:**
```csharp
// Minimum 300x200 scrollable area
this.popupControlContainer1.AutoScrollMinSize = new System.Drawing.Size(300, 200);
```

**VB.NET:**
```vb
' Minimum 300x200 scrollable area
Me.popupControlContainer1.AutoScrollMinSize = New System.Drawing.Size(300, 200)
```

## Complete Configuration Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    private PopupControlContainer popupControlContainer1;
    private Button showPopupButton;

    public Form1()
    {
        InitializeComponent();
        InitializePopup();
    }

    private void InitializePopup()
    {
        // Create popup container
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(250, 200);
        
        // Enable auto-scroll
        this.popupControlContainer1.AutoScroll = true;
        
        // Set scroll margin (2 pixels on all sides)
        this.popupControlContainer1.AutoScrollMargin = new Size(2, 2);
        
        // Set minimum scroll region size
        this.popupControlContainer1.AutoScrollMinSize = new Size(300, 250);
        
        // Add multiple child controls
        for (int i = 0; i < 10; i++)
        {
            Button btn = new Button();
            btn.Text = $"Button {i + 1}";
            btn.Size = new Size(200, 30);
            btn.Location = new Point(10, 10 + (i * 40));
            this.popupControlContainer1.Controls.Add(btn);
        }
        
        // Create trigger button
        this.showPopupButton = new Button();
        this.showPopupButton.Text = "Show Popup";
        this.showPopupButton.Size = new Size(120, 30);
        this.showPopupButton.Location = new Point(50, 50);
        this.showPopupButton.Click += ShowPopupButton_Click;
        
        this.popupControlContainer1.ParentControl = this.showPopupButton;
        this.Controls.Add(this.showPopupButton);
    }

    private void ShowPopupButton_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Private popupControlContainer1 As PopupControlContainer
    Private showPopupButton As Button
    
    Public Sub New()
        InitializeComponent()
        InitializePopup()
    End Sub
    
    Private Sub InitializePopup()
        ' Create popup container
        Me.popupControlContainer1 = New PopupControlContainer()
        Me.popupControlContainer1.Size = New Size(250, 200)
        
        ' Enable auto-scroll
        Me.popupControlContainer1.AutoScroll = True
        
        ' Set scroll margin (2 pixels on all sides)
        Me.popupControlContainer1.AutoScrollMargin = New Size(2, 2)
        
        ' Set minimum scroll region size
        Me.popupControlContainer1.AutoScrollMinSize = New Size(300, 250)
        
        ' Add multiple child controls
        For i As Integer = 0 To 9
            Dim btn As New Button()
            btn.Text = $"Button {i + 1}"
            btn.Size = New Size(200, 30)
            btn.Location = New Point(10, 10 + (i * 40))
            Me.popupControlContainer1.Controls.Add(btn)
        Next
        
        ' Create trigger button
        Me.showPopupButton = New Button()
        Me.showPopupButton.Text = "Show Popup"
        Me.showPopupButton.Size = New Size(120, 30)
        Me.showPopupButton.Location = New Point(50, 50)
        AddHandler Me.showPopupButton.Click, AddressOf ShowPopupButton_Click
        
        Me.popupControlContainer1.ParentControl = Me.showPopupButton
        Me.Controls.Add(Me.showPopupButton)
    End Sub
    
    Private Sub ShowPopupButton_Click(sender As Object, e As EventArgs)
        Me.popupControlContainer1.ShowPopup(Point.Empty)
    End Sub
End Class
```

## Dynamic Content Example

When content changes dynamically, AutoScroll adapts automatically:

```csharp
public partial class DynamicContentForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private FlowLayoutPanel contentPanel;
    private Button addItemButton;

    private void InitializePopup()
    {
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(300, 250);
        this.popupControlContainer1.AutoScroll = true;
        this.popupControlContainer1.AutoScrollMargin = new Size(5, 5);
        
        // Use FlowLayoutPanel for dynamic content
        this.contentPanel = new FlowLayoutPanel();
        this.contentPanel.FlowDirection = FlowDirection.TopDown;
        this.contentPanel.Dock = DockStyle.Fill;
        this.contentPanel.WrapContents = false;
        this.contentPanel.AutoSize = true;
        
        this.popupControlContainer1.Controls.Add(this.contentPanel);
        
        // Add button to dynamically add items
        this.addItemButton = new Button();
        this.addItemButton.Text = "Add Item";
        this.addItemButton.Click += AddItemButton_Click;
        this.contentPanel.Controls.Add(this.addItemButton);
    }

    private void AddItemButton_Click(object sender, EventArgs e)
    {
        // Add new control dynamically
        Label label = new Label();
        label.Text = $"Item {this.contentPanel.Controls.Count}";
        label.AutoSize = true;
        label.Padding = new Padding(5);
        this.contentPanel.Controls.Add(label);
        
        // Scrollbars appear automatically as content grows
    }
}
```

## Common Patterns

### Pattern 1: Fixed Popup with Scrollable Content

```csharp
// Keep popup at fixed size, scroll internal content
this.popupControlContainer1.Size = new Size(300, 200);
this.popupControlContainer1.AutoScroll = true;
this.popupControlContainer1.AutoScrollMinSize = new Size(280, 400);
// Content exceeds 200px height, scrollbar appears
```

### Pattern 2: Dynamic List

```csharp
// Populate with items from data source
this.popupControlContainer1.AutoScroll = true;

foreach (var item in dataSource)
{
    CheckBox chk = new CheckBox();
    chk.Text = item.Name;
    chk.Location = new Point(10, yPosition);
    this.popupControlContainer1.Controls.Add(chk);
    yPosition += 25;
}
// Scrollbar appears if list exceeds popup height
```

### Pattern 3: Responsive Margin

```csharp
// Adjust margin based on content
if (contentCount > 10)
{
    this.popupControlContainer1.AutoScrollMargin = new Size(5, 5);
}
else
{
    this.popupControlContainer1.AutoScrollMargin = new Size(2, 2);
}
```

## Best Practices

1. **Always enable AutoScroll** when content size is uncertain or dynamic
2. **Set AutoScrollMinSize** larger than the popup size to trigger scrolling
3. **Use appropriate margins** to prevent content from touching edges (2-5 pixels recommended)
4. **Test with maximum content** to ensure scrollbars appear correctly
5. **Consider using layout panels** (FlowLayoutPanel, TableLayoutPanel) for dynamic content
6. **Don't rely on fixed positions** when AutoScroll is enabled; use layout controls instead

## Troubleshooting

**Problem:** Scrollbars don't appear
- **Solution:** Ensure `AutoScroll = true` and content actually exceeds the popup size
- **Solution:** Check that `AutoScrollMinSize` is set appropriately

**Problem:** Content is cut off without scrollbars
- **Solution:** Verify `AutoScroll` is enabled
- **Solution:** Make sure child controls are added to the popup's Controls collection

**Problem:** Too much white space in scrollable area
- **Solution:** Adjust `AutoScrollMargin` to reduce padding
- **Solution:** Verify child control positions and sizes are correct

**Problem:** Scrollbar appears when not needed
- **Solution:** Reduce `AutoScrollMinSize` to match actual content requirements
- **Solution:** Check for invisible controls or incorrect positioning

**Problem:** Horizontal scrollbar appears unexpectedly
- **Solution:** Ensure child controls don't exceed the popup width
- **Solution:** Set `AutoScrollMinSize.Width` equal to or less than popup width

## Summary

- **AutoScroll = true:** Enables automatic scrollbars when content overflows
- **AutoScrollMargin:** Sets padding around scrollable content (recommended: 2-5 pixels)
- **AutoScrollMinSize:** Defines minimum scrollable region size (set larger than popup for scrolling)
- Works seamlessly with dynamic content and layout panels
- Improves UX by ensuring all content is accessible regardless of popup size
