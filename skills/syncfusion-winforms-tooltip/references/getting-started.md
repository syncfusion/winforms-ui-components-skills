# Getting Started with SfToolTip

This guide covers the essential steps to add and configure the Syncfusion `SfToolTip` component in Windows Forms applications.

## Assembly Deployment

Before using `SfToolTip`, add the required assembly reference to your project.

### Required Assemblies

| Assembly | Description |
|----------|-------------|
| `Syncfusion.Core.WinForms` | Contains theme-related classes and basic Syncfusion components including SfToolTip |

### Adding References

**Via NuGet:**
1. Right-click project → Manage NuGet Packages
2. Search for `Syncfusion.Core.WinForms`
3. Install the package

**Via DLL Reference:**
1. Right-click project → Add Reference
2. Browse to Syncfusion installation folder
3. Add `Syncfusion.Core.WinForms.dll` and `Syncfusion.Shared.Base`

Refer to [control dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#sftooltip) for the complete list of dependencies.

## Setting SfToolTip to a Control

The `SfToolTip` component can be configured using either the Visual Studio Designer or programmatically through code.

### Through Designer

#### Method 1: Setting Using Text

The simplest way to add a tooltip is with plain text.

**Steps:**
1. Drag `SfToolTip` from the Toolbox to your form
2. The component appears in the component tray
3. Select any control on the form (e.g., Button)
4. In the Properties window, find the extended property: **"ToolTip on sfToolTip1"**
5. Enter your tooltip text in this property

**Example:**
```
ToolTip on sfToolTip1: "The ToolTip information of the Button control."
```

**Designer Generated Code:**
```csharp
SfToolTip sfToolTip1 = new SfToolTip(this.components);
Button button1 = new System.Windows.Forms.Button();

// button1
this.button1.Location = new System.Drawing.Point(62, 74);
this.button1.Name = "button1";
this.button1.Size = new System.Drawing.Size(84, 28);
this.button1.TabIndex = 0;
this.button1.Text = "Button";
this.sfToolTip1.SetToolTip(this.button1, "The ToolTip information of the Button control.");
this.button1.UseVisualStyleBackColor = true;
```

**Result:** The tooltip text displays when hovering over the control.

#### Method 2: Setting Using ToolTipInfo

For more complex tooltips with multiple items or styling, use `ToolTipInfo`.

**Steps:**
1. Drag `SfToolTip` to the form
2. Select a control on the form
3. In Properties, find **"ToolTipInfo on sfToolTip1"**
4. Click the **Ellipse (...)** button
5. The **SfToolTip Editor** dialog opens
6. Click the Ellipse button in the **Items** property
7. The **ToolTipItem Collection Editor** opens
8. Click **Add** to create one or more `ToolTipItem` objects
9. Customize each item's properties (Text, Image, Style, etc.)
10. Click **OK** to close editors

**Designer Generated Code:**
```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
ToolTipItem toolTipItem1 = new ToolTipItem();
ToolTipItem toolTipItem2 = new ToolTipItem();
SfToolTip sfToolTip1 = new SfToolTip(this.components);
Button button1 = new System.Windows.Forms.Button();

// button1
this.button1.Location = new System.Drawing.Point(62, 74);
this.button1.Name = "button1";
this.button1.Size = new System.Drawing.Size(84, 28);
this.button1.TabIndex = 0;
this.button1.Text = "Button";
toolTipItem1.Text = "ToolTipItem1 Text";
toolTipItem2.Text = "ToolTipItem2 Text";
toolTipInfo1.Items.AddRange(new ToolTipItem[] {
    toolTipItem1,
    toolTipItem2
});
this.sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
this.button1.UseVisualStyleBackColor = true;
```

**Result:** Multi-item tooltip with structured content.

### Through Code

#### Setting Using Text

Use the `SetToolTip` method to assign simple text tooltips programmatically.

**Syntax:**
```csharp
sfToolTip.SetToolTip(control, tooltipText);
```

**Example:**
```csharp
using Syncfusion.Windows.Forms;

SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.SetToolTip(this.button1, "The ToolTip information of the Button control.");
```

**Use Case:** Quick tooltips for individual controls without complex formatting.

#### Setting Using ToolTipInfo

Use the `SetToolTipInfo` method for structured tooltips with multiple items.

**Example:**
```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.Controls;

SfToolTip sfToolTip1 = new SfToolTip();
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

// Create first item
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "ToolTipItem 1 Text";

// Create second item
ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "ToolTipItem 2 Text";

// Add items to ToolTipInfo
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2 });

// Assign to control
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Use Case:** Complex tooltips with multiple sections, images, or custom styling.

## Displaying Tooltips Programmatically

Control tooltip display behavior with Show and Hide methods.

### Show Method

Display tooltips on demand without waiting for hover.

#### Show Tooltip at Cursor Position

```csharp
// Show text at current mouse position
this.sfToolTip1.Show("Programmatically showing the tooltip text");
```

**Use Case:** Display tooltip in response to button click or other event.

#### Show Tooltip at Specific Position

```csharp
// Show text at explicit coordinates
Point location = new Point(300, 300);
this.sfToolTip1.Show("Programmatically showing the tooltip in specified position", location);
```

**Use Case:** Position tooltip relative to specific screen area or control.

#### Show ToolTipInfo

```csharp
ToolTipInfo toolTipInfo = new ToolTipInfo();
ToolTipItem toolTipItem = new ToolTipItem();
toolTipItem.Text = "ToolTipItem text";
toolTipInfo.Items.Add(toolTipItem);

// Shows ToolTipInfo at cursor position
this.sfToolTip1.Show(toolTipInfo);
```

**Use Case:** Show complex multi-item tooltips programmatically.

#### Example: Show on Button Click

```csharp
Button button1 = new Button();
this.Controls.Add(button1);
button1.Click += Button1_Click;

Button button2 = new Button();
this.Controls.Add(button2);
button2.Click += Button2_Click;

Button button3 = new Button();
this.Controls.Add(button3);
button3.Click += Button3_Click;

private void Button1_Click(object sender, EventArgs e)
{
    // Shows the text in the cursor position
    this.sfToolTip1.Show("Programmatically showing the tooltip text");
}

private void Button2_Click(object sender, EventArgs e)
{
    // Shows the text in the specified position
    this.sfToolTip1.Show("Programmatically showing the tooltip in specified position", 
                         new Point(300, 300));
}

private void Button3_Click(object sender, EventArgs e)
{
    ToolTipInfo toolTipInfo = new ToolTipInfo();
    ToolTipItem toolTipItem = new ToolTipItem();
    toolTipItem.Text = "ToolTipItem text";
    toolTipInfo.Items.Add(toolTipItem);
    
    // Shows the ToolTipInfo in cursor position
    this.sfToolTip1.Show(toolTipInfo);
}
```

### Hide Method

Manually hide a visible tooltip.

```csharp
this.sfToolTip1.Hide();
```

**Use Case:** Close tooltip after specific duration or user action.

**Example with Delay:**
```csharp
// Show tooltip
this.sfToolTip1.Show("Processing...");

// Hide after 3 seconds
this.Load += Form1_Load;

private async void Form1_Load(object sender, EventArgs e)
{
    sfToolTip1.Show("Processing...");
    await Task.Delay(3000);
    sfToolTip1.Hide();
}

```

## Setting Tooltip Delay

Control how quickly tooltips appear and how long they remain visible.

### InitialDelay Property

The `InitialDelay` property specifies the time (in milliseconds) the tooltip waits before displaying after the pointer rests on a control.

**Default Value:** 0 milliseconds (appears immediately)

**Syntax:**
```csharp
sfToolTip1.InitialDelay = 1000; // Wait 1 second before showing
```

**Use Case:** Prevent tooltips from appearing too quickly during fast cursor movements.

**Example:**
```csharp
SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.InitialDelay = 1000; // 1 second delay

sfToolTip1.SetToolTip(this.button1, "This tooltip appears after 1 second");
```

### AutoPopDelay Property

The `AutoPopDelay` property specifies the duration (in milliseconds) the tooltip remains visible when the mouse pointer is on a control.

**Default Value:** 5000 milliseconds (5 seconds)

**Syntax:**
```csharp
sfToolTip1.AutoPopDelay = 10000; // Display for 10 seconds
```

**Use Case:** Keep complex tooltips visible longer for users to read detailed content.

**Example:**
```csharp
SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.AutoPopDelay = 10000; // Display for 10 seconds

ToolTipInfo toolTipInfo = new ToolTipInfo();
ToolTipItem item = new ToolTipItem();
item.Text = "Detailed information that requires more time to read...";
toolTipInfo.Items.Add(item);

sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

**Combined Example:**
```csharp
SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.InitialDelay = 500;    // Wait 0.5 seconds before showing
sfToolTip1.AutoPopDelay = 8000;   // Keep visible for 8 seconds

sfToolTip1.SetToolTip(this.submitButton, "Click to submit the form");
```

## Showing Tooltip with Beak (Balloon Style)

Create balloon-style tooltips with directional beaks pointing to the control.

### Enabling Balloon Style

Set the `ToolTipStyle` property to `ToolTipStyle.Balloon`.

**Example:**
```csharp
using Syncfusion.WinForms.Controls;

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Result:** Tooltip displays with a triangular beak pointing to the control.

### Setting Beak Back Color

Customize the beak color using the `BeakBackColor` property.

**Example:**
```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo1.BeakBackColor = Color.LightSkyBlue;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Use Case:** Match beak color to tooltip background for cohesive appearance.

## Summary

This guide covered:
- **Assembly deployment:** Adding required Syncfusion.Core.WinForms reference
- **Designer setup:** Setting tooltips via Properties window with text or ToolTipInfo
- **Code setup:** Using SetToolTip and SetToolTipInfo methods
- **Programmatic control:** Show and Hide methods with positioning
- **Timing:** InitialDelay and AutoPopDelay configuration
- **Balloon style:** Basic setup with ToolTipStyle.Balloon and BeakBackColor

**Next Steps:**
- Explore multi-item tooltips and content customization in [tooltip-items-and-content.md](tooltip-items-and-content.md)
- Learn advanced balloon styling in [balloon-style-and-beak.md](balloon-style-and-beak.md)
- Customize appearance in [appearance-customization.md](appearance-customization.md)
