# Orientation and Tooltips - WinForms Rating Control

This reference covers orientation configuration (horizontal and vertical layouts) and tooltip functionality for displaying rating values.

## Orientation Overview

The Rating control supports two orientation modes that determine how rating items are arranged:

- **Horizontal** (Default) - Items arranged left-to-right
- **Vertical** - Items arranged top-to-bottom

## Orientation Property

The `Orientation` property uses the `Orientationmode` enum from the `Syncfusion.Windows.Forms.Tools` namespace.

```csharp
using Syncfusion.Windows.Forms.Tools;

// Horizontal orientation (default)
ratingControl1.Orientation = Orientationmode.Horizontal;

// Vertical orientation
ratingControl1.Orientation = Orientationmode.Vertical;
```

## Horizontal Orientation

### Default Behavior

Horizontal orientation is the default layout, arranging rating items from left to right. This is the most common rating presentation format.

### Horizontal Orientation Example

```csharp
private void CreateHorizontalRating()
{
    var horizontalRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),  // Width > Height for horizontal
        Orientation = Orientationmode.Horizontal,
        Shape = Shapes.Star,
        Value = 4
    };

    this.Controls.Add(horizontalRating);
}
```

### Size Considerations for Horizontal

For optimal horizontal display:
- **Width**: Should accommodate all rating items (typically 150-300 pixels for 5 items)
- **Height**: Should fit one item height (typically 30-50 pixels)
- **Aspect Ratio**: Width should be significantly larger than height

```csharp
// Good sizing for horizontal orientation
ratingControl1.Size = new Size(200, 40);  // 5:1 ratio
ratingControl1.Size = new Size(250, 50);  // 5:1 ratio
```

### When to Use Horizontal Orientation

- **Forms and surveys** - Traditional rating input in forms
- **Product reviews** - E-commerce and review systems
- **Dashboard metrics** - Horizontal space is available
- **Inline ratings** - Within text or table rows
- **Default use case** - When in doubt, use horizontal

## Vertical Orientation

### Vertical Layout

Vertical orientation arranges rating items from top to bottom, useful for sidebar layouts or vertical UI designs.

### Vertical Orientation Example

```csharp
private void CreateVerticalRating()
{
    var verticalRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(40, 200),  // Height > Width for vertical
        Orientation = Orientationmode.Vertical,
        Shape = Shapes.Star,
        Value = 3
    };

    this.Controls.Add(verticalRating);
}
```

### Size Considerations for Vertical

For optimal vertical display:
- **Width**: Should fit one item width (typically 30-50 pixels)
- **Height**: Should accommodate all rating items (typically 150-300 pixels for 5 items)
- **Aspect Ratio**: Height should be significantly larger than width

```csharp
// Good sizing for vertical orientation
ratingControl1.Size = new Size(40, 200);  // 1:5 ratio
ratingControl1.Size = new Size(50, 250);  // 1:5 ratio
```

### When to Use Vertical Orientation

- **Sidebar layouts** - Side panels with limited width
- **Vertical navigation** - Alongside vertical menus
- **Space-constrained horizontal layouts** - When width is limited
- **Mobile portrait mode** - Vertical scrolling interfaces
- **Artistic/creative layouts** - Unique design requirements

## Layout Considerations

### Adjusting Control Size for Orientation

When switching orientation, adjust the control size accordingly:

```csharp
private void SwitchOrientation(Orientationmode newOrientation)
{
    if (newOrientation == Orientationmode.Horizontal)
    {
        // Switch to horizontal: wide and short
        ratingControl1.Orientation = Orientationmode.Horizontal;
        ratingControl1.Size = new Size(200, 40);
    }
    else
    {
        // Switch to vertical: narrow and tall
        ratingControl1.Orientation = Orientationmode.Vertical;
        ratingControl1.Size = new Size(40, 200);
    }
}
```

### Dynamic Orientation Based on Container

