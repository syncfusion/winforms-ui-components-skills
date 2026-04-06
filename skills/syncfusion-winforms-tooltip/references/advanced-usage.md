# Advanced Usage and Events

## Table of Contents
- [Getting and Setting Tooltip Information](#getting-and-setting-tooltip-information)
- [ToolTipShowing Event](#tooltipshowing-event)
- [Changing Tooltip Location](#changing-tooltip-location)
- [Setting Minimum and Maximum Widths](#setting-minimum-and-maximum-widths)
- [Custom Drawing](#custom-drawing)

This guide covers advanced `SfToolTip` features including programmatic access to tooltip data, event handling, dynamic customization, and custom rendering.

## Getting and Setting Tooltip Information

Access and modify tooltip configuration programmatically for dynamic tooltip management.

### Getting Tooltip Text

Retrieve the simple text tooltip assigned to a control using the `GetToolTip` method.

**Syntax:**
```csharp
string tooltipText = sfToolTip.GetToolTip(control);
```

**Example:**
```csharp
string toolTipText = this.sfToolTip1.GetToolTip(this.button1);
Console.WriteLine($"Tooltip: {toolTipText}");
```

**Returns:** The tooltip text string, or empty string if no text tooltip is set.

**Use Case:** 
- Validate tooltip assignment
- Log tooltip content
- Duplicate tooltip to another control

### Setting Tooltip Text

Assign or update simple text tooltip using the `SetToolTip` method.

**Syntax:**
```csharp
sfToolTip.SetToolTip(control, tooltipText);
```

**Example:**
```csharp
this.sfToolTip1.SetToolTip(this.button1, "Button1 ToolTip Text");
```

**Use Case:**
- Dynamically change tooltips based on application state
- Localize tooltip text
- Update tooltips in response to user actions

**Example - Dynamic Text Update:**
```csharp
private void UpdateTooltipBasedOnState()
{
    if (isProcessing)
    {
        sfToolTip1.SetToolTip(this.statusButton, "Processing... Please wait");
    }
    else
    {
        sfToolTip1.SetToolTip(this.statusButton, "Click to view status");
    }
}
```

### Getting ToolTipInfo

Retrieve the complete `ToolTipInfo` object assigned to a control using the `GetToolTipInfo` method.

**Syntax:**
```csharp
ToolTipInfo tooltipInfo = sfToolTip.GetToolTipInfo(control);
```

**Example:**
```csharp
ToolTipInfo toolTipInfo = this.sfToolTip1.GetToolTipInfo(this.button1);

// Access ToolTipInfo properties
Console.WriteLine($"Item count: {toolTipInfo.Items.Count}");
Console.WriteLine($"Style: {toolTipInfo.ToolTipStyle}");
```

**Returns:** The `ToolTipInfo` object, or `null` if not set.

**Use Case:**
- Inspect current tooltip configuration
- Clone tooltip settings to other controls
- Modify existing tooltip properties

**Example - Cloning Tooltip:**
```csharp
// Get tooltip from button1
ToolTipInfo originalInfo = this.sfToolTip1.GetToolTipInfo(this.button1);

// Clone and modify
ToolTipInfo clonedInfo = new ToolTipInfo();
foreach (ToolTipItem item in originalInfo.Items)
{
    ToolTipItem newItem = new ToolTipItem();
    newItem.Text = item.Text;
    newItem.Image = item.Image;
    clonedInfo.Items.Add(newItem);
}

// Apply to button2
this.sfToolTip1.SetToolTipInfo(this.button2, clonedInfo);
```

### Setting ToolTipInfo

Assign or update the complete `ToolTipInfo` for a control using the `SetToolTipInfo` method.

**Syntax:**
```csharp
sfToolTip.SetToolTipInfo(control, toolTipInfo);
```

**Example:**
```csharp
ToolTipInfo toolTipInfo = new ToolTipInfo();
ToolTipItem item = new ToolTipItem();
item.Text = "Detailed tooltip information";
toolTipInfo.Items.Add(item);

this.sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

**Use Case:**
- Replace entire tooltip configuration
- Apply templates or predefined tooltip styles
- Update multiple tooltip properties atomically

**Example - Template Application:**
```csharp
private ToolTipInfo CreateWarningTooltip(string message)
{
    ToolTipInfo warningInfo = new ToolTipInfo();
    warningInfo.BorderColor = Color.Red;
    warningInfo.BorderThickness = 2;
    
    ToolTipItem item = new ToolTipItem();
    item.Text = $"⚠ Warning: {message}";
    item.Style.BackColor = Color.MistyRose;
    item.Style.ForeColor = Color.DarkRed;
    
    warningInfo.Items.Add(item);
    return warningInfo;
}

// Apply warning tooltip
this.sfToolTip1.SetToolTipInfo(this.criticalButton, CreateWarningTooltip("Irreversible action"));
```

## ToolTipShowing Event

The `ToolTipShowing` event fires immediately before a tooltip displays, allowing dynamic customization or cancellation.

**Event Signature:**
```csharp
public event EventHandler<ToolTipShowingEventArgs> ToolTipShowing;
```

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `Control` | Control | The control triggering the tooltip |
| `ToolTipInfo` | ToolTipInfo | The tooltip configuration (modifiable) |
| `Location` | Point | The tooltip display location (modifiable) |
| `Cancel` | bool | Set to `true` to prevent tooltip display |

### Subscribing to the Event

```csharp
this.sfToolTip1.ToolTipShowing += SfToolTip1_ToolTipShowing;

private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    // Event handling logic
}
```

### Disabling Tooltip from Showing

Cancel tooltip display conditionally by setting `e.Cancel = true`.

**Example - Disable for Specific Control:**
```csharp
this.sfToolTip1.ToolTipShowing += SfToolTip1_ToolTipShowing;

private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control.Name == "cancelButton")
    {
        e.Cancel = true;
    }
}
```

**Example - Disable for Disabled Controls:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (!e.Control.Enabled)
    {
        e.Cancel = true; // Don't show tooltips on disabled controls
    }
}
```

**Use Case:**
- Hide tooltips during specific application states
- Prevent tooltips on disabled controls
- Conditional display based on user permissions

### Customizing Appearance Per Control

Modify `e.ToolTipInfo` properties to dynamically adjust appearance.

**Example - Style Based on Control Type:**
```csharp
this.sfToolTip1.ToolTipShowing += SfToolTip1_ToolTipShowing;

private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control is Button)
    {
        e.ToolTipInfo.Items[0].Style.BackColor = Color.LightSkyBlue;
        e.ToolTipInfo.Items[0].Style.ForeColor = Color.Black;
    }
}
```

**Example - Style Based on Control State:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control is Button button)
    {
        if (!button.Enabled)
        {
            // Grayed out style for disabled controls
            e.ToolTipInfo.Items[0].Style.BackColor = Color.LightGray;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.Gray;
        }
        else if (button.Tag?.ToString() == "Primary")
        {
            // Highlight style for primary buttons
            e.ToolTipInfo.Items[0].Style.BackColor = Color.LightBlue;
            e.ToolTipInfo.BorderColor = Color.Blue;
            e.ToolTipInfo.BorderThickness = 2;
        }
    }
}
```

**Example - Style Based on Control Tag:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    string severity = e.Control.Tag?.ToString();
    
    switch (severity)
    {
        case "Error":
            e.ToolTipInfo.Items[0].Style.BackColor = Color.MistyRose;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.DarkRed;
            e.ToolTipInfo.BorderColor = Color.Red;
            break;
            
        case "Warning":
            e.ToolTipInfo.Items[0].Style.BackColor = Color.LightYellow;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.DarkOrange;
            e.ToolTipInfo.BorderColor = Color.Orange;
            break;
            
        case "Info":
            e.ToolTipInfo.Items[0].Style.BackColor = Color.AliceBlue;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.DarkBlue;
            e.ToolTipInfo.BorderColor = Color.Blue;
            break;
    }
}
```

