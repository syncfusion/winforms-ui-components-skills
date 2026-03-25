# Getting Started with ContextMenuStripEx

This guide covers installation, basic setup, and initial implementation of the ContextMenuStripEx control in Windows Forms applications.

## Overview

ContextMenuStripEx is an enhanced context menu control that appears when users right-click on associated controls. It provides a rich set of features beyond the standard Windows Forms ContextMenuStrip, including support for multiple item types and extensive customization options.

## Prerequisites

### Licensing Requirement

**Important:** Starting with v16.2.0.x, you must include a Syncfusion license key in your projects when referencing Syncfusion assemblies from trial setup or NuGet feed.

Register your license key before initializing any Syncfusion controls:

```csharp
// In Program.cs or application startup
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

Learn more about [Syncfusion licensing](https://help.syncfusion.com/common/essential-studio/licensing/overview).

### Required Assemblies

Add the following assembly references to use ContextMenuStripEx:

- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Licensing.dll` (v16.2.0.x and later)

**Assembly Location:**
```
{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\{Platform}\{Build Version}\precompiledassemblies\{Framework Version}
```

## Installation via NuGet

The recommended approach is to install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.Windows
```

This automatically adds all required assembly references and manages dependencies.

**Learn more:** [Installing NuGet packages in WinForms](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

## Adding ContextMenuStripEx via Designer

The designer approach provides visual feedback and simplifies configuration.

### Step 1: Add Control to Form

1. Open your form in the Visual Studio designer
2. Locate the Syncfusion toolbox section: **"Syncfusion Windows [VS Version] Toolbox Essential Studio [Version]"**
3. Drag and drop **ContextMenuStripEx** from the toolbox onto the designer

**Result:** The control appears in the component tray below the designer surface. Required assemblies are automatically added to your project.

### Step 2: Add Menu Items

**Option A: Type Here Interface**

1. Click **"Type Here"** in the context menu designer
2. A dropdown appears showing available ToolStripItem types:
   - MenuItem
   - ComboBox
   - TextBox
   - Separator
3. Select the desired item type
4. The item is added to the menu

**Option B: Items Collection Editor**

1. Select the ContextMenuStripEx control in the component tray
2. In the Properties panel, locate the **Items** property
3. Click the ellipsis (...) button to open **Items Collection Editor**
4. Click **Add** dropdown and select ToolStripItem type
5. Configure item properties in the right panel
6. Click OK when finished

### Step 3: Configure Menu Item Properties

1. Right-click the menu item in the designer
2. Select **Properties** from the context menu
3. Configure properties in the Properties panel:

**Common Properties to Set:**
- **Appearance → Text:** Display text for the menu item
- **Behavior → Enabled:** Whether item is enabled
- **Behavior → Checked:** Whether check mark appears
- **Behavior → ShortcutKeys:** Keyboard shortcut

### Step 4: Associate with a Control

Context menus must be associated with controls to appear on right-click.

1. Drag any control onto your form (e.g., RichTextBox, Button, Label, TextBox)
2. Right-click the control and select **Properties**
3. Locate **Behavior → ContextMenuStrip** property
4. Select your ContextMenuStripEx instance from the dropdown

**Supported Controls:**
You can associate ContextMenuStripEx with any Windows Forms control:
- RichTextBox, TextBox, MaskedTextBox
- Button, Label, PictureBox
- ListView, TreeView, DataGridView
- Panels, GroupBoxes, custom controls

### Step 5: Wire Up Events (Optional)

To handle menu item clicks:

1. Select a menu item in the designer
2. In the Properties panel, click the Events button (lightning bolt icon)
3. Double-click the **Click** event or type an event handler name
4. Visual Studio generates the event handler method

## Adding ContextMenuStripEx via Code

The code approach provides full programmatic control and is ideal for dynamic menu creation.

### Step 1: Add Assembly References

Manually add references to the required assemblies listed in [Required Assemblies](#required-assemblies).

### Step 2: Declare Controls

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

public partial class Form1 : Form
{
    // Declarations
    private ContextMenuStripEx contextMenuStripEx;
    private ToolStripMenuItem toolStripMenuItem1;
    private ToolStripMenuItem toolStripMenuItem2;
    private ToolStripMenuItem toolStripMenuItem3;
    private RichTextBox richTextBox1;
```