```csharp
private void AdjustOrientationToContainer(Panel container)
{
    // Use vertical orientation if container is taller than wide
    if (container.Height > container.Width)
    {
        ratingControl1.Orientation = Orientationmode.Vertical;
        ratingControl1.Size = new Size(
            Math.Min(50, container.Width - 10),
            container.Height - 10
        );
    }
    else
    {
        ratingControl1.Orientation = Orientationmode.Horizontal;
        ratingControl1.Size = new Size(
            container.Width - 10,
            Math.Min(50, container.Height - 10)
        );
    }
}
```

## Tooltip Functionality

Tooltips display the current rating value when users hover over the rating control, providing immediate feedback.

### Enabling Tooltips

Use the `ShowTooltip` property to enable or disable tooltips:

```csharp
// Enable tooltips
ratingControl1.ShowTooltip = true;

// Disable tooltips
ratingControl1.ShowTooltip = false;
```

### Default Tooltip Behavior

When `ShowTooltip` is true:
- Hovering over the control displays the current rating value
- The tooltip updates as the user moves the mouse over different rating items
- The tooltip automatically follows the mouse cursor

### Basic Tooltip Example

```csharp
private void CreateRatingWithTooltip()
{
    var ratingWithTooltip = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        Shape = Shapes.Star,
        Value = 4,
        ShowTooltip = true,  // Enable tooltips
        Precision = PrecisionMode.Half
    };

    this.Controls.Add(ratingWithTooltip);
    // Hovering displays: "4.0" or "4.5" depending on position
}
```

## Tooltip Appearance Customization

### Default Tooltip Format

By default, the tooltip displays the rating value as a decimal number (e.g., "3.5", "4.0").

### Customizing Tooltip Content

While the Rating control displays values by default, you can enhance the user experience by adding contextual labels:

```csharp
private void CreateDescriptiveRatingWithLabel()
{
    // Create a label to provide context
    Label lblRatingContext = new Label
    {
        Text = "Quality Rating:",
        Location = new Point(50, 25),
        AutoSize = true
    };

    var rating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        Value = 4,
        ShowTooltip = true,  // Shows "4.0" on hover
        Precision = PrecisionMode.Half
    };

    // Add descriptive feedback label
    Label lblCurrentValue = new Label
    {
        Location = new Point(260, 55),
        AutoSize = true,
        Text = $"{rating.Value:F1} out of 5"
    };

    // Update feedback on value change
    rating.RatingChanged += (s, e) =>
    {
        lblCurrentValue.Text = $"{rating.Value:F1} out of 5";
    };

    this.Controls.Add(lblRatingContext);
    this.Controls.Add(rating);
    this.Controls.Add(lblCurrentValue);
}
```

### Tooltip with Precision Modes

Tooltips automatically adjust to the precision mode:

```csharp
// Standard precision - tooltip shows whole numbers (1, 2, 3, 4, 5)
ratingControl1.Precision = PrecisionMode.Standard;
ratingControl1.ShowTooltip = true;

// Half precision - tooltip shows half increments (1.5, 2.0, 2.5, 3.0)
ratingControl2.Precision = PrecisionMode.Half;
ratingControl2.ShowTooltip = true;
```

## Combined Orientation and Tooltip Examples

### Example 1: Sidebar Rating with Tooltip

```csharp
private void CreateSidebarRating()
{
    Panel sidebar = new Panel
    {
        Location = new Point(10, 10),
        Size = new Size(80, 400),
        BackColor = Color.FromArgb(240, 240, 240),
        BorderStyle = BorderStyle.FixedSingle
    };

    Label lblTitle = new Label
    {
        Text = "Rate This",
        Location = new Point(10, 10),
        AutoSize = true,
        Font = new Font("Segoe UI", 9, FontStyle.Bold)
    };

    var verticalRating = new RatingControl
    {
        Location = new Point(20, 40),
        Size = new Size(40, 200),
        Orientation = Orientationmode.Vertical,
        Shape = Shapes.Star,
        Precision = PrecisionMode.Half,
        Value = 3.5f,
        ShowTooltip = true,
        VisualStyle = RatingControl.Style.Office2016White
    };

    Label lblValue = new Label
    {
        Location = new Point(10, 250),
        AutoSize = true,
        Text = "3.5/5"
    };

    verticalRating.RatingChanged += (s, e) =>
    {
        lblValue.Text = $"{verticalRating.Value:F1}/5";
    };

    sidebar.Controls.Add(lblTitle);
    sidebar.Controls.Add(verticalRating);
    sidebar.Controls.Add(lblValue);
    this.Controls.Add(sidebar);
}
```