### Dynamic Content Updates

Modify tooltip content just before display.

**Example - Show Current Time:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control.Name == "clockButton")
    {
        e.ToolTipInfo.Items[0].Text = $"Current Time: {DateTime.Now:HH:mm:ss}";
    }
}
```

**Example - Show Live Data:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control.Name == "statusIndicator")
    {
        int activeUsers = GetActiveUserCount();
        e.ToolTipInfo.Items[0].Text = $"Active Users: {activeUsers}";
    }
}
```

## Changing Tooltip Location

Customize where the tooltip appears using the `Location` property in the `ToolTipShowing` event.

**Syntax:**
```csharp
e.Location = new Point(x, y);
```

**Example - Offset Tooltip:**
```csharp
this.sfToolTip1.ToolTipShowing += SfToolTip1_ToolTipShowing;

private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    // Move tooltip 20 pixels right and 25 pixels up from default position
    e.Location = new Point(e.Location.X + 20, e.Location.Y - 25);
}
```

**Use Case:**
- Position tooltip away from cursor
- Align tooltip with specific screen areas
- Prevent tooltip from covering important UI elements

**Example - Position Above Control:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    // Position tooltip above the control
    Point controlLocation = e.Control.PointToScreen(Point.Empty);
    e.Location = new Point(
        controlLocation.X + (e.Control.Width / 2),
        controlLocation.Y - 50 // 50 pixels above control
    );
}
```

**Example - Position at Fixed Screen Location:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control.Name == "specialButton")
    {
        // Always show at specific screen coordinates
        e.Location = new Point(100, 100);
    }
}
```

