# Getting Started with SfScrollFrame

This guide covers the basic setup, installation, and usage of the SfScrollFrame control in Windows Forms applications.

## Assembly Deployment

The SfScrollFrame component requires the following assembly reference:

### Required Assembly

**Assembly:** `Syncfusion.Core.WinForms`

**Description:** Contains theme-related classes for Syncfusion controls and basic components including SfScrollFrame, SfToolTip, SfButton, SfForm, and SfSkinManager.

### Installation Methods

**Option 1: NuGet Package Manager**

```powershell
Install-Package Syncfusion.Core.WinForms
```

**Option 2: Package Manager UI**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Core.WinForms"
3. Click Install

**Option 3: Control Panel**
- Use Syncfusion Control Panel to install the complete Windows Forms suite

### Verify Installation

Check that the assembly is referenced in your project:
- Solution Explorer → References
- Confirm `Syncfusion.Core.WinForms` appears in the list

For complete control dependencies, see [Syncfusion Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#sfscrollframe).

## Attaching SfScrollFrame to a Control

SfScrollFrame can be attached to any control derived from `ScrollableControl` or `Container`. This section demonstrates attaching to a ListView control.

### Through Designer

The SfScrollFrame can be attached through the Visual Studio designer.

#### Steps

1. **Open Form in Designer**
   - Open your Windows Form in Design view

2. **Add SfScrollFrame from Toolbox**
   - Open Toolbox (View → Toolbox or Ctrl+Alt+X)
   - Locate "SfScrollFrame" under Syncfusion Controls
   - Drag and drop onto the form

3. **Add Target Control**
   - Add a ListView (or other ScrollableControl) to the form
   - Configure the ListView properties as needed

4. **Link SfScrollFrame to Control**
   - Select the SfScrollFrame in the designer
   - In Properties window, find the `Control` property
   - Click dropdown and select the target control (e.g., listView1)

#### Designer-Generated Code

When attached through the designer, Visual Studio generates code similar to:

```csharp
// Member variables
private Syncfusion.WinForms.Controls.SfScrollFrame sfScrollFrame1;
private System.Windows.Forms.ListView listView1;
private System.Windows.Forms.ColumnHeader OrderID;
private System.Windows.Forms.ColumnHeader CustomerID;
private System.Windows.Forms.ColumnHeader columnHeader1;
private System.Windows.Forms.ColumnHeader Quantity;

// In InitializeComponent()
this.sfScrollFrame1 = new Syncfusion.WinForms.Controls.SfScrollFrame();
this.listView1 = new System.Windows.Forms.ListView();
this.OrderID = new System.Windows.Forms.ColumnHeader();
this.CustomerID = new System.Windows.Forms.ColumnHeader();
this.columnHeader1 = new System.Windows.Forms.ColumnHeader();
this.Quantity = new System.Windows.Forms.ColumnHeader();
this.SuspendLayout();

// ListView configuration
this.listView1.Columns.AddRange(new System.Windows.Forms.ColumnHeader[] {
    this.OrderID,
    this.CustomerID,
    this.columnHeader1,
    this.Quantity
});
this.listView1.Location = new System.Drawing.Point(30, 29);
this.listView1.Name = "listView1";
this.listView1.Size = new System.Drawing.Size(379, 285);
this.listView1.TabIndex = 6;
this.listView1.UseCompatibleStateImageBehavior = false;
this.listView1.View = System.Windows.Forms.View.Details;

// Attach SfScrollFrame to ListView
this.sfScrollFrame1.Control = this.listView1;
```

### Through Code

Programmatically attach SfScrollFrame by setting the `Control` property.

#### Basic Code Implementation

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

public partial class MainForm : Form
{
    private SfScrollFrame sfScrollFrame1;
    private ListView listView1;

    public MainForm()
    {
        InitializeComponent();
        
        // Create ListView
        listView1 = new ListView();
        listView1.View = View.Details;
        listView1.Location = new System.Drawing.Point(30, 29);
        listView1.Size = new System.Drawing.Size(379, 285);
        
        // Add columns
        listView1.Columns.Add("OrderID", 100);
        listView1.Columns.Add("CustomerID", 100);
        listView1.Columns.Add("Product", 100);
        listView1.Columns.Add("Quantity", 79);
        
        // Add items
        for (int i = 1; i <= 100; i++)
        {
            ListViewItem item = new ListViewItem(i.ToString());
            item.SubItems.Add($"CUST{i:000}");
            item.SubItems.Add($"Product {i}");
            item.SubItems.Add((i % 50 + 1).ToString());
            listView1.Items.Add(item);
        }
        
        // Create SfScrollFrame
        sfScrollFrame1 = new SfScrollFrame();
        
        // Attach SfScrollFrame to ListView
        sfScrollFrame1.Control = listView1;
        
        // Add ListView to form
        this.Controls.Add(listView1);
    }
}
```

#### Simplified Attachment

```csharp
// Minimal code to attach SfScrollFrame
this.sfScrollFrame1.Control = listView1;
```

The SfScrollFrame automatically detects and replaces the default scrollbars of the attached control.

## Programmatic Scrolling

Control the scroll position programmatically using the `Value` property of the corresponding scrollbar.

### Scroll to Specific Position

```csharp
// Scroll vertical scrollbar to position 100
this.sfScrollFrame1.VerticalScrollBar.Value = 100;

// Scroll horizontal scrollbar to position 100
this.sfScrollFrame1.HorizontalScrollBar.Value = 100;
```

### Scroll to Top/Bottom

```csharp
// Scroll to top
this.sfScrollFrame1.VerticalScrollBar.Value = this.sfScrollFrame1.VerticalScrollBar.Minimum;

// Scroll to bottom
this.sfScrollFrame1.VerticalScrollBar.Value = this.sfScrollFrame1.VerticalScrollBar.Maximum;

// Scroll to left
this.sfScrollFrame1.HorizontalScrollBar.Value = this.sfScrollFrame1.HorizontalScrollBar.Minimum;

// Scroll to right
this.sfScrollFrame1.HorizontalScrollBar.Value = this.sfScrollFrame1.HorizontalScrollBar.Maximum;
```

### Scroll Incrementally

```csharp
// Scroll down by 50 pixels
int currentValue = this.sfScrollFrame1.VerticalScrollBar.Value;
int maxValue = this.sfScrollFrame1.VerticalScrollBar.Maximum;
int newValue = Math.Min(currentValue + 50, maxValue);
this.sfScrollFrame1.VerticalScrollBar.Value = newValue;

// Scroll up by 50 pixels
currentValue = this.sfScrollFrame1.VerticalScrollBar.Value;
int minValue = this.sfScrollFrame1.VerticalScrollBar.Minimum;
newValue = Math.Max(currentValue - 50, minValue);
this.sfScrollFrame1.VerticalScrollBar.Value = newValue;
```

### Example: Scroll on Button Click

```csharp
// Add buttons to control scrolling
Button scrollTopButton = new Button();
scrollTopButton.Text = "Scroll to Top";
scrollTopButton.Location = new Point(450, 50);
scrollTopButton.Click += (s, e) =>
{
    sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Minimum;
};

Button scrollBottomButton = new Button();
scrollBottomButton.Text = "Scroll to Bottom";
scrollBottomButton.Location = new Point(450, 90);
scrollBottomButton.Click += (s, e) =>
{
    sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Maximum;
};

Button scrollMiddleButton = new Button();
scrollMiddleButton.Text = "Scroll to Middle";
scrollMiddleButton.Location = new Point(450, 130);
scrollMiddleButton.Click += (s, e) =>
{
    int middle = (sfScrollFrame1.VerticalScrollBar.Maximum + sfScrollFrame1.VerticalScrollBar.Minimum) / 2;
    sfScrollFrame1.VerticalScrollBar.Value = middle;
};

this.Controls.Add(scrollTopButton);
this.Controls.Add(scrollBottomButton);
this.Controls.Add(scrollMiddleButton);
```

## Changing the SmallChange Value

The `SmallChange` property controls how much the scrollbar moves when clicking the arrow buttons. Increase this value for faster scrolling.

### Setting SmallChange

```csharp
// Set horizontal scrollbar SmallChange
this.sfScrollFrame1.HorizontalScrollBar.SmallChange = 10;

// Set vertical scrollbar SmallChange
this.sfScrollFrame1.VerticalScrollBar.SmallChange = 10;
```

### Understanding SmallChange

- **Default:** Typically 1 (one pixel per arrow click)
- **Recommended Range:** 10-50 for noticeable speed increase
- **Effect:** Only affects arrow button clicks, not mouse wheel or drag
- **Independent:** Horizontal and vertical can have different values

### Example: Fast Scrolling Panel

```csharp
// Create panel with many controls
Panel panel = new Panel();
panel.AutoScroll = true;
panel.Size = new Size(400, 300);
panel.Location = new Point(20, 20);

// Add many buttons to require scrolling
for (int i = 0; i < 100; i++)
{
    Button btn = new Button();
    btn.Text = $"Button {i + 1}";
    btn.Size = new Size(150, 30);
    btn.Location = new Point(10, i * 35);
    panel.Controls.Add(btn);
}

// Attach SfScrollFrame
SfScrollFrame scrollFrame = new SfScrollFrame();
scrollFrame.Control = panel;

// Set fast scrolling (30 pixels per arrow click)
scrollFrame.VerticalScrollBar.SmallChange = 30;
scrollFrame.HorizontalScrollBar.SmallChange = 30;

this.Controls.Add(panel);
```

### SmallChange vs LargeChange

**SmallChange:**
- Controlled by SfScrollFrame
- Used for arrow button clicks
- Can be customized

**LargeChange:**
- Controlled by attached control
- Used for track clicks (between thumb and arrows)
- Cannot be modified (see [Limitations](limitations.md))

## Complete Working Example

Here's a complete example combining all basic features:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace SfScrollFrameDemo
{
    public partial class MainForm : Form
    {
        private SfScrollFrame sfScrollFrame1;
        private ListView listView1;

        public MainForm()
        {
            InitializeComponent();
            SetupListView();
            AttachScrollFrame();
        }

        private void SetupListView()
        {
            // Create and configure ListView
            listView1 = new ListView();
            listView1.View = View.Details;
            listView1.Location = new Point(20, 20);
            listView1.Size = new Size(400, 300);
            listView1.FullRowSelect = true;

            // Add columns
            listView1.Columns.Add("ID", 50);
            listView1.Columns.Add("Name", 150);
            listView1.Columns.Add("Category", 100);
            listView1.Columns.Add("Price", 100);

            // Populate with sample data
            for (int i = 1; i <= 100; i++)
            {
                ListViewItem item = new ListViewItem(i.ToString());
                item.SubItems.Add($"Product {i}");
                item.SubItems.Add($"Category {(i % 5) + 1}");
                item.SubItems.Add($"${(i * 10.99):F2}");
                listView1.Items.Add(item);
            }

            // Add to form
            this.Controls.Add(listView1);
        }

        private void AttachScrollFrame()
        {
            // Create SfScrollFrame
            sfScrollFrame1 = new SfScrollFrame();

            // Attach to ListView
            sfScrollFrame1.Control = listView1;

            // Configure scroll speed
            sfScrollFrame1.VerticalScrollBar.SmallChange = 20;
            sfScrollFrame1.HorizontalScrollBar.SmallChange = 20;
        }
    }
}
```

## Next Steps

- **Customize Appearance:** See [appearance-styling.md](appearance-styling.md) to learn about customizing colors, sizes, and themes
- **Add Context Menus:** See [context-menu.md](context-menu.md) for custom menu implementation
- **Localize for Multiple Languages:** See [localization.md](localization.md) for internationalization
- **Enable Testing:** See [ui-automation.md](ui-automation.md) for Coded UI and QTP support
- **Understand Constraints:** See [limitations.md](limitations.md) for applicable controls and known limitations
