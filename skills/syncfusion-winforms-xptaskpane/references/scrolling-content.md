# Scrolling and Content Overflow

## Enabling Vertical Scrolling

Enable automatic vertical scrolling when page content exceeds visible area:

```csharp
// Enable vertical scrolling
xpTaskPane1.VerticalScroll = true;
```

```vb
' Enable vertical scrolling
Me.xpTaskPane1.VerticalScroll = True
```

**When to Use:**
- Pages contain more content than visible area
- Want automatic scroll behavior
- Supporting variable content sizes
- Building responsive layouts

**Default Behavior:**
- Scrolling disabled by default (`VerticalScroll = false`)
- Content may be clipped if it exceeds page height
- Enable when needed

## Scroll Speed Configuration

Control how fast content scrolls on mouse hover:

```csharp
// Set scroll speed (default is typically 10-20)
xpTaskPane1.ScrollSpeed = 20;
```

```vb
' Set scroll speed
Me.xpTaskPane1.ScrollSpeed = 20
```

**Speed Values:**
- Lower values (5-10): Slower scrolling
- Medium values (15-25): Normal scrolling
- Higher values (30+): Faster scrolling

**Speed Examples:**

```csharp
// Slow and controlled
xpTaskPane1.ScrollSpeed = 5;

// Fast for power users
xpTaskPane1.ScrollSpeed = 30;

// Default balanced speed
xpTaskPane1.ScrollSpeed = 15;
```

## Scroll Behavior

### Automatic Hover-Based Scrolling

When scrolling is enabled:

1. Mouse hovers over scrollbar
2. Content automatically moves (scrolls)
3. Scroll speed determines movement rate
4. Hover away to stop scrolling

```
XPTaskPane Content Area
┌─────────────────────┐
│ Task Page Content   │ ← Normal display
│ More content here   │
│ Even more content   │ ← Page area overflowing
│ (scrollable)        │
└─────────────────────┘
        ^ Hover here for auto-scroll
```

### No Manual Scroll Bar

- XPTaskPane doesn't display traditional scroll bars
- Scrolling triggered by mouse hover
- Content scrolls smoothly when hovering
- Clean interface without scroll bar UI

## Complete Scrolling Setup

```csharp
private void ConfigureScrolling()
{
    // Enable scrolling for overflow content
    xpTaskPane1.VerticalScroll = true;

    // Set scroll speed for smooth experience
    xpTaskPane1.ScrollSpeed = 15;

    // Add page with lots of content
    XPTaskPage contentPage = new XPTaskPage();
    contentPage.Title = "Large Content Area";

    // Add many controls that will overflow
    for (int i = 0; i < 20; i++)
    {
        Label lbl = new Label();
        lbl.Text = "Item " + (i + 1);
        lbl.Height = 30;
        lbl.Dock = DockStyle.Top;
        contentPage.Controls.Add(lbl);
    }

    wizardContainer1.Controls.Add(contentPage);
    xpTaskPane1.TaskPages = new XPTaskPage[] { contentPage };
}
```

## Use Cases for Scrolling

### Case 1: Dynamic Content Lists

```csharp
// Page with scrollable item list
public void CreateItemListPage(List<string> items)
{
    xpTaskPane1.VerticalScroll = true;
    xpTaskPane1.ScrollSpeed = 20;

    XPTaskPage listPage = new XPTaskPage();
    listPage.Title = "Items";

    // Add items dynamically
    foreach (string item in items)
    {
        CheckBox chk = new CheckBox();
        chk.Text = item;
        chk.Height = 25;
        chk.Dock = DockStyle.Top;
        listPage.Controls.Add(chk);
    }

    wizardContainer1.Controls.Add(listPage);
}
```

### Case 2: Settings Forms

```csharp
// Scrollable settings page
public void CreateSettingsPage()
{
    xpTaskPane1.VerticalScroll = true;

    XPTaskPage settingsPage = new XPTaskPage();
    settingsPage.Title = "Settings";

    // Create panel container
    Panel panel = new Panel();
    panel.Dock = DockStyle.Fill;

    // Add many settings controls
    int yPos = 10;
    for (int i = 0; i < 15; i++)
    {
        Label lbl = new Label();
        lbl.Text = "Setting " + (i + 1) + ":";
        lbl.Location = new Point(10, yPos);
        panel.Controls.Add(lbl);

        TextBox txt = new TextBox();
        txt.Location = new Point(150, yPos);
        txt.Width = 200;
        panel.Controls.Add(txt);

        yPos += 30;
    }

    settingsPage.Controls.Add(panel);
    wizardContainer1.Controls.Add(settingsPage);
}
```

### Case 3: Data Grid Display