**Coordinate System:** `Location` uses screen coordinates (absolute position).

## Setting Minimum and Maximum Widths

Control tooltip width constraints using `MinWidth` and `MaxWidth` properties.

### MinWidth Property

Set the minimum width for the tooltip.

**Syntax:**
```csharp
toolTipInfo.MinWidth = minimumWidthInPixels;
```

**Example:**
```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.MinWidth = 100;

ToolTipItem item = new ToolTipItem();
item.Text = "Short";
toolTipInfo1.Items.Add(item);

sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Behavior:** If the tooltip's natural width is less than `MinWidth`, it will be expanded to `MinWidth`.

**Use Case:** Ensure consistent tooltip sizes for visual uniformity.

### MaxWidth Property

Set the maximum width for the tooltip.

**Syntax:**
```csharp
toolTipInfo.MaxWidth = maximumWidthInPixels;
```

**Example:**
```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.MaxWidth = 500;

ToolTipItem item = new ToolTipItem();
item.Text = "Very long tooltip text that would normally extend beyond screen boundaries...";
toolTipInfo1.Items.Add(item);

sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Behavior:** If the tooltip's natural width exceeds `MaxWidth`, it will be constrained to `MaxWidth` and text will wrap.

**Use Case:** Prevent tooltips from extending off-screen or becoming too wide to read comfortably.

### Combined Width Constraints

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.MinWidth = 100;
toolTipInfo1.MaxWidth = 500;

ToolTipItem item = new ToolTipItem();
item.Text = "Tooltip with width constraints";
toolTipInfo1.Items.Add(item);

sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Best Practice:** Set `MaxWidth` between 300-600 pixels for optimal readability.

### Dynamic Width Based on Content

```csharp
private void ApplyTooltipWithDynamicWidth(Control control, string text)
{
    ToolTipInfo toolTipInfo = new ToolTipInfo();
    
    // Short text: narrow tooltip
    if (text.Length < 50)
    {
        toolTipInfo.MinWidth = 100;
        toolTipInfo.MaxWidth = 200;
    }
    // Long text: wider tooltip
    else
    {
        toolTipInfo.MinWidth = 200;
        toolTipInfo.MaxWidth = 500;
    }
    
    ToolTipItem item = new ToolTipItem();
    item.Text = text;
    toolTipInfo.Items.Add(item);
    
    sfToolTip1.SetToolTipInfo(control, toolTipInfo);
}
```

