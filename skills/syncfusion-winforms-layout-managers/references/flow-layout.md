# FlowLayout

## Table of Contents

- [FlowLayout](#flowlayout)
  - [Table of Contents](#table-of-contents)
  - [What is FlowLayout](#what-is-flowlayout)
  - [Key Features](#key-features)
  - [LayoutMode Property](#layoutmode-property)
  - [Alignment Property](#alignment-property)
  - [Flow Direction](#flow-direction)
  - [AutoHeight Feature](#autoheight-feature)
  - [Spacing Configuration](#spacing-configuration)
  - [Adding Controls via Designer](#adding-controls-via-designer)
  - [Adding Controls via Code](#adding-controls-via-code)
  - [Per-Child Constraints](#per-child-constraints)
    - [HAlign and VAlign](#halign-and-valign)
    - [Layout Participation](#layout-participation)
    - [Line Beginner](#line-beginner)
    - [Row Height and Column Width](#row-height-and-column-width)
  - [Wrapping Behavior](#wrapping-behavior)
  - [Complete Examples](#complete-examples)
    - [Horizontal Toolbar with Wrapping](#horizontal-toolbar-with-wrapping)
    - [Vertical Sidebar Navigation](#vertical-sidebar-navigation)
    - [Tag Cloud (Dynamic Labels)](#tag-cloud-dynamic-labels)
    - [Responsive Button Panel](#responsive-button-panel)
  - [Advanced Constraint Scenarios](#advanced-constraint-scenarios)
  - [Centering Child Controls](#centering-child-controls)
  - [Dynamic Control Management](#dynamic-control-management)
  - [Common Patterns](#common-patterns)
  - [Best Practices](#best-practices)
  - [Troubleshooting](#troubleshooting)

FlowLayout is the **most commonly used** layout manager in Syncfusion Windows Forms. It arranges child controls horizontally or vertically in a specific order with automatic wrapping when space runs out. This versatile layout manager is ideal for creating toolbars, tag lists, responsive panels, and dynamic content layouts.

## What is FlowLayout

FlowLayout is a layout manager that arranges child components in a flow direction (horizontal or vertical) with automatic wrapping capabilities. It is one of the most versatile and widely applicable layout managers in Windows Forms.

**Purpose**: Arrange controls in horizontal or vertical flow with automatic wrapping when the container boundary is reached.

**Key Characteristics**:
- Automatic wrapping when space runs out
- Supports both simple and constraint-based layout modes
- Most commonly used for general-purpose layouts
- Can handle both uniform and variable-sized controls

**When to Use FlowLayout**:
- Creating toolbars with auto-wrap functionality
- Building tag clouds or chip lists
- Designing responsive button panels
- Arranging icon grids
- Creating dynamic content that adapts to container size

## Key Features

FlowLayout provides comprehensive features for flexible control arrangement:

1. **Horizontal or Vertical Layout Modes**: Choose between left-to-right or top-to-bottom flow
2. **Automatic Wrapping**: Controls automatically wrap to new rows or columns
3. **Alignment Options**: Near, Far, Center, or ChildConstraints alignment
4. **Per-Child Constraints**: Set MinSize and PreferredSize for individual controls
5. **Flow Direction Control**: Normal or reverse flow direction (useful for RTL)
6. **AutoHeight Feature**: Automatically adjust container height based on content
7. **Spacing Control**: HGap and VGap for horizontal and vertical spacing

## LayoutMode Property

The LayoutMode property determines whether controls flow horizontally or vertically.

**FlowLayoutMode.Horizontal** (Default):
- Controls flow from left to right
- Wraps to next row when reaching container edge
- Best for toolbars and button panels

**FlowLayoutMode.Vertical**:
- Controls flow from top to bottom
- Wraps to next column when reaching container edge
- Best for sidebar navigation and vertical menus

**Code Example - Horizontal Mode**:

{% tabs %}

{% highlight C# %}

// Horizontal flow (default)
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = this;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;

// Add buttons
for (int i = 1; i <= 10; i++)
{
    ButtonAdv button = new ButtonAdv();
    button.Text = "Button " + i;
    button.Size = new Size(80, 30);
    this.Controls.Add(button);
}

{% endhighlight %}

{% highlight VB %}

' Horizontal flow (default)
Dim flowLayout1 As New FlowLayout()
flowLayout1.ContainerControl = Me
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal

' Add buttons
For i As Integer = 1 To 10
    Dim button As New ButtonAdv()
    button.Text = "Button " & i
    button.Size = New Size(80, 30)
    Me.Controls.Add(button)
Next

{% endhighlight %}

{% endtabs %}

**Code Example - Vertical Mode**:

{% tabs %}

{% highlight C# %}

// Vertical flow
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Vertical;

{% endhighlight %}

{% highlight VB %}

' Vertical flow
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Vertical

{% endhighlight %}

{% endtabs %}

## Alignment Property

The Alignment property controls how controls are aligned within the flow direction.

**FlowAlignment.Near**:
- Aligns controls to the start (left for horizontal, top for vertical)
- Default behavior
- Use for standard left-aligned layouts

**FlowAlignment.Far**:
- Aligns controls to the end (right for horizontal, bottom for vertical)
- Use for right-aligned toolbars or menus

**FlowAlignment.Center**:
- Centers controls in the flow direction
- Use for centered button panels or dialogs

**FlowAlignment.ChildConstraints**:
- Uses per-child constraint settings
- Enables complex, constraint-based layouts
- Most flexible option

**Code Examples**:

{% tabs %}

{% highlight C# %}

// Near alignment (left/top)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Near;

// Far alignment (right/bottom)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Far;

// Center alignment
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center;

// Child constraints (custom per-control)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints;

{% endhighlight %}

{% highlight VB %}

' Near alignment (left/top)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Near

' Far alignment (right/bottom)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Far

' Center alignment
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center

' Child constraints (custom per-control)
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints

{% endhighlight %}

{% endtabs %}

## Flow Direction

The ReverseRows property allows you to reverse the flow direction, which is useful for right-to-left (RTL) languages or special layout requirements.

**Use Cases**:
- RTL language support (Arabic, Hebrew)
- Creating reverse-order lists
- Special visual effects

**Code Example**:

{% tabs %}

{% highlight C# %}

// Reverse flow direction (right-to-left or bottom-to-top)
flowLayout1.ReverseRows = true;

{% endhighlight %}

{% highlight VB %}

' Reverse flow direction (right-to-left or bottom-to-top)
flowLayout1.ReverseRows = True

{% endhighlight %}

{% endtabs %}

## AutoHeight Feature

The AutoHeight property automatically adjusts the container's height when controls wrap in horizontal mode. This is particularly useful for dynamic content where the number of rows may vary.

**When to Enable**:
- Creating resizable panels with dynamic content
- Building responsive forms
- Enforcing minimum heights on containers

**Code Example**:

{% tabs %}

{% highlight C# %}

// Enable auto-height for dynamic content
flowLayout1.AutoHeight = true;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;

{% endhighlight %}

{% highlight VB %}

' Enable auto-height for dynamic content
flowLayout1.AutoHeight = True
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal

{% endhighlight %}

{% endtabs %}

## Spacing Configuration

Control the spacing between child controls using HGap (horizontal gap) and VGap (vertical gap) properties.

**Properties**:
- **HGap**: Horizontal spacing between controls (in pixels)
- **VGap**: Vertical spacing between controls (in pixels)

**Code Example**:

{% tabs %}

{% highlight C# %}

// Set spacing between controls
flowLayout1.HGap = 10;  // 10 pixels horizontal gap
flowLayout1.VGap = 10;  // 10 pixels vertical gap

{% endhighlight %}

{% highlight VB %}

' Set spacing between controls
flowLayout1.HGap = 10  ' 10 pixels horizontal gap
flowLayout1.VGap = 10  ' 10 pixels vertical gap

{% endhighlight %}

{% endtabs %}

## Adding Controls via Designer

**Step-by-Step Instructions**:

1. **Open your Windows Forms project** in Visual Studio
2. **Locate FlowLayout** in the Toolbox (under Syncfusion Controls)
3. **Drag and drop** FlowLayout onto the form
4. **Click "Yes"** in the popup to set the form as the container control
5. **Add child controls** by dragging them from the Toolbox onto the form
6. **Configure properties**:
   - Set `LayoutMode` (Horizontal or Vertical)
   - Set `Alignment` (Near, Far, Center, ChildConstraints)
   - Adjust `HGap` and `VGap` for spacing
   - Enable `AutoHeight` if needed
7. **Rearrange controls** using "Bring To Front" or "Send To Back" context menu options

## Adding Controls via Code

**Step-by-Step Instructions**:

**Step 1**: Add assembly reference:
- `Syncfusion.Shared.Base.dll`

**Step 2**: Include namespace:

{% tabs %}

{% highlight C# %}

using Syncfusion.Windows.Forms.Tools;

{% endhighlight %}

{% highlight VB %}

Imports Syncfusion.Windows.Forms.Tools

{% endhighlight %}

{% endtabs %}

**Step 3**: Create FlowLayout and add controls:

{% tabs %}

{% highlight C# %}

// Create FlowLayout instance
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = this;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
flowLayout1.HGap = 5;
flowLayout1.VGap = 5;

// Create and add child controls
ButtonAdv button1 = new ButtonAdv();
button1.Text = "New";
button1.Size = new Size(75, 30);

ButtonAdv button2 = new ButtonAdv();
button2.Text = "Open";
button2.Size = new Size(75, 30);

ButtonAdv button3 = new ButtonAdv();
button3.Text = "Save";
button3.Size = new Size(75, 30);

ButtonAdv button4 = new ButtonAdv();
button4.Text = "Print";
button4.Size = new Size(75, 30);

// Add to form (container control)
this.Controls.Add(button1);
this.Controls.Add(button2);
this.Controls.Add(button3);
this.Controls.Add(button4);

{% endhighlight %}

{% highlight VB %}

' Create FlowLayout instance
Dim flowLayout1 As New FlowLayout()
flowLayout1.ContainerControl = Me
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
flowLayout1.HGap = 5
flowLayout1.VGap = 5

' Create and add child controls
Dim button1 As New ButtonAdv()
button1.Text = "New"
button1.Size = New Size(75, 30)

Dim button2 As New ButtonAdv()
button2.Text = "Open"
button2.Size = New Size(75, 30)

Dim button3 As New ButtonAdv()
button3.Text = "Save"
button3.Size = New Size(75, 30)

Dim button4 As New ButtonAdv()
button4.Text = "Print"
button4.Size = New Size(75, 30)

' Add to form (container control)
Me.Controls.Add(button1)
Me.Controls.Add(button2)
Me.Controls.Add(button3)
Me.Controls.Add(button4)

{% endhighlight %}

{% endtabs %}

## Per-Child Constraints

When the Alignment property is set to `ChildConstraints`, you can specify individual layout constraints for each child control using the `FlowLayoutConstraints` class.

**FlowLayoutConstraints Constructor**:
```
FlowLayoutConstraints(bool active, HorzFlowAlign hAlign, VertFlowAlign vAlign, 
                      bool newLine, bool proportionalColWidth, bool proportionalRowHeight)
```

### HAlign and VAlign

Control the alignment of individual controls within rows or columns.

**HAlign Options** (Horizontal Mode):
- **Left**: Align to left of row
- **Right**: Align to right of row
- **Center**: Center in row
- **Justify**: Fill available horizontal space

**VAlign Options** (Vertical Mode):
- **Top**: Align to top of column
- **Bottom**: Align to bottom of column
- **Center**: Center in column
- **Justify**: Fill available vertical space

**Code Example**:

{% tabs %}

{% highlight C# %}

// Set alignment to use child constraints
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints;

// Create textbox with justified alignment
TextBox textBox1 = new TextBox();
flowLayout1.SetConstraints(textBox1, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true,  // active
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Justify,  // horizontal justify
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,   // vertical center
        false, // not new line
        false, // no proportional column width
        false  // no proportional row height
    ));
flowLayout1.SetPreferredSize(textBox1, new Size(100, 20));
this.Controls.Add(textBox1);

{% endhighlight %}

{% highlight VB %}

' Set alignment to use child constraints
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints

' Create textbox with justified alignment
Dim textBox1 As New TextBox()
flowLayout1.SetConstraints(textBox1, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True,  ' active
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Justify,  ' horizontal justify
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,   ' vertical center
        False, ' not new line
        False, ' no proportional column width
        False  ' no proportional row height
    ))
flowLayout1.SetPreferredSize(textBox1, New Size(100, 20))
Me.Controls.Add(textBox1)

{% endhighlight %}

{% endtabs %}

### Layout Participation

Control whether a child participates in the layout using the `Active` parameter or `SetParticipateInLayout` method.

{% tabs %}

{% highlight C# %}

// Exclude control from layout
flowLayout1.SetParticipateInLayout(button1, false);

// Include control in layout
flowLayout1.SetParticipateInLayout(button2, true);

{% endhighlight %}

{% highlight VB %}

' Exclude control from layout
flowLayout1.SetParticipateInLayout(button1, False)

' Include control in layout
flowLayout1.SetParticipateInLayout(button2, True)

{% endhighlight %}

{% endtabs %}

### Line Beginner

Force a control to always start on a new row (horizontal mode) or column (vertical mode) using the `NewLine` parameter.

{% tabs %}

{% highlight C# %}

// Force button to start on new line
flowLayout1.SetConstraints(button4, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true,   // active
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Left,
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,
        true,   // new line - forces new row
        false,
        false
    ));

{% endhighlight %}

{% highlight VB %}

' Force button to start on new line
flowLayout1.SetConstraints(button4, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True,   ' active
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Left, _
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center, _
        True,   ' new line - forces new row
        False, _
        False _
    ))

{% endhighlight %}

{% endtabs %}

### Row Height and Column Width

Use proportional row heights (horizontal mode) or column widths (vertical mode) to distribute extra space.

**ProportionalRowHeight**: When enabled, extra vertical space is distributed among rows
**ProportionalColWidth**: When enabled, extra horizontal space is distributed among columns

{% tabs %}

{% highlight C# %}

// Enable proportional row height for centering
flowLayout1.SetConstraints(textBox1, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true,
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Center,
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,
        false,
        false,
        true   // proportional row height
    ));

{% endhighlight %}

{% highlight VB %}

' Enable proportional row height for centering
flowLayout1.SetConstraints(textBox1, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True, _
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Center, _
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center, _
        False, _
        False, _
        True   ' proportional row height
    ))

{% endhighlight %}

{% endtabs %}

## Wrapping Behavior

FlowLayout automatically wraps controls to new rows (horizontal mode) or columns (vertical mode) when the container boundary is reached.

**Horizontal Mode Wrapping**:
- Controls flow left-to-right
- When reaching the right edge, a new row is created below
- Continue flowing left-to-right in the new row

**Vertical Mode Wrapping**:
- Controls flow top-to-bottom
- When reaching the bottom edge, a new column is created to the right
- Continue flowing top-to-bottom in the new column

**Factors Affecting Wrapping**:
1. Container width (horizontal mode) or height (vertical mode)
2. Control sizes (width/height)
3. HGap and VGap spacing
4. Control margins

**Code Example - Demonstrating Wrapping**:

{% tabs %}

{% highlight C# %}

// Create panel with limited width to demonstrate wrapping
Panel panel1 = new Panel();
panel1.Size = new Size(300, 200);
panel1.BorderStyle = BorderStyle.FixedSingle;
this.Controls.Add(panel1);

// Create FlowLayout for the panel
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = panel1;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
flowLayout1.HGap = 5;
flowLayout1.VGap = 5;

// Add multiple buttons - will wrap automatically
for (int i = 1; i <= 12; i++)
{
    ButtonAdv button = new ButtonAdv();
    button.Text = "Btn " + i;
    button.Size = new Size(70, 30);
    panel1.Controls.Add(button);
}
// Buttons will automatically wrap to multiple rows

{% endhighlight %}

{% highlight VB %}

' Create panel with limited width to demonstrate wrapping
Dim panel1 As New Panel()
panel1.Size = New Size(300, 200)
panel1.BorderStyle = BorderStyle.FixedSingle
Me.Controls.Add(panel1)

' Create FlowLayout for the panel
Dim flowLayout1 As New FlowLayout()
flowLayout1.ContainerControl = panel1
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
flowLayout1.HGap = 5
flowLayout1.VGap = 5

' Add multiple buttons - will wrap automatically
For i As Integer = 1 To 12
    Dim button As New ButtonAdv()
    button.Text = "Btn " & i
    button.Size = New Size(70, 30)
    panel1.Controls.Add(button)
Next
' Buttons will automatically wrap to multiple rows

{% endhighlight %}

{% endtabs %}

## Complete Examples

### Horizontal Toolbar with Wrapping

Create a toolbar that automatically wraps buttons to multiple rows when the window is resized.

{% tabs %}

{% highlight C# %}

using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ToolbarForm : Form
{
    private FlowLayout flowLayout1;
    private Panel toolbarPanel;

    public ToolbarForm()
    {
        InitializeComponent();
    }

    private void InitializeComponent()
    {
        // Create toolbar panel
        toolbarPanel = new Panel();
        toolbarPanel.Dock = DockStyle.Top;
        toolbarPanel.Height = 100;
        toolbarPanel.BackColor = Color.LightGray;
        this.Controls.Add(toolbarPanel);

        // Create FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = toolbarPanel;
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
        flowLayout1.AutoHeight = true;
        flowLayout1.HGap = 3;
        flowLayout1.VGap = 3;

        // Add toolbar buttons
        string[] buttonNames = { "New", "Open", "Save", "Print", "Cut", 
                                 "Copy", "Paste", "Undo", "Redo", "Find" };
        
        foreach (string name in buttonNames)
        {
            ButtonAdv button = new ButtonAdv();
            button.Text = name;
            button.Size = new Size(60, 30);
            toolbarPanel.Controls.Add(button);
        }

        // Form settings
        this.Text = "Toolbar with Auto-Wrap";
        this.Size = new Size(600, 400);
    }
}

{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class ToolbarForm
    Inherits Form

    Private flowLayout1 As FlowLayout
    Private toolbarPanel As Panel

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Create toolbar panel
        toolbarPanel = New Panel()
        toolbarPanel.Dock = DockStyle.Top
        toolbarPanel.Height = 100
        toolbarPanel.BackColor = Color.LightGray
        Me.Controls.Add(toolbarPanel)

        ' Create FlowLayout
        flowLayout1 = New FlowLayout()
        flowLayout1.ContainerControl = toolbarPanel
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
        flowLayout1.AutoHeight = True
        flowLayout1.HGap = 3
        flowLayout1.VGap = 3

        ' Add toolbar buttons
        Dim buttonNames() As String = {"New", "Open", "Save", "Print", "Cut", _
                                       "Copy", "Paste", "Undo", "Redo", "Find"}

        For Each name As String In buttonNames
            Dim button As New ButtonAdv()
            button.Text = name
            button.Size = New Size(60, 30)
            toolbarPanel.Controls.Add(button)
        Next

        ' Form settings
        Me.Text = "Toolbar with Auto-Wrap"
        Me.Size = New Size(600, 400)
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

### Vertical Sidebar Navigation

Create a vertical navigation menu using FlowLayout.

{% tabs %}

{% highlight C# %}

using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class SidebarForm : Form
{
    private FlowLayout flowLayout1;
    private Panel sidebarPanel;

    public SidebarForm()
    {
        InitializeComponent();
    }

    private void InitializeComponent()
    {
        // Create sidebar panel
        sidebarPanel = new Panel();
        sidebarPanel.Dock = DockStyle.Left;
        sidebarPanel.Width = 150;
        sidebarPanel.BackColor = Color.FromArgb(45, 45, 48);
        this.Controls.Add(sidebarPanel);

        // Create FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = sidebarPanel;
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Vertical;
        flowLayout1.VGap = 2;

        // Add navigation buttons
        string[] menuItems = { "Dashboard", "Projects", "Tasks", 
                              "Calendar", "Reports", "Settings" };

        foreach (string item in menuItems)
        {
            ButtonAdv button = new ButtonAdv();
            button.Text = item;
            button.Size = new Size(140, 40);
            button.ForeColor = Color.White;
            button.BackColor = Color.FromArgb(45, 45, 48);
            button.FlatStyle = FlatStyle.Flat;
            sidebarPanel.Controls.Add(button);
        }

        // Form settings
        this.Text = "Vertical Sidebar Navigation";
        this.Size = new Size(800, 600);
    }
}

{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class SidebarForm
    Inherits Form

    Private flowLayout1 As FlowLayout
    Private sidebarPanel As Panel

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Create sidebar panel
        sidebarPanel = New Panel()
        sidebarPanel.Dock = DockStyle.Left
        sidebarPanel.Width = 150
        sidebarPanel.BackColor = Color.FromArgb(45, 45, 48)
        Me.Controls.Add(sidebarPanel)

        ' Create FlowLayout
        flowLayout1 = New FlowLayout()
        flowLayout1.ContainerControl = sidebarPanel
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Vertical
        flowLayout1.VGap = 2

        ' Add navigation buttons
        Dim menuItems() As String = {"Dashboard", "Projects", "Tasks", _
                                     "Calendar", "Reports", "Settings"}

        For Each item As String In menuItems
            Dim button As New ButtonAdv()
            button.Text = item
            button.Size = New Size(140, 40)
            button.ForeColor = Color.White
            button.BackColor = Color.FromArgb(45, 45, 48)
            button.FlatStyle = FlatStyle.Flat
            sidebarPanel.Controls.Add(button)
        Next

        ' Form settings
        Me.Text = "Vertical Sidebar Navigation"
        Me.Size = New Size(800, 600)
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

### Tag Cloud (Dynamic Labels)

Create a tag cloud with dynamic label addition and removal.

{% tabs %}

{% highlight C# %}

using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class TagCloudForm : Form
{
    private FlowLayout flowLayout1;
    private Panel tagPanel;
    private TextBox tagInput;
    private ButtonAdv addButton;

    public TagCloudForm()
    {
        InitializeComponent();
    }

    private void InitializeComponent()
    {
        // Input area
        Panel inputPanel = new Panel();
        inputPanel.Dock = DockStyle.Top;
        inputPanel.Height = 40;
        this.Controls.Add(inputPanel);

        tagInput = new TextBox();
        tagInput.Location = new Point(10, 10);
        tagInput.Width = 200;
        inputPanel.Controls.Add(tagInput);

        addButton = new ButtonAdv();
        addButton.Text = "Add Tag";
        addButton.Location = new Point(220, 8);
        addButton.Click += AddButton_Click;
        inputPanel.Controls.Add(addButton);

        // Tag display panel
        tagPanel = new Panel();
        tagPanel.Dock = DockStyle.Fill;
        tagPanel.BackColor = Color.White;
        tagPanel.AutoScroll = true;
        this.Controls.Add(tagPanel);

        // Create FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = tagPanel;
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
        flowLayout1.HGap = 5;
        flowLayout1.VGap = 5;

        // Add some initial tags
        AddTag("C#");
        AddTag("WinForms");
        AddTag("Syncfusion");
        AddTag("Layout");

        // Form settings
        this.Text = "Tag Cloud Example";
        this.Size = new Size(600, 400);
    }

    private void AddButton_Click(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(tagInput.Text))
        {
            AddTag(tagInput.Text);
            tagInput.Clear();
        }
    }

    private void AddTag(string tagText)
    {
        Label tagLabel = new Label();
        tagLabel.Text = tagText + " ×";
        tagLabel.AutoSize = true;
        tagLabel.Padding = new Padding(8, 4, 8, 4);
        tagLabel.BackColor = Color.LightBlue;
        tagLabel.Cursor = Cursors.Hand;
        tagLabel.Click += (s, e) => RemoveTag(tagLabel);
        tagPanel.Controls.Add(tagLabel);
    }

    private void RemoveTag(Label tagLabel)
    {
        tagPanel.Controls.Remove(tagLabel);
        tagPanel.PerformLayout();
    }
}

{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class TagCloudForm
    Inherits Form

    Private flowLayout1 As FlowLayout
    Private tagPanel As Panel
    Private tagInput As TextBox
    Private addButton As ButtonAdv

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Input area
        Dim inputPanel As New Panel()
        inputPanel.Dock = DockStyle.Top
        inputPanel.Height = 40
        Me.Controls.Add(inputPanel)

        tagInput = New TextBox()
        tagInput.Location = New Point(10, 10)
        tagInput.Width = 200
        inputPanel.Controls.Add(tagInput)

        addButton = New ButtonAdv()
        addButton.Text = "Add Tag"
        addButton.Location = New Point(220, 8)
        AddHandler addButton.Click, AddressOf AddButton_Click
        inputPanel.Controls.Add(addButton)

        ' Tag display panel
        tagPanel = New Panel()
        tagPanel.Dock = DockStyle.Fill
        tagPanel.BackColor = Color.White
        tagPanel.AutoScroll = True
        Me.Controls.Add(tagPanel)

        ' Create FlowLayout
        flowLayout1 = New FlowLayout()
        flowLayout1.ContainerControl = tagPanel
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
        flowLayout1.HGap = 5
        flowLayout1.VGap = 5

        ' Add some initial tags
        AddTag("C#")
        AddTag("WinForms")
        AddTag("Syncfusion")
        AddTag("Layout")

        ' Form settings
        Me.Text = "Tag Cloud Example"
        Me.Size = New Size(600, 400)
    End Sub

    Private Sub AddButton_Click(sender As Object, e As EventArgs)
        If Not String.IsNullOrWhiteSpace(tagInput.Text) Then
            AddTag(tagInput.Text)
            tagInput.Clear()
        End If
    End Sub

    Private Sub AddTag(tagText As String)
        Dim tagLabel As New Label()
        tagLabel.Text = tagText & " ×"
        tagLabel.AutoSize = True
        tagLabel.Padding = New Padding(8, 4, 8, 4)
        tagLabel.BackColor = Color.LightBlue
        tagLabel.Cursor = Cursors.Hand
        AddHandler tagLabel.Click, Sub(s, e) RemoveTag(tagLabel)
        tagPanel.Controls.Add(tagLabel)
    End Sub

    Private Sub RemoveTag(tagLabel As Label)
        tagPanel.Controls.Remove(tagLabel)
        tagPanel.PerformLayout()
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

### Responsive Button Panel

Create a button panel that adapts to window resizing.

{% tabs %}

{% highlight C# %}

using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ResponsiveButtonForm : Form
{
    private FlowLayout flowLayout1;
    private Panel buttonPanel;

    public ResponsiveButtonForm()
    {
        InitializeComponent();
    }

    private void InitializeComponent()
    {
        // Create button panel
        buttonPanel = new Panel();
        buttonPanel.Dock = DockStyle.Fill;
        buttonPanel.Padding = new Padding(10);
        this.Controls.Add(buttonPanel);

        // Create FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = buttonPanel;
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
        flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center;
        flowLayout1.HGap = 10;
        flowLayout1.VGap = 10;

        // Add category buttons
        string[,] categories = {
            {"Music", "🎵"}, {"Videos", "🎬"}, {"Photos", "📷"},
            {"Documents", "📄"}, {"Downloads", "⬇"}, {"Settings", "⚙"}
        };

        for (int i = 0; i < categories.GetLength(0); i++)
        {
            ButtonAdv button = new ButtonAdv();
            button.Text = categories[i, 1] + "\n" + categories[i, 0];
            button.Size = new Size(100, 80);
            button.Font = new Font("Segoe UI", 10, FontStyle.Bold);
            buttonPanel.Controls.Add(button);
        }

        // Form settings
        this.Text = "Responsive Button Panel";
        this.Size = new Size(500, 400);
        this.MinimumSize = new Size(300, 200);
    }
}

{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class ResponsiveButtonForm
    Inherits Form

    Private flowLayout1 As FlowLayout
    Private buttonPanel As Panel

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Create button panel
        buttonPanel = New Panel()
        buttonPanel.Dock = DockStyle.Fill
        buttonPanel.Padding = New Padding(10)
        Me.Controls.Add(buttonPanel)

        ' Create FlowLayout
        flowLayout1 = New FlowLayout()
        flowLayout1.ContainerControl = buttonPanel
        flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
        flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center
        flowLayout1.HGap = 10
        flowLayout1.VGap = 10

        ' Add category buttons
        Dim categories(,) As String = {
            {"Music", "🎵"}, {"Videos", "🎬"}, {"Photos", "📷"},
            {"Documents", "📄"}, {"Downloads", "⬇"}, {"Settings", "⚙"}
        }

        For i As Integer = 0 To categories.GetLength(0) - 1
            Dim button As New ButtonAdv()
            button.Text = categories(i, 1) & vbLf & categories(i, 0)
            button.Size = New Size(100, 80)
            button.Font = New Font("Segoe UI", 10, FontStyle.Bold)
            buttonPanel.Controls.Add(button)
        Next

        ' Form settings
        Me.Text = "Responsive Button Panel"
        Me.Size = New Size(500, 400)
        Me.MinimumSize = New Size(300, 200)
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

## Advanced Constraint Scenarios

Examples of advanced constraint usage for complex layouts.

{% tabs %}

{% highlight C# %}

// Set up FlowLayout with child constraints
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = this;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints;

// Control 1: Left-aligned with minimum size
TextBox textBox1 = new TextBox();
flowLayout1.SetConstraints(textBox1, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true, HorzFlowAlign.Left, VertFlowAlign.Center, false, false, false));
flowLayout1.SetMinSize(textBox1, new Size(50, 20));
this.Controls.Add(textBox1);

// Control 2: Justified (fills available space)
TextBox textBox2 = new TextBox();
flowLayout1.SetConstraints(textBox2, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true, HorzFlowAlign.Justify, VertFlowAlign.Center, false, false, false));
flowLayout1.SetPreferredSize(textBox2, new Size(150, 20));
this.Controls.Add(textBox2);

// Control 3: Start on new line
ButtonAdv button1 = new ButtonAdv();
button1.Text = "Submit";
flowLayout1.SetConstraints(button1, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true, HorzFlowAlign.Right, VertFlowAlign.Center, true, false, false));
this.Controls.Add(button1);

{% endhighlight %}

{% highlight VB %}

' Set up FlowLayout with child constraints
Dim flowLayout1 As New FlowLayout()
flowLayout1.ContainerControl = Me
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints

' Control 1: Left-aligned with minimum size
Dim textBox1 As New TextBox()
flowLayout1.SetConstraints(textBox1, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True, HorzFlowAlign.Left, VertFlowAlign.Center, False, False, False))
flowLayout1.SetMinSize(textBox1, New Size(50, 20))
Me.Controls.Add(textBox1)

' Control 2: Justified (fills available space)
Dim textBox2 As New TextBox()
flowLayout1.SetConstraints(textBox2, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True, HorzFlowAlign.Justify, VertFlowAlign.Center, False, False, False))
flowLayout1.SetPreferredSize(textBox2, New Size(150, 20))
Me.Controls.Add(textBox2)

' Control 3: Start on new line
Dim button1 As New ButtonAdv()
button1.Text = "Submit"
flowLayout1.SetConstraints(button1, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True, HorzFlowAlign.Right, VertFlowAlign.Center, True, False, False))
Me.Controls.Add(button1)

{% endhighlight %}

{% endtabs %}

## Centering Child Controls

To center controls both horizontally and vertically, use the following approach:

{% tabs %}

{% highlight C# %}

// Set up FlowLayout for centering
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = this;
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal;
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints;

// Add controls with centered constraints
TextBox textBox1 = new TextBox();
flowLayout1.SetConstraints(textBox1, 
    new Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints(
        true,
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Center,  // horizontal center
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,  // vertical center
        false,
        false,
        true   // proportional row height for true centering
    ));
this.Controls.Add(textBox1);

{% endhighlight %}

{% highlight VB %}

' Set up FlowLayout for centering
Dim flowLayout1 As New FlowLayout()
flowLayout1.ContainerControl = Me
flowLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.FlowLayoutMode.Horizontal
flowLayout1.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.ChildConstraints

' Add controls with centered constraints
Dim textBox1 As New TextBox()
flowLayout1.SetConstraints(textBox1, _
    New Syncfusion.Windows.Forms.Tools.FlowLayoutConstraints( _
        True, _
        Syncfusion.Windows.Forms.Tools.HorzFlowAlign.Center,  ' horizontal center
        Syncfusion.Windows.Forms.Tools.VertFlowAlign.Center,  ' vertical center
        False, _
        False, _
        True   ' proportional row height for true centering
    ))
Me.Controls.Add(textBox1)

{% endhighlight %}

{% endtabs %}

## Dynamic Control Management

FlowLayout automatically handles dynamic addition and removal of controls.

{% tabs %}

{% highlight C# %}

// Adding controls at runtime
private void AddControl()
{
    ButtonAdv newButton = new ButtonAdv();
    newButton.Text = "New Button";
    newButton.Size = new Size(80, 30);
    
    // Simply add to container - FlowLayout handles layout automatically
    containerPanel.Controls.Add(newButton);
}

// Removing controls at runtime
private void RemoveControl(Control control)
{
    containerPanel.Controls.Remove(control);
    containerPanel.PerformLayout();  // Force layout update
}

// Reordering controls
private void MoveControlToFront(Control control)
{
    containerPanel.Controls.SetChildIndex(control, 0);
    containerPanel.PerformLayout();
}

{% endhighlight %}

{% highlight VB %}

' Adding controls at runtime
Private Sub AddControl()
    Dim newButton As New ButtonAdv()
    newButton.Text = "New Button"
    newButton.Size = New Size(80, 30)
    
    ' Simply add to container - FlowLayout handles layout automatically
    containerPanel.Controls.Add(newButton)
End Sub

' Removing controls at runtime
Private Sub RemoveControl(control As Control)
    containerPanel.Controls.Remove(control)
    containerPanel.PerformLayout()  ' Force layout update
End Sub

' Reordering controls
Private Sub MoveControlToFront(control As Control)
    containerPanel.Controls.SetChildIndex(control, 0)
    containerPanel.PerformLayout()
End Sub

{% endhighlight %}

{% endtabs %}

## Common Patterns

**Toolbar with Auto-Wrap**:
- Horizontal FlowLayout
- AutoHeight = true
- HGap and VGap for spacing
- Fixed-size buttons

**Tag List or Chip List**:
- Horizontal FlowLayout
- Center alignment
- Small VGap and HGap
- AutoSize labels

**Icon Panel**:
- Horizontal or Vertical FlowLayout
- Equal-sized icon buttons
- Regular spacing
- Optional wrapping

**Navigation Menu**:
- Vertical FlowLayout
- Full-width buttons
- Small VGap
- Left alignment

**Search Results List**:
- Vertical FlowLayout
- Variable-height items
- VGap for separation
- Top alignment

## Best Practices

1. **Use FlowLayout as Default**: FlowLayout is the most versatile layout manager - use it as your first choice for general layouts

2. **Set Appropriate Spacing**: Always set HGap and VGap for visual separation between controls (typically 5-10 pixels)

3. **Use ChildConstraints for Complex Layouts**: When you need different alignment per control, use `FlowAlignment.ChildConstraints`

4. **Test Wrapping Behavior**: Always test your layout with different window sizes to ensure wrapping works as expected

5. **Use AutoHeight for Dynamic Content**: Enable AutoHeight when the number of rows may vary

6. **Consider Container's AutoScroll**: For content that may exceed container bounds, enable AutoScroll on the container panel

7. **Set PreferredSize for Justified Controls**: When using `HorzFlowAlign.Justify`, always specify PreferredSize

8. **Use SetParticipateInLayout Carefully**: Only exclude controls from layout when necessary

9. **Leverage NewLine for Sections**: Use NewLine constraint to create logical sections in your layout

10. **Test Resize Performance**: With many controls, test resize performance and consider optimizing

## Troubleshooting

**Controls Not Wrapping**:
- Check container size - ensure there's a boundary to wrap at
- Verify AutoHeight is enabled for horizontal mode
- Check control sizes - very large controls may not wrap properly

**Unexpected Alignment**:
- Verify Alignment property is set correctly
- For ChildConstraints, check each control's constraint settings
- Ensure HAlign/VAlign are set appropriately

**Spacing Issues**:
- Adjust HGap and VGap properties
- Check control margins and padding
- Verify container padding is not interfering

**Controls Overlapping**:
- Check MinSize constraints are appropriate
- Verify PreferredSize is set for justified controls
- Ensure controls are participating in layout

**AutoHeight Not Working**:
- Verify LayoutMode is set to Horizontal (AutoHeight only works in horizontal mode)
- Check that container can resize vertically
- Ensure AutoHeight property is set to true

**Poor Resize Performance**:
- Reduce number of controls
- Use SuspendLayout/ResumeLayout during bulk additions
- Consider virtualization for very large lists
