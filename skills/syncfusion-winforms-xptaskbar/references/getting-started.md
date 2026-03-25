# Getting Started with XPTaskBar

## Table of Contents
- [Assembly References](#assembly-references)
- [Adding XPTaskBar via Designer](#adding-xptaskbar-via-designer)
- [Creating XPTaskBar Programmatically](#creating-xptaskbar-programmatically)
- [Adding XPTaskBarBox](#adding-xptaskbarbox)
- [Adding XPTaskBarItems](#adding-xptaskbaritems)

## Assembly References

To use XPTaskBar in your Windows Forms application, add the following assembly references to your project:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

If using NuGet, install the appropriate Syncfusion Windows Forms package for your framework version. Refer to the [Syncfusion NuGet installation guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages) for detailed steps.

## Adding XPTaskBar via Designer

The easiest way to add XPTaskBar to your form is through the Visual Studio designer:

1. **Open the Toolbox** in Visual Studio
2. **Locate XPTaskBar** in the Syncfusion Windows Forms components section
3. **Drag and drop** the control onto your form
4. The required assembly references are automatically added to your project

### Adding XPTaskBarBox from Designer

Once XPTaskBar is on your form:

1. **Select the XPTaskBar control** on the designer
2. **Open Smart Tag** by clicking the small arrow in the top-right corner
3. **Click "Add TaskBarBox"** to create a new collapsible box
4. Repeat to add multiple boxes as needed

### Adding XPTaskBarItems from Designer

To add items to a box:

1. **Select the XPTaskBarBox** in the designer
2. **Open Smart Tag**
3. **Click "Edit Items"** to open the XPTaskBarItem Collection Editor
4. **Click "Add"** to create new items
5. Set properties like `Text`, `Tag`, and `ToolTipText` for each item

## Creating XPTaskBar Programmatically

To create XPTaskBar in code, first add the required namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

Then create an instance and add it to your form:

```csharp
XPTaskBar xpTaskBar1 = new XPTaskBar();
xpTaskBar1.Dock = DockStyle.Left;
this.Controls.Add(xpTaskBar1);
```

**VB.NET:**

```vb
Dim xpTaskBar1 As New XPTaskBar()
xpTaskBar1.Dock = DockStyle.Left
Me.Controls.Add(xpTaskBar1)
```

## Adding XPTaskBarBox

Create XPTaskBarBox instances and add them to the XPTaskBar's controls collection:

```csharp
// Create boxes
XPTaskBarBox box1 = new XPTaskBarBox();
XPTaskBarBox box2 = new XPTaskBarBox();
XPTaskBarBox box3 = new XPTaskBarBox();

// Set box names (displayed as headers)
box1.Text = "File Operations";
box2.Text = "Editing Tools";
box3.Text = "View Settings";

// Add boxes to the XPTaskBar
xpTaskBar1.Controls.Add(box1);
xpTaskBar1.Controls.Add(box2);
xpTaskBar1.Controls.Add(box3);
```

**VB.NET:**

```vb
Dim box1 As New XPTaskBarBox()
Dim box2 As New XPTaskBarBox()
Dim box3 As New XPTaskBarBox()

box1.Text = "File Operations"
box2.Text = "Editing Tools"
box3.Text = "View Settings"

xpTaskBar1.Controls.Add(box1)
xpTaskBar1.Controls.Add(box2)
xpTaskBar1.Controls.Add(box3)
```

## Adding XPTaskBarItems

Items are added to a box's Items collection. Each item represents a clickable command:

```csharp
// Add items to box1
xpTaskBar1.Controls[0].Controls.AddRange(new Syncfusion.Windows.Forms.Tools.XPTaskBarItem[] {
    new XPTaskBarItem("New Document", System.Drawing.Color.Empty, -1, "NewDoc"),
    new XPTaskBarItem("Open File", System.Drawing.Color.Empty, -1, "OpenFile"),
    new XPTaskBarItem("Save Document", System.Drawing.Color.Empty, -1, "Save")
});

// Or access the box directly
XPTaskBarBox box1 = xpTaskBar1.Controls[0] as XPTaskBarBox;
box1.Items.AddRange(new Syncfusion.Windows.Forms.Tools.XPTaskBarItem[] {
    new XPTaskBarItem("Cut", System.Drawing.Color.Empty, -1, "Cut"),
    new XPTaskBarItem("Copy", System.Drawing.Color.Empty, -1, "Copy"),
    new XPTaskBarItem("Paste", System.Drawing.Color.Empty, -1, "Paste")
});
```

**VB.NET:**

```vb
xpTaskBar1.Controls(0).Controls.AddRange(New Syncfusion.Windows.Forms.Tools.XPTaskBarItem() {
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("New Document", System.Drawing.Color.Empty, -1, "NewDoc"),
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("Open File", System.Drawing.Color.Empty, -1, "OpenFile"),
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("Save Document", System.Drawing.Color.Empty, -1, "Save")})

' Or access the box directly
Dim box1 As XPTaskBarBox = CType(xpTaskBar1.Controls(0), XPTaskBarBox)
box1.Items.AddRange(New Syncfusion.Windows.Forms.Tools.XPTaskBarItem() {
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("Cut", System.Drawing.Color.Empty, -1, "Cut"),
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("Copy", System.Drawing.Color.Empty, -1, "Copy"),
    New Syncfusion.Windows.Forms.Tools.XPTaskBarItem("Paste", System.Drawing.Color.Empty, -1, "Paste")})
```

### XPTaskBarItem Constructor Parameters

The XPTaskBarItem constructor takes four parameters:

1. **Text** (string) - The display text for the item
2. **ForeColor** (Color) - Text color (use `System.Drawing.Color.Empty` for default)
3. **ImageIndex** (int) - Index in the parent box's ImageList (-1 for no image)
4. **Tag** (object) - Custom data for event handling (typically a string identifier)

Example with detailed parameters:

```csharp
var item = new XPTaskBarItem(
    text: "Save Document",
    foreColor: System.Drawing.Color.Empty,
    imageIndex: 0,  // First image in ImageList
    tag: "Save"     // Used in ItemClick event routing
);
```

## Next Steps

Once your XPTaskBar structure is set up:
- See [box-structure.md](box-structure.md) to customize box headers and behavior
- See [items-and-content.md](items-and-content.md) to work with items and child controls
- See [behavior-and-events.md](behavior-and-events.md) to handle user interactions