### Step 3: Initialize Controls

```csharp
    public Form1()
    {
        InitializeComponent();
        InitializeContextMenu();
    }

    private void InitializeContextMenu()
    {
        // Initialize context menu
        this.contextMenuStripEx = new ContextMenuStripEx();
        
        // Initialize menu items
        this.toolStripMenuItem1 = new ToolStripMenuItem();
        this.toolStripMenuItem2 = new ToolStripMenuItem();
        this.toolStripMenuItem3 = new ToolStripMenuItem();
        
        // Initialize target control
        this.richTextBox1 = new RichTextBox();
```

### Step 4: Configure Menu Items

```csharp
        // Configure first menu item
        this.toolStripMenuItem1.Text = "New";
        this.toolStripMenuItem1.ShortcutKeys = Keys.Control | Keys.N;
        
        // Configure second menu item
        this.toolStripMenuItem2.Text = "Copy";
        this.toolStripMenuItem2.ShortcutKeys = Keys.Control | Keys.C;
        
        // Configure third menu item
        this.toolStripMenuItem3.Text = "Cut";
        this.toolStripMenuItem3.ShortcutKeys = Keys.Control | Keys.X;
```

### Step 5: Add Items to Context Menu

```csharp
        // Add items to context menu
        this.contextMenuStripEx.Items.AddRange(new ToolStripItem[] {
            this.toolStripMenuItem1,
            this.toolStripMenuItem2,
            this.toolStripMenuItem3
        });
```

### Step 6: Associate with Control

```csharp
        // Associate context menu with control
        this.richTextBox1.ContextMenuStrip = this.contextMenuStripEx;
        
        // Add control to form
        this.richTextBox1.Location = new System.Drawing.Point(10, 10);
        this.richTextBox1.Size = new System.Drawing.Size(400, 300);
        this.Controls.Add(this.richTextBox1);
    }
}
```

### Complete Code Example

**C# Complete Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private ContextMenuStripEx contextMenuStripEx;
    private ToolStripMenuItem toolStripMenuItem1;
    private ToolStripMenuItem toolStripMenuItem2;
    private ToolStripMenuItem toolStripMenuItem3;
    private RichTextBox richTextBox1;

    public Form1()
    {
        InitializeComponent();
        InitializeContextMenu();
    }

    private void InitializeContextMenu()
    {
        this.contextMenuStripEx = new ContextMenuStripEx();
        this.toolStripMenuItem1 = new ToolStripMenuItem();
        this.toolStripMenuItem2 = new ToolStripMenuItem();
        this.toolStripMenuItem3 = new ToolStripMenuItem();
        this.richTextBox1 = new RichTextBox();

        this.toolStripMenuItem1.Text = "New";
        this.toolStripMenuItem1.ShortcutKeys = Keys.Control | Keys.N;
        
        this.toolStripMenuItem2.Text = "Copy";
        this.toolStripMenuItem2.ShortcutKeys = Keys.Control | Keys.C;
        
        this.toolStripMenuItem3.Text = "Cut";
        this.toolStripMenuItem3.ShortcutKeys = Keys.Control | Keys.X;

        this.contextMenuStripEx.Items.AddRange(new ToolStripItem[] {
            this.toolStripMenuItem1,
            this.toolStripMenuItem2,
            this.toolStripMenuItem3
        });

        this.richTextBox1.ContextMenuStrip = this.contextMenuStripEx;
        this.richTextBox1.Location = new Point(10, 10);
        this.richTextBox1.Size = new Size(400, 300);
        this.Controls.Add(this.richTextBox1);
    }
}
```

**VB.NET Complete Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Private contextMenuStripEx As ContextMenuStripEx
    Private toolStripMenuItem1 As ToolStripMenuItem
    Private toolStripMenuItem2 As ToolStripMenuItem
    Private toolStripMenuItem3 As ToolStripMenuItem
    Private richTextBox1 As RichTextBox

    Public Sub New()
        InitializeComponent()
        InitializeContextMenu()
    End Sub

    Private Sub InitializeContextMenu()
        Me.contextMenuStripEx = New ContextMenuStripEx()
        Me.toolStripMenuItem1 = New ToolStripMenuItem()
        Me.toolStripMenuItem2 = New ToolStripMenuItem()
        Me.toolStripMenuItem3 = New ToolStripMenuItem()
        Me.richTextBox1 = New RichTextBox()

        Me.toolStripMenuItem1.Text = "New"
        Me.toolStripMenuItem1.ShortcutKeys = Keys.Control Or Keys.N
        
        Me.toolStripMenuItem2.Text = "Copy"
        Me.toolStripMenuItem2.ShortcutKeys = Keys.Control Or Keys.C
        
        Me.toolStripMenuItem3.Text = "Cut"
        Me.toolStripMenuItem3.ShortcutKeys = Keys.Control Or Keys.X

        Me.contextMenuStripEx.Items.AddRange(New ToolStripItem() {
            Me.toolStripMenuItem1,
            Me.toolStripMenuItem2,
            Me.toolStripMenuItem3
        })

        Me.richTextBox1.ContextMenuStrip = Me.contextMenuStripEx
        Me.richTextBox1.Location = New Point(10, 10)
        Me.richTextBox1.Size = New Size(400, 300)
        Me.Controls.Add(Me.richTextBox1)
    End Sub
End Class
```