```csharp
// Scrollable data view
public void CreateDataPage(DataTable data)
{
    xpTaskPane1.VerticalScroll = true;
    xpTaskPane1.ScrollSpeed = 25;

    XPTaskPage dataPage = new XPTaskPage();
    dataPage.Title = "Data View";

    DataGridView grid = new DataGridView();
    grid.Dock = DockStyle.Fill;
    grid.DataSource = data;
    grid.AutoResizeColumns();

    dataPage.Controls.Add(grid);
    wizardContainer1.Controls.Add(dataPage);
}
```

## Performance Considerations

### Large Content Pages

```csharp
// For pages with 50+ controls, consider:

// 1. Use scrolling to avoid layout issues
xpTaskPane1.VerticalScroll = true;

// 2. Defer adding controls
var page = new XPTaskPage();
page.Title = "Large List";
// Add controls when page becomes visible

// 3. Use lightweight controls
// Prefer Labels/TextBoxes over complex controls

// 4. Virtualize if possible
// Only add visible controls (if scrolling library available)
```

### Optimal Scroll Speed

```csharp
// Balance between responsiveness and usability
private void SetOptimalScrollSpeed()
{
    // Page height < 300: Use slower speed (10-12)
    if (xpTaskPane1.Height < 300)
        xpTaskPane1.ScrollSpeed = 10;

    // Page height 300-500: Use medium speed (15-20)
    else if (xpTaskPane1.Height < 500)
        xpTaskPane1.ScrollSpeed = 18;

    // Page height > 500: Use faster speed (25-30)
    else
        xpTaskPane1.ScrollSpeed = 25;
}
```

## Scrolling Troubleshooting

**Issue: Content not scrolling**
- Solution: Verify VerticalScroll = true
- Solution: Check that content actually overflows page height
- Solution: Ensure controls are properly added to page

**Issue: Scrolling too fast/slow**
- Solution: Adjust ScrollSpeed property
- Solution: Test with different values (5-50 range)
- Solution: Match speed to content type

**Issue: Controls get cut off**
- Solution: Enable VerticalScroll = true
- Solution: Use panel container with auto-scroll
- Solution: Adjust page height or control sizes

**Issue: Scroll doesn't stop at bottom**
- Solution: Verify all content fits within scroll container
- Solution: Check panel/container size calculations
- Solution: Ensure last control has proper margins

## Scrolling with Custom Containers

```csharp
// Use FlowLayoutPanel for automatic sizing
public void CreateScrollableFlowPage()
{
    xpTaskPane1.VerticalScroll = true;

    XPTaskPage flowPage = new XPTaskPage();
    flowPage.Title = "Flow Layout";

    FlowLayoutPanel flow = new FlowLayoutPanel();
    flow.Dock = DockStyle.Fill;
    flow.FlowDirection = FlowDirection.TopDown;
    flow.WrapContents = false;
    flow.AutoScroll = true; // Built-in scrolling

    // Add items - flow handles layout automatically
    for (int i = 0; i < 30; i++)
    {
        Button btn = new Button();
        btn.Text = "Item " + i;
        btn.Width = 200;
        flow.Controls.Add(btn);
    }

    flowPage.Controls.Add(flow);
    wizardContainer1.Controls.Add(flowPage);
}
```

## Combined Scrolling and Styling

```csharp
private void SetupScrollingAndStyle()
{
    // Enable scrolling
    xpTaskPane1.VerticalScroll = true;
    xpTaskPane1.ScrollSpeed = 20;

    // Set visual style
    xpTaskPane1.VisualStyle = VisualStyle.Office2007;

    // Create styled scrollable page
    XPTaskPage page = new XPTaskPage();
    page.Title = "Styled Scrollable Content";
    page.Font = new Font("Segoe UI", 9F);
    page.ForeColor = Color.FromArgb(64, 64, 64);
    page.BorderStyle = BorderStyle.FixedSingle;

    // Add scrollable content
    Panel contentPanel = new Panel();
    contentPanel.Dock = DockStyle.Fill;
    for (int i = 0; i < 25; i++)
    {
        Label lbl = new Label();
        lbl.Text = "Content Item " + (i + 1);
        lbl.Height = 25;
        lbl.Dock = DockStyle.Top;
        contentPanel.Controls.Add(lbl);
    }

    page.Controls.Add(contentPanel);
    wizardContainer1.Controls.Add(page);
    xpTaskPane1.TaskPages = new XPTaskPage[] { page };
}
```

## Summary

- **Enable scrolling** for pages with overflow content
- **Adjust scroll speed** to match page size and content
- **Test hover behavior** to ensure smooth scrolling
- **Monitor performance** with large content pages
- **Combine with styling** for professional appearance
