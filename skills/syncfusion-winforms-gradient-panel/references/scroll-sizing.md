# Scroll and Auto-Size Settings

## Overview

GradientPanel provides automatic scrolling and sizing capabilities for dynamic content management. These features allow panels to adapt to content that exceeds their bounds or grow/shrink based on child controls.

**Key capabilities:**
- Automatic scroll bars when content overflows
- Configurable scroll margins and minimum sizes
- Auto-sizing based on content
- Two sizing modes: GrowOnly and GrowAndShrink

## AutoScroll Settings

### AutoScroll Property

Enable automatic scroll bars when content exceeds the panel's visible area:

```csharp
gradientPanel1.AutoScroll = true;
```

**When enabled:**
- Vertical scroll bar appears if content height exceeds panel height
- Horizontal scroll bar appears if content width exceeds panel width
- User can scroll to see all content

**When disabled (default):**
- No scroll bars, content may be clipped
- User cannot scroll to hidden content

### Basic AutoScroll Example

```csharp
GradientPanel scrollPanel = new GradientPanel();
scrollPanel.Size = new Size(300, 200);
scrollPanel.Location = new Point(20, 20);
scrollPanel.BackgroundColor = new BrushInfo(Color.WhiteSmoke);

// Enable auto-scroll
scrollPanel.AutoScroll = true;

// Add many child controls that exceed panel size
for (int i = 0; i < 20; i++)
{
    Button btn = new Button();
    btn.Text = $"Button {i + 1}";
    btn.Location = new Point(10, 10 + (i * 40));
    btn.Size = new Size(200, 30);
    scrollPanel.Controls.Add(btn);
}

this.Controls.Add(scrollPanel);
```

**Result:** Vertical scroll bar appears, user can scroll through all 20 buttons.

### AutoScrollMargin Property

Set the margin width around content during auto-scroll:

```csharp
gradientPanel1.AutoScroll = true;
gradientPanel1.AutoScrollMargin = new Size(5, 5);
```

**Parameters:**
- `Width` - Horizontal margin (left and right)
- `Height` - Vertical margin (top and bottom)

**Purpose:** Provides padding between content and scroll bars

### AutoScrollMinSize Property

Specify the minimum logical size for the auto-scroll region:

```csharp
gradientPanel1.AutoScroll = true;
gradientPanel1.AutoScrollMinSize = new Size(20, 20);
```

**Parameters:**
- `Width` - Minimum scrollable width
- `Height` - Minimum scrollable height

**Purpose:** Ensures a minimum scroll area even if content is smaller

### Complete AutoScroll Configuration

```csharp
GradientPanel scrollPanel = new GradientPanel();
scrollPanel.Size = new Size(400, 300);
scrollPanel.Location = new Point(20, 20);
scrollPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

// AutoScroll settings
scrollPanel.AutoScroll = true;
scrollPanel.AutoScrollMargin = new Size(5, 5);
scrollPanel.AutoScrollMinSize = new Size(20, 20);

// Border
scrollPanel.BorderStyle = BorderStyle.FixedSingle;
scrollPanel.BorderColor = Color.Gray;

this.Controls.Add(scrollPanel);
```

## Auto-Size Settings

### AutoSize Property

Enable automatic panel resizing based on content:

```csharp
gradientPanel1.AutoSize = true;
```

**When enabled:**
- Panel automatically resizes to fit all child controls
- Panel size changes as child controls are added/removed
- Controlled by `AutoSizeMode` property

**When disabled (default):**
- Panel maintains fixed size
- Content may exceed panel bounds

### AutoSizeMode Property

Specify how the panel resizes when `AutoSize` is enabled:

```csharp
gradientPanel1.AutoSize = true;
gradientPanel1.AutoSizeMode = AutoSizeMode.GrowOnly;
```

**AutoSizeMode.GrowOnly:**
- Panel grows as needed to fit content
- Panel does NOT shrink smaller than initial `Size` value
- Minimum size is the value specified in `Size` property

```csharp
// Panel starts at 300x200, grows as needed, never shrinks below 300x200
gradientPanel1.Size = new Size(300, 200);
gradientPanel1.AutoSize = true;
gradientPanel1.AutoSizeMode = AutoSizeMode.GrowOnly;
```

**AutoSizeMode.GrowAndShrink:**
- Panel grows and shrinks to exactly fit content
- May become smaller than initial `Size` value
- Dynamic sizing in both directions

```csharp
// Panel resizes to exactly fit content, regardless of initial size
gradientPanel1.Size = new Size(300, 200);
gradientPanel1.AutoSize = true;
gradientPanel1.AutoSizeMode = AutoSizeMode.GrowAndShrink;
```

### GrowOnly vs GrowAndShrink

#### GrowOnly Example