## Testing Your Context Menu

### Verify Basic Functionality

1. **Run the application** (F5 or Debug → Start Debugging)
2. **Right-click the associated control** (e.g., RichTextBox)
3. **Verify menu appears** with your configured items
4. **Click menu items** to ensure they respond (if events are wired)

### Common Testing Scenarios

**Test Menu Display:**
- Right-click should show the menu
- Menu should appear near the mouse cursor
- All configured items should be visible

**Test Menu Items:**
- Text should display correctly
- Enabled items should be clickable
- Disabled items should appear grayed out
- Keyboard shortcuts should display (if configured)

**Test Multiple Controls:**
- Each control can have its own context menu
- Right-clicking different controls shows appropriate menus

## Next Steps

Now that you have a basic context menu working:

1. **Add more item types:** Explore TextBox, ComboBox, and Separator items (see [toolstrip-item-types.md](toolstrip-item-types.md))
2. **Create multi-level menus:** Add submenus for hierarchical navigation (see [multilevel-menus.md](multilevel-menus.md))
3. **Configure item states:** Implement checked/unchecked states and dynamic enabling (see [menu-item-states.md](menu-item-states.md))
4. **Add keyboard shortcuts:** Configure shortcut keys for quick access (see [keyboard-and-touch.md](keyboard-and-touch.md))
5. **Customize appearance:** Apply colors, fonts, and styling (see [appearance-customization.md](appearance-customization.md))
6. **Handle events:** Implement menu behavior and event handling (see [menu-behavior.md](menu-behavior.md))

## Troubleshooting

**Context menu not appearing:**
- Verify the control's ContextMenuStrip property is set correctly
- Ensure the control allows mouse events (not disabled or covered)
- Check that the context menu has at least one item

**Assembly reference errors:**
- Verify all required assemblies are referenced
- Check assembly versions match (don't mix versions)
- Use NuGet to manage dependencies automatically

**License key errors (v16.2.0.x+):**
- Register license key before initializing controls
- Verify license key is valid for your Syncfusion version
- Check that license registration code executes before form creation

**Menu items not visible in designer:**
- Click "Type Here" or use Items Collection Editor
- Refresh the designer (close and reopen form)
- Rebuild the solution

**Events not firing:**
- Verify event handlers are subscribed correctly
- Check that menu items are enabled
- Ensure no exceptions are thrown in event handlers