### Example 2: Horizontal Rating in Form with Tooltip

```csharp
private void CreateFormRating()
{
    // Product name label
    Label lblProduct = new Label
    {
        Text = "Product Quality:",
        Location = new Point(20, 25),
        AutoSize = true
    };

    // Horizontal rating with tooltip
    var productRating = new RatingControl
    {
        Location = new Point(150, 20),
        Size = new Size(220, 45),
        Orientation = Orientationmode.Horizontal,
        Shape = Shapes.Star,
        Precision = PrecisionMode.Half,
        Value = 0,
        ShowTooltip = true,
        ItemSelectionColor = Color.Gold,
        ApplyGradientColors = false
    };

    // Feedback label
    Label lblFeedback = new Label
    {
        Location = new Point(380, 25),
        AutoSize = true,
        ForeColor = Color.Gray,
        Text = "Not rated"
    };

    // Update feedback on rating change
    productRating.RatingChanged += (s, e) =>
    {
        if (productRating.Value == 0)
            lblFeedback.Text = "Not rated";
        else if (productRating.Value <= 2)
            lblFeedback.Text = "Poor";
        else if (productRating.Value <= 3)
            lblFeedback.Text = "Average";
        else if (productRating.Value <= 4)
            lblFeedback.Text = "Good";
        else
            lblFeedback.Text = "Excellent";
    };

    this.Controls.Add(lblProduct);
    this.Controls.Add(productRating);
    this.Controls.Add(lblFeedback);
}
```

### Example 3: Responsive Orientation with Tooltip

```csharp
private Panel containerPanel;
private RatingControl responsiveRating;

private void CreateResponsiveRating()
{
    // Container panel
    containerPanel = new Panel
    {
        Location = new Point(50, 50),
        Size = new Size(300, 300),
        BorderStyle = BorderStyle.FixedSingle,
        BackColor = Color.White
    };

    // Rating control
    responsiveRating = new RatingControl
    {
        Shape = Shapes.Heart,
        Precision = PrecisionMode.Half,
        Value = 4,
        ShowTooltip = true,
        VisualStyle = RatingControl.Style.Office2016Colorful
    };

    // Resize button
    Button btnToggleSize = new Button
    {
        Text = "Toggle Container Size",
        Location = new Point(50, 370),
        AutoSize = true
    };
    btnToggleSize.Click += (s, e) =>
    {
        // Toggle between wide and tall container
        if (containerPanel.Width > containerPanel.Height)
        {
            containerPanel.Size = new Size(100, 300);
        }
        else
        {
            containerPanel.Size = new Size(300, 100);
        }
        AdjustRatingOrientation();
    };

    containerPanel.Controls.Add(responsiveRating);
    this.Controls.Add(containerPanel);
    this.Controls.Add(btnToggleSize);

    AdjustRatingOrientation();
}

private void AdjustRatingOrientation()
{
    if (containerPanel.Width > containerPanel.Height)
    {
        // Wide container - use horizontal orientation
        responsiveRating.Orientation = Orientationmode.Horizontal;
        responsiveRating.Size = new Size(
            containerPanel.Width - 20,
            40
        );
        responsiveRating.Location = new Point(10, 
            (containerPanel.Height - 40) / 2);
    }
    else
    {
        // Tall container - use vertical orientation
        responsiveRating.Orientation = Orientationmode.Vertical;
        responsiveRating.Size = new Size(
            40,
            containerPanel.Height - 20
        );
        responsiveRating.Location = new Point(
            (containerPanel.Width - 40) / 2, 10);
    }
}
```