```csharp
GradientPanel growOnlyPanel = new GradientPanel();
growOnlyPanel.Size = new Size(200, 100);  // Minimum size
growOnlyPanel.Location = new Point(20, 20);
growOnlyPanel.AutoSize = true;
growOnlyPanel.AutoSizeMode = AutoSizeMode.GrowOnly;
growOnlyPanel.BackgroundColor = new BrushInfo(Color.LightBlue);
growOnlyPanel.BorderStyle = BorderStyle.FixedSingle;

// Add content that exceeds size
Label largeLabel = new Label();
largeLabel.Text = "This is a very long text that will cause the panel to grow";
largeLabel.AutoSize = true;
largeLabel.Location = new Point(10, 10);
growOnlyPanel.Controls.Add(largeLabel);

this.Controls.Add(growOnlyPanel);
```

**Result:** Panel grows to fit the label, but won't shrink below 200x100.

#### GrowAndShrink Example

```csharp
GradientPanel flexiblePanel = new GradientPanel();
flexiblePanel.Location = new Point(20, 150);
flexiblePanel.AutoSize = true;
flexiblePanel.AutoSizeMode = AutoSizeMode.GrowAndShrink;
flexiblePanel.BackgroundColor = new BrushInfo(Color.LightGreen);
flexiblePanel.BorderStyle = BorderStyle.FixedSingle;

// Add content
Label dynamicLabel = new Label();
dynamicLabel.Text = "Dynamic Size";
dynamicLabel.AutoSize = true;
dynamicLabel.Location = new Point(10, 10);
flexiblePanel.Controls.Add(dynamicLabel);

this.Controls.Add(flexiblePanel);
```

**Result:** Panel sizes exactly to fit the label, can be smaller than typical panel size.

## Common Scenarios

### Scenario 1: Scrollable Content Container

```csharp
GradientPanel contentContainer = new GradientPanel();
contentContainer.Dock = DockStyle.Fill;
contentContainer.BackgroundColor = new BrushInfo(Color.White);

// Enable scrolling
contentContainer.AutoScroll = true;
contentContainer.AutoScrollMargin = new Size(10, 10);

// Add many child controls
for (int i = 0; i < 50; i++)
{
    Label item = new Label();
    item.Text = $"Item {i + 1}";
    item.AutoSize = true;
    item.Location = new Point(10, 10 + (i * 25));
    contentContainer.Controls.Add(item);
}

this.Controls.Add(contentContainer);
```

### Scenario 2: Dynamic Form Panel

```csharp
GradientPanel formPanel = new GradientPanel();
formPanel.Location = new Point(20, 20);
formPanel.AutoSize = true;
formPanel.AutoSizeMode = AutoSizeMode.GrowOnly;
formPanel.Size = new Size(300, 100);  // Minimum size
formPanel.BackgroundColor = new BrushInfo(Color.WhiteSmoke);
formPanel.BorderStyle = BorderStyle.FixedSingle;

// Add form fields dynamically
int y = 10;
foreach (string fieldName in new[] { "Name", "Email", "Phone", "Address" })
{
    Label label = new Label();
    label.Text = fieldName + ":";
    label.Location = new Point(10, y);
    label.AutoSize = true;
    formPanel.Controls.Add(label);
    
    TextBox textBox = new TextBox();
    textBox.Location = new Point(100, y);
    textBox.Width = 180;
    formPanel.Controls.Add(textBox);
    
    y += 30;
}

this.Controls.Add(formPanel);
```

**Result:** Panel grows vertically to accommodate all form fields.

### Scenario 3: Toolbar Panel

```csharp
GradientPanel toolbarPanel = new GradientPanel();
toolbarPanel.Dock = DockStyle.Top;
toolbarPanel.AutoSize = true;
toolbarPanel.AutoSizeMode = AutoSizeMode.GrowAndShrink;
toolbarPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightGray,
    Color.Gray
);

// Add toolbar buttons
FlowLayoutPanel buttonFlow = new FlowLayoutPanel();
buttonFlow.AutoSize = true;
buttonFlow.Dock = DockStyle.Fill;

for (int i = 0; i < 10; i++)
{
    Button toolButton = new Button();
    toolButton.Text = $"Tool {i + 1}";
    toolButton.Size = new Size(60, 30);
    buttonFlow.Controls.Add(toolButton);
}

toolbarPanel.Controls.Add(buttonFlow);
this.Controls.Add(toolbarPanel);
```

**Result:** Toolbar panel automatically sizes to fit button content.

### Scenario 4: Message List with Scroll

```csharp
GradientPanel messagePanel = new GradientPanel();
messagePanel.Size = new Size(350, 400);
messagePanel.Location = new Point(20, 20);
messagePanel.BackgroundColor = new BrushInfo(Color.WhiteSmoke);
messagePanel.BorderStyle = BorderStyle.Fixed3D;
messagePanel.Border3DStyle = Border3DStyle.Sunken;

// Enable scrolling
messagePanel.AutoScroll = true;
messagePanel.AutoScrollMargin = new Size(5, 5);

// Add messages
int yPos = 5;
for (int i = 0; i < 30; i++)
{
    Panel messageItem = new Panel();
    messageItem.Size = new Size(320, 60);
    messageItem.Location = new Point(5, yPos);
    messageItem.BackColor = Color.White;
    messageItem.BorderStyle = BorderStyle.FixedSingle;
    
    Label messageLabel = new Label();
    messageLabel.Text = $"Message {i + 1}\nThis is message content...";
    messageLabel.AutoSize = true;
    messageLabel.Location = new Point(5, 5);
    messageItem.Controls.Add(messageLabel);
    
    messagePanel.Controls.Add(messageItem);
    yPos += 65;
}

this.Controls.Add(messagePanel);
```

