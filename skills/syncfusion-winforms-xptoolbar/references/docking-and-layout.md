# Docking and Layout

## Table of Contents
- [Overview](#overview)
- [Docking Positions](#docking-positions)
- [Container Requirements](#container-requirements)
- [Top Docking (Detailed)](#top-docking-detailed)
- [Bottom Docking (Detailed)](#bottom-docking-detailed)
- [Left/Right Docking (Detailed)](#leftright-docking-detailed)
- [Multiple Toolbar Levels](#multiple-toolbar-levels)
- [Best Practices](#best-practices)

## Overview

The XPToolBar control provides flexible positioning capabilities through the Dock property, allowing you to place toolbars at any edge of the form or container. Proper docking and layout management is essential for creating professional-looking applications with intuitive toolbar placement. The control supports Top, Bottom, Left, Right, Fill, and None docking styles.

## Docking Positions

The XPToolBar supports all standard Windows Forms docking positions through the `Dock` property:

### Top (Most Common)

Top docking places the toolbar at the top of its container, making it ideal for menu bars and primary toolbars.

```csharp
xpToolBar1.Dock = DockStyle.Top;
```

```vb
xpToolBar1.Dock = DockStyle.Top
```

### Bottom

Bottom docking positions the toolbar at the bottom of the container, commonly used for status bars or command bars.

```csharp
xpToolBar1.Dock = DockStyle.Bottom;
```

```vb
xpToolBar1.Dock = DockStyle.Bottom
```

### Left

Left docking creates a vertical toolbar on the left side of the container.

```csharp
xpToolBar1.Dock = DockStyle.Left;
```

```vb
xpToolBar1.Dock = DockStyle.Left
```

### Right

Right docking creates a vertical toolbar on the right side of the container.

```csharp
xpToolBar1.Dock = DockStyle.Right;
```

```vb
xpToolBar1.Dock = DockStyle.Right
```

## Container Requirements

### Why Panel is Required

The XPToolBar control **must be hosted within a container control** (typically a Panel) for proper functionality. Direct placement on the form is not supported. The container provides:

- **Layout Management** - Proper sizing and positioning of the toolbar
- **Docking Support** - Correct docking behavior within the form
- **Multiple Toolbar Stacking** - Ability to stack multiple toolbars
- **Isolation** - Separation from other form controls

### Panel Configuration

When using a Panel as the container:

1. The Panel should be docked to the desired edge of the form
2. The XPToolBar should be added to the Panel's Controls collection
3. The XPToolBar can then be docked within the Panel

### Dock Property on Panel vs XPToolBar

You have two options for docking configuration:

**Option 1: Dock the Panel**
```csharp
panel1.Dock = DockStyle.Top;  // Panel docked to form
xpToolBar1.Dock = DockStyle.Top;  // Toolbar docked within panel
panel1.Controls.Add(xpToolBar1);
```

**Option 2: Position the Panel**
```csharp
panel1.Location = new Point(0, 0);
panel1.Size = new Size(800, 32);
xpToolBar1.Dock = DockStyle.Top;
panel1.Controls.Add(xpToolBar1);
```

### Code Example Showing Proper Structure

```csharp
// Create panel container
Panel toolbarPanel = new Panel();
toolbarPanel.Dock = DockStyle.Top;
toolbarPanel.Height = 32;

// Create toolbar
XPToolBar xpToolBar1 = new XPToolBar();
xpToolBar1.Dock = DockStyle.Top;

// Add bar items
BarItem fileItem = new BarItem() { Text = "File" };
BarItem editItem = new BarItem() { Text = "Edit" };
xpToolBar1.Bar.Items.AddRange(new BarItem[] { fileItem, editItem });

// Add toolbar to panel, panel to form
toolbarPanel.Controls.Add(xpToolBar1);
this.Controls.Add(toolbarPanel);
```

```vb
' Create panel container
Dim toolbarPanel As New Panel()
toolbarPanel.Dock = DockStyle.Top
toolbarPanel.Height = 32

' Create toolbar
Dim xpToolBar1 As New XPToolBar()
xpToolBar1.Dock = DockStyle.Top

' Add bar items
Dim fileItem As New BarItem() With {.Text = "File"}
Dim editItem As New BarItem() With {.Text = "Edit"}
xpToolBar1.Bar.Items.AddRange(New BarItem() { fileItem, editItem })

' Add toolbar to panel, panel to form
toolbarPanel.Controls.Add(xpToolBar1)
Me.Controls.Add(toolbarPanel)
```

## Top Docking (Detailed)

### Standard Menu Bar Scenario

Top docking is the most common configuration for menu bars and primary toolbars in desktop applications.

### Panel Setup

```csharp
// Configure panel for top docking
Panel topPanel = new Panel();
topPanel.Dock = DockStyle.Top;
topPanel.Height = 30;  // Adjust height as needed
topPanel.BorderStyle = BorderStyle.None;
```

```vb
' Configure panel for top docking
Dim topPanel As New Panel()
topPanel.Dock = DockStyle.Top
topPanel.Height = 30  ' Adjust height as needed
topPanel.BorderStyle = BorderStyle.None
```

### Complete Code Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class TopDockingExample : Form
{
    private Panel menuPanel;
    private XPToolBar menuBar;

    public TopDockingExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Create panel for menu bar
        menuPanel = new Panel();
        menuPanel.Dock = DockStyle.Top;
        menuPanel.Height = 32;

        // Create menu bar
        menuBar = new XPToolBar();
        menuBar.Dock = DockStyle.Top;

        // Create menu items
        ParentBarItem fileMenu = new ParentBarItem();
        fileMenu.Text = "File";
        
        BarItem newItem = new BarItem() { Text = "New" };
        BarItem openItem = new BarItem() { Text = "Open" };
        BarItem saveItem = new BarItem() { Text = "Save" };
        
        fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });

        ParentBarItem editMenu = new ParentBarItem();
        editMenu.Text = "Edit";
        
        BarItem cutItem = new BarItem() { Text = "Cut" };
        BarItem copyItem = new BarItem() { Text = "Copy" };
        BarItem pasteItem = new BarItem() { Text = "Paste" };
        
        editMenu.Items.AddRange(new BarItem[] { cutItem, copyItem, pasteItem });

        // Add menus to toolbar
        menuBar.Bar.Items.AddRange(new BarItem[] { fileMenu, editMenu });

        // Add toolbar to panel
        menuPanel.Controls.Add(menuBar);

        // Add panel to form
        this.Controls.Add(menuPanel);

        // Form settings
        this.Text = "Top Docked Menu Bar";
        this.Size = new System.Drawing.Size(800, 600);
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools.XPMenus

Public Class TopDockingExample
    Inherits Form
    
    Private menuPanel As Panel
    Private menuBar As XPToolBar

    Public Sub New()
        InitializeComponents()
    End Sub

    Private Sub InitializeComponents()
        ' Create panel for menu bar
        menuPanel = New Panel()
        menuPanel.Dock = DockStyle.Top
        menuPanel.Height = 32

        ' Create menu bar
        menuBar = New XPToolBar()
        menuBar.Dock = DockStyle.Top

        ' Create menu items
        Dim fileMenu As New ParentBarItem()
        fileMenu.Text = "File"
        
        Dim newItem As New BarItem() With {.Text = "New"}
        Dim openItem As New BarItem() With {.Text = "Open"}
        Dim saveItem As New BarItem() With {.Text = "Save"}
        
        fileMenu.Items.AddRange(New BarItem() { newItem, openItem, saveItem })

        Dim editMenu As New ParentBarItem()
        editMenu.Text = "Edit"
        
        Dim cutItem As New BarItem() With {.Text = "Cut"}
        Dim copyItem As New BarItem() With {.Text = "Copy"}
        Dim pasteItem As New BarItem() With {.Text = "Paste"}
        
        editMenu.Items.AddRange(New BarItem() { cutItem, copyItem, pasteItem })

        ' Add menus to toolbar
        menuBar.Bar.Items.AddRange(New BarItem() { fileMenu, editMenu })

        ' Add toolbar to panel
        menuPanel.Controls.Add(menuBar)

        ' Add panel to form
        Me.Controls.Add(menuPanel)

        ' Form settings
        Me.Text = "Top Docked Menu Bar"
        Me.Size = New System.Drawing.Size(800, 600)
    End Sub
End Class
```

## Bottom Docking (Detailed)

### Status/Command Bar Scenario

Bottom docking is commonly used for status bars, command bars, or secondary toolbars that provide contextual actions.

### Panel Setup

```csharp
// Configure panel for bottom docking
Panel bottomPanel = new Panel();
bottomPanel.Dock = DockStyle.Bottom;
bottomPanel.Height = 30;
bottomPanel.BorderStyle = BorderStyle.FixedSingle;
```

```vb
' Configure panel for bottom docking
Dim bottomPanel As New Panel()
bottomPanel.Dock = DockStyle.Bottom
bottomPanel.Height = 30
bottomPanel.BorderStyle = BorderStyle.FixedSingle
```

### Complete Code Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class BottomDockingExample : Form
{
    private Panel statusPanel;
    private XPToolBar statusBar;

    public BottomDockingExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Create panel for status bar
        statusPanel = new Panel();
        statusPanel.Dock = DockStyle.Bottom;
        statusPanel.Height = 30;
        statusPanel.BorderStyle = BorderStyle.FixedSingle;

        // Create status bar
        statusBar = new XPToolBar();
        statusBar.Dock = DockStyle.Bottom;

        // Create status items
        StaticBarItem statusLabel = new StaticBarItem();
        statusLabel.Text = "Ready";
        statusLabel.SizeToFit = true;

        StaticBarItem lineLabel = new StaticBarItem();
        lineLabel.Text = "Line: 1";
        lineLabel.SizeToFit = true;

        StaticBarItem columnLabel = new StaticBarItem();
        columnLabel.Text = "Col: 1";
        columnLabel.SizeToFit = true;

        // Add items to status bar
        statusBar.Bar.Items.AddRange(new BarItem[] {
            statusLabel,
            lineLabel,
            columnLabel
        });

        // Add separators
        statusBar.SeparatorIndices.AddRange(new int[] { 0, 1 });

        // Add toolbar to panel
        statusPanel.Controls.Add(statusBar);

        // Add panel to form
        this.Controls.Add(statusPanel);

        // Form settings
        this.Text = "Bottom Docked Status Bar";
        this.Size = new System.Drawing.Size(800, 600);
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools.XPMenus

Public Class BottomDockingExample
    Inherits Form
    
    Private statusPanel As Panel
    Private statusBar As XPToolBar

    Public Sub New()
        InitializeComponents()
    End Sub

    Private Sub InitializeComponents()
        ' Create panel for status bar
        statusPanel = New Panel()
        statusPanel.Dock = DockStyle.Bottom
        statusPanel.Height = 30
        statusPanel.BorderStyle = BorderStyle.FixedSingle

        ' Create status bar
        statusBar = New XPToolBar()
        statusBar.Dock = DockStyle.Bottom

        ' Create status items
        Dim statusLabel As New StaticBarItem()
        statusLabel.Text = "Ready"
        statusLabel.SizeToFit = True

        Dim lineLabel As New StaticBarItem()
        lineLabel.Text = "Line: 1"
        lineLabel.SizeToFit = True

        Dim columnLabel As New StaticBarItem()
        columnLabel.Text = "Col: 1"
        columnLabel.SizeToFit = True

        ' Add items to status bar
        statusBar.Bar.Items.AddRange(New BarItem() {
            statusLabel,
            lineLabel,
            columnLabel
        })

        ' Add separators
        statusBar.SeparatorIndices.AddRange(New Integer() { 0, 1 })

        ' Add toolbar to panel
        statusPanel.Controls.Add(statusBar)

        ' Add panel to form
        Me.Controls.Add(statusPanel)

        ' Form settings
        Me.Text = "Bottom Docked Status Bar"
        Me.Size = New System.Drawing.Size(800, 600)
    End Sub
End Class
```

## Left/Right Docking (Detailed)

### Vertical Toolbar Scenario

Left and right docking creates vertical toolbars, useful for tool palettes, navigation bars, or quick access buttons.

### Panel Setup and Considerations

When creating vertical toolbars:
- Consider the width carefully (typically 40-80 pixels)
- Use icons rather than text for better appearance
- Ensure icons are appropriately sized (16x16 or 32x32)

```csharp
// Configure panel for left docking
Panel leftPanel = new Panel();
leftPanel.Dock = DockStyle.Left;
leftPanel.Width = 50;  // Width instead of Height for vertical
```

```vb
' Configure panel for left docking
Dim leftPanel As New Panel()
leftPanel.Dock = DockStyle.Left
leftPanel.Width = 50  ' Width instead of Height for vertical
```

### Complete Code Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class VerticalDockingExample : Form
{
    private Panel toolPanel;
    private XPToolBar verticalToolbar;

    public VerticalDockingExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Create panel for vertical toolbar
        toolPanel = new Panel();
        toolPanel.Dock = DockStyle.Left;
        toolPanel.Width = 50;

        // Create vertical toolbar
        verticalToolbar = new XPToolBar();
        verticalToolbar.Dock = DockStyle.Left;

        // Create toolbar items with icons
        BarItem newItem = new BarItem();
        newItem.Image = LoadIcon("New.png");
        newItem.ToolTip = "New";
        newItem.SizeToFit = true;

        BarItem openItem = new BarItem();
        openItem.Image = LoadIcon("Open.png");
        openItem.ToolTip = "Open";
        openItem.SizeToFit = true;

        BarItem saveItem = new BarItem();
        saveItem.Image = LoadIcon("Save.png");
        saveItem.ToolTip = "Save";
        saveItem.SizeToFit = true;

        BarItem printItem = new BarItem();
        printItem.Image = LoadIcon("Print.png");
        printItem.ToolTip = "Print";
        printItem.SizeToFit = true;

        // Add items to toolbar
        verticalToolbar.Bar.Items.AddRange(new BarItem[] {
            newItem,
            openItem,
            saveItem,
            printItem
        });

        // Add separators
        verticalToolbar.SeparatorIndices.AddRange(new int[] { 1, 2 });

        // Add toolbar to panel
        toolPanel.Controls.Add(verticalToolbar);

        // Add panel to form
        this.Controls.Add(toolPanel);

        // Form settings
        this.Text = "Left Docked Vertical Toolbar";
        this.Size = new Size(800, 600);
    }

    private Image LoadIcon(string filename)
    {
        // Replace with actual image loading logic
        return new Bitmap(32, 32);
    }
}
```

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools.XPMenus

Public Class VerticalDockingExample
    Inherits Form
    
    Private toolPanel As Panel
    Private verticalToolbar As XPToolBar

    Public Sub New()
        InitializeComponents()
    End Sub

    Private Sub InitializeComponents()
        ' Create panel for vertical toolbar
        toolPanel = New Panel()
        toolPanel.Dock = DockStyle.Left
        toolPanel.Width = 50

        ' Create vertical toolbar
        verticalToolbar = New XPToolBar()
        verticalToolbar.Dock = DockStyle.Left

        ' Create toolbar items with icons
        Dim newItem As New BarItem()
        newItem.Image = LoadIcon("New.png")
        newItem.ToolTip = "New"
        newItem.SizeToFit = True

        Dim openItem As New BarItem()
        openItem.Image = LoadIcon("Open.png")
        openItem.ToolTip = "Open"
        openItem.SizeToFit = True

        Dim saveItem As New BarItem()
        saveItem.Image = LoadIcon("Save.png")
        saveItem.ToolTip = "Save"
        saveItem.SizeToFit = True

        Dim printItem As New BarItem()
        printItem.Image = LoadIcon("Print.png")
        printItem.ToolTip = "Print"
        printItem.SizeToFit = True

        ' Add items to toolbar
        verticalToolbar.Bar.Items.AddRange(New BarItem() {
            newItem,
            openItem,
            saveItem,
            printItem
        })

        ' Add separators
        verticalToolbar.SeparatorIndices.AddRange(New Integer() { 1, 2 })

        ' Add toolbar to panel
        toolPanel.Controls.Add(verticalToolbar)

        ' Add panel to form
        Me.Controls.Add(toolPanel)

        ' Form settings
        Me.Text = "Left Docked Vertical Toolbar"
        Me.Size = New Size(800, 600)
    End Sub

    Private Function LoadIcon(ByVal filename As String) As Image
        ' Replace with actual image loading logic
        Return New Bitmap(32, 32)
    End Function
End Class
```

## Multiple Toolbar Levels

### Stacking Multiple Toolbars

You can stack multiple XPToolBar controls by using separate panels or by adding multiple toolbars to the same panel.

### Panel-per-Toolbar Pattern

The recommended approach is to use one Panel per toolbar for better control:

```csharp
// First toolbar (menu bar)
Panel menuPanel = new Panel();
menuPanel.Dock = DockStyle.Top;
menuPanel.Height = 30;

XPToolBar menuBar = new XPToolBar();
menuBar.Dock = DockStyle.Top;
menuPanel.Controls.Add(menuBar);

// Second toolbar (quick access)
Panel quickPanel = new Panel();
quickPanel.Dock = DockStyle.Top;
quickPanel.Height = 35;

XPToolBar quickToolbar = new XPToolBar();
quickToolbar.Dock = DockStyle.Top;
quickPanel.Controls.Add(quickToolbar);

// Add to form in reverse order (bottom to top)
this.Controls.Add(quickPanel);
this.Controls.Add(menuPanel);
```

### Docking Order Importance

**Critical:** When adding docked panels to a form, add them in **reverse order** of how you want them to appear:

```csharp
// To display: Menu (top) → Quick Access → Formatting
// Add in reverse order:
this.Controls.Add(formattingPanel);  // Will appear third from top
this.Controls.Add(quickPanel);       // Will appear second from top
this.Controls.Add(menuPanel);        // Will appear first (top)
```

### Menu Bar + Quick Access Toolbar Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class MultipleToolbarsExample : Form
{
    private Panel menuPanel;
    private Panel quickAccessPanel;
    private XPToolBar menuBar;
    private XPToolBar quickAccessToolbar;

    public MultipleToolbarsExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Panel 1: Menu Bar
        menuPanel = new Panel();
        menuPanel.Dock = DockStyle.Top;
        menuPanel.Height = 30;

        menuBar = new XPToolBar();
        menuBar.Dock = DockStyle.Top;

        // Add menu items
        ParentBarItem fileMenu = new ParentBarItem() { Text = "File" };
        ParentBarItem editMenu = new ParentBarItem() { Text = "Edit" };
        ParentBarItem viewMenu = new ParentBarItem() { Text = "View" };

        menuBar.Bar.Items.AddRange(new BarItem[] { fileMenu, editMenu, viewMenu });
        menuPanel.Controls.Add(menuBar);

        // Panel 2: Quick Access Toolbar
        quickAccessPanel = new Panel();
        quickAccessPanel.Dock = DockStyle.Top;
        quickAccessPanel.Height = 35;

        quickAccessToolbar = new XPToolBar();
        quickAccessToolbar.Dock = DockStyle.Top;

        // Add quick access items
        BarItem newItem = new BarItem();
        newItem.Image = CreateSampleIcon();
        newItem.ToolTip = "New";
        newItem.SizeToFit = true;

        BarItem openItem = new BarItem();
        openItem.Image = CreateSampleIcon();
        openItem.ToolTip = "Open";
        openItem.SizeToFit = true;

        BarItem saveItem = new BarItem();
        saveItem.Image = CreateSampleIcon();
        saveItem.ToolTip = "Save";
        saveItem.SizeToFit = true;

        quickAccessToolbar.Bar.Items.AddRange(new BarItem[] {
            newItem, openItem, saveItem
        });
        quickAccessPanel.Controls.Add(quickAccessToolbar);

        // Add panels to form in reverse order
        this.Controls.Add(quickAccessPanel);
        this.Controls.Add(menuPanel);

        // Form settings
        this.Text = "Multiple Toolbars Example";
        this.Size = new Size(800, 600);
    }

    private Image CreateSampleIcon()
    {
        Bitmap bmp = new Bitmap(16, 16);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.FillRectangle(Brushes.LightBlue, 0, 0, 16, 16);
        }
        return bmp;
    }
}
```

### Three-Level Toolbar Example (Menu + Quick + Formatting)

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class ThreeLevelToolbarsExample : Form
{
    public ThreeLevelToolbarsExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Level 1: Menu Bar
        Panel menuPanel = new Panel() { Dock = DockStyle.Top, Height = 30 };
        XPToolBar menuBar = new XPToolBar() { Dock = DockStyle.Top };
        
        menuBar.Bar.Items.AddRange(new BarItem[] {
            new ParentBarItem() { Text = "File" },
            new ParentBarItem() { Text = "Edit" },
            new ParentBarItem() { Text = "View" },
            new ParentBarItem() { Text = "Tools" }
        });
        menuPanel.Controls.Add(menuBar);

        // Level 2: Quick Access Toolbar
        Panel quickPanel = new Panel() { Dock = DockStyle.Top, Height = 32 };
        XPToolBar quickToolbar = new XPToolBar() { Dock = DockStyle.Top };
        
        quickToolbar.Bar.Items.AddRange(new BarItem[] {
            new BarItem() { Image = CreateIcon(Color.LightGreen), ToolTip = "New" },
            new BarItem() { Image = CreateIcon(Color.LightBlue), ToolTip = "Open" },
            new BarItem() { Image = CreateIcon(Color.LightCoral), ToolTip = "Save" }
        });
        quickToolbar.SeparatorIndices.Add(1);
        quickPanel.Controls.Add(quickToolbar);

        // Level 3: Formatting Toolbar
        Panel formatPanel = new Panel() { Dock = DockStyle.Top, Height = 32 };
        XPToolBar formatToolbar = new XPToolBar() { Dock = DockStyle.Top };
        
        ComboBoxBarItem fontCombo = new ComboBoxBarItem();
        fontCombo.ChoiceList.AddRange(new string[] { "Arial", "Times New Roman", "Courier New" });
        fontCombo.TextBoxValue = "Arial";
        fontCombo.MinWidth = 120;

        ComboBoxBarItem sizeCombo = new ComboBoxBarItem();
        sizeCombo.ChoiceList.AddRange(new string[] { "8", "10", "12", "14", "16" });
        sizeCombo.TextBoxValue = "12";
        sizeCombo.MinWidth = 50;

        formatToolbar.Bar.Items.AddRange(new BarItem[] {
            fontCombo,
            sizeCombo,
            new BarItem() { Text = "B", ToolTip = "Bold" },
            new BarItem() { Text = "I", ToolTip = "Italic" },
            new BarItem() { Text = "U", ToolTip = "Underline" }
        });
        formatToolbar.SeparatorIndices.AddRange(new int[] { 1 });
        formatPanel.Controls.Add(formatToolbar);

        // Add panels to form in REVERSE order (bottom to top)
        this.Controls.Add(formatPanel);   // Will appear third
        this.Controls.Add(quickPanel);    // Will appear second
        this.Controls.Add(menuPanel);     // Will appear first (top)

        // Form settings
        this.Text = "Three-Level Toolbars";
        this.Size = new Size(900, 600);
    }

    private Image CreateIcon(Color color)
    {
        Bitmap bmp = new Bitmap(16, 16);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.FillRectangle(new SolidBrush(color), 0, 0, 16, 16);
            g.DrawRectangle(Pens.Black, 0, 0, 15, 15);
        }
        return bmp;
    }
}
```

## Best Practices

### Panel Management Tips

1. **Use Descriptive Names**: Name panels clearly (e.g., `menuPanel`, `quickAccessPanel`, `statusPanel`)

2. **Set Appropriate Heights/Widths**:
   - Menu bars: 28-32 pixels
   - Standard toolbars: 30-35 pixels
   - Icon-only toolbars: 35-40 pixels
   - Vertical toolbars: 40-80 pixels width

3. **Consider Border Styles**:
   ```csharp
   panel.BorderStyle = BorderStyle.None;  // For seamless look
   panel.BorderStyle = BorderStyle.FixedSingle;  // For defined boundary
   ```

4. **Use Dock.Fill for Content Area**:
   ```csharp
   Panel contentPanel = new Panel();
   contentPanel.Dock = DockStyle.Fill;  // Fills remaining space
   ```

### Docking Order for Stacked Toolbars

Always add docked panels in **reverse order** of display:

```csharp
// Display order (top to bottom): Menu → Quick → Formatting
// Add order: Formatting → Quick → Menu
this.Controls.Add(formattingPanel);
this.Controls.Add(quickAccessPanel);
this.Controls.Add(menuPanel);
```

### Height/Width Considerations

- **Menu bars**: Keep height at 28-32 pixels for standard text
- **Icon toolbars**: Use 35-40 pixels for 16x16 or 24x24 icons
- **Combo boxes**: Ensure minimum height of 32 pixels for proper rendering
- **Vertical toolbars**: Width of 40-80 pixels works well for icons

### Layout Tips

1. **Consistent Spacing**: Use consistent panel heights across toolbars at the same level

2. **Separator Usage**: Use separators to visually group related items:
   ```csharp
   toolbar.SeparatorIndices.AddRange(new int[] { 2, 5, 8 });
   ```

3. **SizeToFit Property**: Enable for items that should auto-size:
   ```csharp
   barItem.SizeToFit = true;
   ```

4. **MinWidth for ComboBoxes**: Always set minimum width for combo boxes:
   ```csharp
   comboBox.MinWidth = 100;
   ```

5. **Responsive Design**: Consider how toolbars behave when form is resized

6. **Z-Order Management**: Add panels in correct order to ensure proper layering

By following these best practices and understanding the docking system, you can create professional, well-organized toolbar layouts that enhance your application's usability and appearance.