## Use Cases by Orientation

### Horizontal Orientation Use Cases

1. **Product Review Forms**
```csharp
// Standard product review layout
var reviewRating = new RatingControl
{
    Orientation = Orientationmode.Horizontal,
    Size = new Size(200, 40),
    ShowTooltip = true
};
```

2. **Survey Questions**
```csharp
// Survey question with horizontal rating
Label question = new Label { Text = "How satisfied are you?" };
var surveyRating = new RatingControl
{
    Orientation = Orientationmode.Horizontal,
    Size = new Size(250, 45),
    Precision = PrecisionMode.Half,
    ShowTooltip = true
};
```

3. **Dashboard Metrics**
```csharp
// Horizontal rating in dashboard
var metricRating = new RatingControl
{
    Orientation = Orientationmode.Horizontal,
    Size = new Size(180, 35),
    ShowTooltip = true,
    VisualStyle = RatingControl.Style.Office2016White
};
```

### Vertical Orientation Use Cases

1. **Sidebar Navigation**
```csharp
// Vertical rating in sidebar
var sidebarRating = new RatingControl
{
    Orientation = Orientationmode.Vertical,
    Size = new Size(45, 220),
    ShowTooltip = true
};
```

2. **Space-Constrained Layouts**
```csharp
// Vertical rating when horizontal space is limited
var compactRating = new RatingControl
{
    Orientation = Orientationmode.Vertical,
    Size = new Size(40, 200),
    ShowTooltip = true
};
```

3. **Artistic/Creative Interfaces**
```csharp
// Vertical rating for unique design
var creativeRating = new RatingControl
{
    Orientation = Orientationmode.Vertical,
    Size = new Size(50, 250),
    Shape = Shapes.Heart,
    ShowTooltip = true,
    VisualStyle = RatingControl.Style.Office2016Colorful
};
```

## Best Practices

### Orientation Best Practices

1. **Match Container Shape**: Use horizontal for wide containers, vertical for tall containers
2. **Consistent Direction**: Maintain consistent orientation within the same UI section
3. **Adequate Spacing**: Ensure control size accommodates all rating items comfortably
4. **User Expectation**: Horizontal is more familiar; use vertical only when beneficial

### Tooltip Best Practices

1. **Always Enable for Precision**: Enable tooltips when using Half precision for clarity
2. **Provide Context**: Add labels to explain what the rating represents
3. **Show Current Value**: Display the current rating value alongside the control
4. **Accessibility**: Tooltips aid users with visual impairments when combined with screen readers

## Troubleshooting

### Issue: Rating items are cut off

**Cause:** Control size is too small for the orientation.

**Solution:**
```csharp
// For horizontal, ensure width is adequate
ratingControl1.Orientation = Orientationmode.Horizontal;
ratingControl1.Size = new Size(200, 40);  // Minimum width: 150

// For vertical, ensure height is adequate
ratingControl1.Orientation = Orientationmode.Vertical;
ratingControl1.Size = new Size(40, 200);  // Minimum height: 150
```

### Issue: Tooltip not appearing

**Cause:** `ShowTooltip` is false or control is disabled.

**Solution:**
```csharp
// Ensure tooltip is enabled
ratingControl1.ShowTooltip = true;
ratingControl1.Enabled = true;
```

### Issue: Tooltip displays incorrect value

**Cause:** Tooltip shows the hover position, not the current rating.

**Expected Behavior:** Tooltip displays the value that would be set if clicked at the current hover position, not the current rating value. This is by design to show preview feedback.

**Solution:** Add a separate label to display the current rating value:
```csharp
Label lblCurrentRating = new Label
{
    Text = $"Current: {ratingControl1.Value:F1}",
    Location = new Point(ratingControl1.Right + 10, ratingControl1.Top + 10)
};
```