### Scenario 5: Collapsible Section

```csharp
GradientPanel collapsiblePanel = new GradientPanel();
collapsiblePanel.Location = new Point(20, 20);
collapsiblePanel.Width = 300;
collapsiblePanel.AutoSize = true;
collapsiblePanel.AutoSizeMode = AutoSizeMode.GrowAndShrink;
collapsiblePanel.BackgroundColor = new BrushInfo(Color.LightSteelBlue);
collapsiblePanel.BorderStyle = BorderStyle.FixedSingle;

// Header
Button toggleButton = new Button();
toggleButton.Text = "▼ Show Details";
toggleButton.Dock = DockStyle.Top;
toggleButton.Height = 30;
collapsiblePanel.Controls.Add(toggleButton);

// Collapsible content
Panel contentPanel = new Panel();
contentPanel.Dock = DockStyle.Top;
contentPanel.AutoSize = true;
contentPanel.Visible = false;

Label details = new Label();
details.Text = "These are the details...\n\nMore information here...";
details.AutoSize = true;
details.Location = new Point(10, 10);
contentPanel.Controls.Add(details);

collapsiblePanel.Controls.Add(contentPanel);

// Toggle handler
toggleButton.Click += (s, e) => {
    contentPanel.Visible = !contentPanel.Visible;
    toggleButton.Text = contentPanel.Visible ? "▲ Hide Details" : "▼ Show Details";
};

this.Controls.Add(collapsiblePanel);
```

**Result:** Panel automatically resizes when content is shown/hidden.

## Troubleshooting

### Issue: Scroll bars not appearing

**Causes:**
- `AutoScroll = false`
- Content does not exceed panel size
- Child controls positioned outside panel bounds incorrectly

**Solutions:**
```csharp
// Ensure AutoScroll is enabled
gradientPanel1.AutoScroll = true;

// Verify content exceeds panel
// Panel size: 200x100, Content at Y=150 should trigger scroll
Label label = new Label();
label.Location = new Point(10, 150);  // Beyond panel height
label.AutoSize = true;
gradientPanel1.Controls.Add(label);
```

### Issue: Panel not auto-sizing

**Causes:**
- `AutoSize = false`
- Child controls have fixed positions that don't affect auto-size
- AutoSizeMode not set correctly

**Solutions:**
```csharp
// Enable AutoSize
gradientPanel1.AutoSize = true;
gradientPanel1.AutoSizeMode = AutoSizeMode.GrowAndShrink;

// Ensure child controls use auto-size
Label label = new Label();
label.AutoSize = true;  // Important for calculating size
label.Location = new Point(10, 10);
gradientPanel1.Controls.Add(label);
```

### Issue: Panel shrinks too small

**Cause:** Using `GrowAndShrink` mode

**Solution:**
```csharp
// Use GrowOnly to maintain minimum size
gradientPanel1.Size = new Size(300, 200);  // Minimum
gradientPanel1.AutoSize = true;
gradientPanel1.AutoSizeMode = AutoSizeMode.GrowOnly;  // Won't shrink below 300x200
```

### Issue: Scroll margin not visible

**Cause:** AutoScrollMargin value too small

**Solution:**
```csharp
// Increase margin for visibility
gradientPanel1.AutoScroll = true;
gradientPanel1.AutoScrollMargin = new Size(10, 10);  // Larger margin
```

### Issue: Content clipped despite AutoScroll

**Causes:**
- Child control positioned with Dock or Anchor
- AutoScroll minimum size too small

**Solutions:**
```csharp
// Ensure sufficient minimum size
gradientPanel1.AutoScrollMinSize = new Size(100, 100);

// Or position child controls absolutely
label.Location = new Point(10, 10);  // Not docked
```

## Best Practices

1. **Enable AutoScroll for many items** - Use AutoScroll when content may exceed panel size
2. **Set appropriate margins** - Use AutoScrollMargin (5-10px) for comfortable spacing
3. **GrowOnly for forms** - Use GrowOnly mode to maintain minimum panel size
4. **GrowAndShrink for dynamic content** - Use GrowAndShrink for panels that should fit content exactly
5. **Child AutoSize** - Set child controls to AutoSize for correct panel sizing
6. **Test scrolling** - Verify scroll bars appear and function correctly
7. **Minimum size** - Always set reasonable initial Size for GrowOnly mode
8. **Dock carefully** - Docked child controls may interfere with auto-sizing
9. **Performance** - Limit number of child controls for smooth scrolling
10. **Visual feedback** - Ensure scroll bars are visible (may depend on OS theme)