## Custom Drawing

Implement completely custom tooltip rendering using the `DrawToolTipItem` event.

### DrawToolTipItem Event

This event fires during tooltip rendering for each `ToolTipItem`, allowing custom drawing logic.

**Event Signature:**
```csharp
public event EventHandler<DrawToolTipItemEventArgs> DrawToolTipItem;
```

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `Graphics` | Graphics | Drawing surface for custom rendering |
| `ToolTipItem` | ToolTipItem | The item being drawn |
| `ToolTipItemRectangle` | Rectangle | The item's drawing bounds |
| `Cancel` | bool | Set to `true` to prevent default drawing |

### Basic Custom Drawing

**Example - Gradient Background with Custom Text:**
```csharp
this.sfToolTip1.DrawToolTipItem += SfToolTip1_DrawToolTipItem;

private void SfToolTip1_DrawToolTipItem(object sender, DrawToolTipItemEventArgs e)
{
    // Cancel default drawing
    e.Cancel = true;
    
    // Draw custom gradient background
    LinearGradientBrush gradientBrush = new LinearGradientBrush(
        e.ToolTipItemRectangle, 
        Color.LightSkyBlue, 
        Color.LightGreen, 
        LinearGradientMode.Horizontal
    );
    e.Graphics.FillRectangle(gradientBrush, e.ToolTipItemRectangle);
    
    // Draw border
    Pen pen = new Pen(Color.Black);
    Rectangle borderRectangle = e.ToolTipItemRectangle;
    borderRectangle.Width -= 1;
    borderRectangle.Height -= 1;
    e.Graphics.DrawRectangle(pen, borderRectangle);
    
    // Draw text
    SolidBrush solidBrush = new SolidBrush(Color.Black);
    StringFormat stringFormat = new StringFormat();
    stringFormat.Alignment = StringAlignment.Center;
    stringFormat.LineAlignment = StringAlignment.Center;
    e.Graphics.DrawString(
        e.ToolTipItem.Text, 
        e.ToolTipItem.Style.Font, 
        solidBrush, 
        e.ToolTipItemRectangle, 
        stringFormat
    );
}
```

**Important:** Set `e.Cancel = true` to prevent default rendering; otherwise, custom drawing will be overlaid on default rendering.

### Custom Drawing with Rounded Corners

```csharp
private void SfToolTip1_DrawToolTipItem(object sender, DrawToolTipItemEventArgs e)
{
    e.Cancel = true;
    
    // Create rounded rectangle path
    GraphicsPath path = GetRoundedRectanglePath(e.ToolTipItemRectangle, 10);
    
    // Fill with solid color
    e.Graphics.SmoothingMode = SmoothingMode.AntiAlias;
    e.Graphics.FillPath(new SolidBrush(Color.WhiteSmoke), path);
    
    // Draw border
    e.Graphics.DrawPath(new Pen(Color.Gray, 2), path);
    
    // Draw text
    StringFormat format = new StringFormat
    {
        Alignment = StringAlignment.Center,
        LineAlignment = StringAlignment.Center
    };
    e.Graphics.DrawString(
        e.ToolTipItem.Text,
        e.ToolTipItem.Style.Font,
        Brushes.Black,
        e.ToolTipItemRectangle,
        format
    );
}

private GraphicsPath GetRoundedRectanglePath(Rectangle rect, int radius)
{
    GraphicsPath path = new GraphicsPath();
    int diameter = radius * 2;
    
    path.AddArc(rect.X, rect.Y, diameter, diameter, 180, 90);
    path.AddArc(rect.Right - diameter, rect.Y, diameter, diameter, 270, 90);
    path.AddArc(rect.Right - diameter, rect.Bottom - diameter, diameter, diameter, 0, 90);
    path.AddArc(rect.X, rect.Bottom - diameter, diameter, diameter, 90, 90);
    path.CloseFigure();
    
    return path;
}
```

### Custom Drawing with Image and Text

```csharp
private void SfToolTip1_DrawToolTipItem(object sender, DrawToolTipItemEventArgs e)
{
    e.Cancel = true;
    
    // Fill background
    e.Graphics.FillRectangle(
        new SolidBrush(Color.White), 
        e.ToolTipItemRectangle
    );
    
    // Draw image if present
    if (e.ToolTipItem.Image != null)
    {
        Rectangle imageRect = new Rectangle(
            e.ToolTipItemRectangle.X + 5,
            e.ToolTipItemRectangle.Y + 5,
            e.ToolTipItem.Style.ImageSize.Width,
            e.ToolTipItem.Style.ImageSize.Height
        );
        e.Graphics.DrawImage(e.ToolTipItem.Image, imageRect);
    }
    
    // Calculate text rectangle
    int textX = e.ToolTipItemRectangle.X + 
                e.ToolTipItem.Style.ImageSize.Width + 15;
    Rectangle textRect = new Rectangle(
        textX,
        e.ToolTipItemRectangle.Y,
        e.ToolTipItemRectangle.Width - textX,
        e.ToolTipItemRectangle.Height
    );
    
    // Draw text
    StringFormat format = new StringFormat
    {
        Alignment = StringAlignment.Near,
        LineAlignment = StringAlignment.Center
    };
    e.Graphics.DrawString(
        e.ToolTipItem.Text,
        e.ToolTipItem.Style.Font,
        new SolidBrush(e.ToolTipItem.Style.ForeColor),
        textRect,
        format
    );
}
```

### Conditional Custom Drawing

Draw only specific items custom while leaving others with default rendering.

```csharp
private void SfToolTip1_DrawToolTipItem(object sender, DrawToolTipItemEventArgs e)
{
    // Only customize items tagged as "Custom"
    if (e.ToolTipItem.Tag?.ToString() != "Custom")
    {
        return; // Use default rendering
    }
    
    e.Cancel = true;
    
    // Custom drawing for tagged items
    e.Graphics.FillRectangle(
        new LinearGradientBrush(
            e.ToolTipItemRectangle,
            Color.Orange,
            Color.Yellow,
            LinearGradientMode.Vertical
        ),
        e.ToolTipItemRectangle
    );
    
    e.Graphics.DrawString(
        e.ToolTipItem.Text,
        new Font("Arial", 10f, FontStyle.Bold),
        Brushes.White,
        e.ToolTipItemRectangle
    );
}
```

### Custom Drawing Best Practices

1. **Always set `e.Cancel = true`** when providing complete custom rendering
2. **Use anti-aliasing** for smooth graphics: `e.Graphics.SmoothingMode = SmoothingMode.AntiAlias`
3. **Respect `ToolTipItemRectangle` bounds** to avoid drawing outside allocated space
4. **Dispose resources** when creating Pens, Brushes, or Paths
5. **Test with various text lengths** to ensure layout scales properly
6. **Consider accessibility** - maintain readable text contrast

## Summary

This guide covered:
- **Getting/Setting:** GetToolTip, SetToolTip, GetToolTipInfo, SetToolTipInfo methods
- **ToolTipShowing event:** Conditional display, dynamic customization, appearance modification
- **Location customization:** Positioning tooltips programmatically
- **Width constraints:** MinWidth and MaxWidth properties
- **Custom drawing:** DrawToolTipItem event with complete rendering control

**Common Use Cases:**
1. **Dynamic tooltips:** Update content based on application state
2. **Conditional display:** Show/hide based on user permissions or context
3. **Style per control:** Different appearances for different control types
4. **Custom positioning:** Precise tooltip placement
5. **Advanced rendering:** Unique visual designs beyond standard styling

**Best Practices:**
1. Use `ToolTipShowing` for dynamic adjustments rather than recreating tooltips
2. Cache tooltip templates for reuse across controls
3. Test custom drawing with various screen DPI settings
4. Ensure custom-drawn text remains readable
5. Consider performance impact of complex custom drawing

**Next Steps:**
- Review basic setup in [getting-started.md](getting-started.md)
- Explore appearance options in [appearance-customization.md](appearance-customization.md)
