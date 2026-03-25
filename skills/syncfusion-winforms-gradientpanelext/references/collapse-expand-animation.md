# Collapse and Expand Animation

Guide to implementing animated collapse/expand functionality in GradientPanelExt for collapsible panel sections.

## Overview

GradientPanelExt supports animated collapse and expand operations when used with a **CollapsePrimitive**. This feature allows panels to smoothly transition between full and collapsed states.

**Requirements:**
- **CollapsePrimitive** must be added to panel's Primitives collection
- Animation properties configure the transition behavior

---

## Animation Properties

### Animated Property

Enables or disables collapse/expand animation.

**Property Type:** `bool`  
**Default Value:** `false` (instant collapse, no animation)

**C# Example:**
```csharp
gradientPanel.Animated = true;   // Enable animation
gradientPanel.Animated = false;  // Disable animation (instant)
```

**VB.NET Example:**
```vb
gradientPanel.Animated = True   ' Enable animation
gradientPanel.Animated = False  ' Disable (instant)
```

---

### AnimationDelay Property

Delay in milliseconds before animation starts.

**Property Type:** `int`  
**Default Value:** `0` (no delay)  
**Unit:** Milliseconds

**C# Example:**
```csharp
gradientPanel.AnimationDelay = 0;    // No delay
gradientPanel.AnimationDelay = 100;  // 100ms delay
gradientPanel.AnimationDelay = 500;  // 500ms delay
```

**VB.NET Example:**
```vb
gradientPanel.AnimationDelay = 0    ' No delay
gradientPanel.AnimationDelay = 100  ' 100ms delay
```

---

### AnimationSpeed Property

Speed factor controlling how fast the animation plays.

**Property Type:** `int`  
**Default Value:** `1` (normal speed)  
**Range:** 1 (slow) to higher values (faster)

**C# Example:**
```csharp
gradientPanel.AnimationSpeed = 1;  // Slow
gradientPanel.AnimationSpeed = 2;  // Normal
gradientPanel.AnimationSpeed = 5;  // Fast
gradientPanel.AnimationSpeed = 10; // Very fast
```

**VB.NET Example:**
```vb
gradientPanel.AnimationSpeed = 1  ' Slow
gradientPanel.AnimationSpeed = 2  ' Normal
gradientPanel.AnimationSpeed = 5  ' Fast
```

**Note:** Higher values = faster animation. Value of 1 is slowest.

---

## Complete Collapse Setup

### Basic Animated Collapse

**C# Example:**
```csharp
// Create panel
GradientPanelExt collapsiblePanel = new GradientPanelExt
{
    Size = new Size(400, 250),
    Location = new Point(30, 30),
    CornerRadius = 10,
    
    // Enable animation
    Animated = true,
    AnimationDelay = 0,
    AnimationSpeed = 3
};

// Gradient background
collapsiblePanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightSteelBlue,
    Color.White
);

// Add collapse primitive
CollapsePrimitive collapseButton = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 350,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.CollapseIcon,
    ExpandImage = Properties.Resources.ExpandIcon,
    BackColor = Color.Transparent
};

collapsiblePanel.Primitives.Add(collapseButton);

this.Controls.Add(collapsiblePanel);
```

**VB.NET Example:**
```vb
' Create panel
Dim collapsiblePanel As New GradientPanelExt With {
    .Size = New Size(400, 250),
    .Location = New Point(30, 30),
    .CornerRadius = 10,
    .Animated = True,
    .AnimationDelay = 0,
    .AnimationSpeed = 3
}

' Gradient background
collapsiblePanel.BackgroundColor = New BrushInfo( _
    GradientStyle.Vertical, _
    Color.LightSteelBlue, _
    Color.White _
)

' Add collapse primitive
Dim collapseButton As New CollapsePrimitive With {
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    .Position = 350,
    .Size = New Size(30, 30),
    .CollapseImage = My.Resources.CollapseIcon,
    .ExpandImage = My.Resources.ExpandIcon,
    .BackColor = Color.Transparent
}

collapsiblePanel.Primitives.Add(collapseButton)

Me.Controls.Add(collapsiblePanel)
```

---

## Animation Examples

### Example 1: Slow Smooth Animation

```csharp
GradientPanelExt slowPanel = new GradientPanelExt
{
    Size = new Size(350, 200),
    Animated = true,
    AnimationDelay = 0,
    AnimationSpeed = 1  // Slowest = smooth
};

slowPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkSeaGreen,
    Color.LightGreen
);

// Add collapse primitive
CollapsePrimitive collapse = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 160,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.UpArrow,
    ExpandImage = Properties.Resources.DownArrow
};

slowPanel.Primitives.Add(collapse);
```

**Best For:** Deliberate, noticeable transitions, emphasis on movement

---

### Example 2: Fast Snappy Animation

```csharp
GradientPanelExt fastPanel = new GradientPanelExt
{
    Size = new Size(350, 200),
    Animated = true,
    AnimationDelay = 0,
    AnimationSpeed = 8  // Fast
};

fastPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.SteelBlue,
    Color.LightBlue
);

// Add collapse primitive
CollapsePrimitive collapse = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 300,
    Size = new Size(25, 25),
    CollapseImage = Properties.Resources.Minus,
    ExpandImage = Properties.Resources.Plus
};

fastPanel.Primitives.Add(collapse);
```

**Best For:** Quick, responsive feel, minimal distraction

---

### Example 3: Delayed Animation

```csharp
GradientPanelExt delayedPanel = new GradientPanelExt
{
    Size = new Size(350, 200),
    Animated = true,
    AnimationDelay = 300,  // 300ms delay before animation
    AnimationSpeed = 3
};

delayedPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.Plum,
    Color.Lavender
);

// Add collapse primitive
CollapsePrimitive collapse = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 310,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.CollapseIcon,
    ExpandImage = Properties.Resources.ExpandIcon
};

delayedPanel.Primitives.Add(collapse);
```

**Best For:** Preventing accidental clicks, intentional delay for user feedback

---

### Example 4: No Animation (Instant)

```csharp
GradientPanelExt instantPanel = new GradientPanelExt
{
    Size = new Size(350, 200),
    Animated = false  // No animation = instant collapse
};

instantPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.IndianRed,
    Color.LightCoral
);

// Add collapse primitive
CollapsePrimitive collapse = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 310,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.Collapse,
    ExpandImage = Properties.Resources.Expand
};

instantPanel.Primitives.Add(collapse);
```

**Best For:** Performance-critical scenarios, simple toggling without visual effect

---

## Complete Dashboard Example

**Collapsible dashboard section with smooth animation:**

```csharp
// Create collapsible section
GradientPanelExt dashboardSection = new GradientPanelExt
{
    Size = new Size(600, 300),
    Location = new Point(20, 20),
    CornerRadius = 10,
    
    // Smooth animation
    Animated = true,
    AnimationDelay = 0,
    AnimationSpeed = 4
};

// Professional gradient
dashboardSection.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(245, 245, 245),
    Color.White
);

// Section title
TextPrimitive title = new TextPrimitive
{
    Text = "Sales Analytics",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 20,
    Size = new Size(180, 35),
    TextFont = new Font("Segoe UI", 14, FontStyle.Bold),
    TextColor = Color.DarkSlateGray,
    BackColor = Color.Transparent
};

// Collapse button
CollapsePrimitive collapseBtn = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 550,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.MinusIcon,
    ExpandImage = Properties.Resources.PlusIcon,
    BackColor = Color.Transparent
};

dashboardSection.Primitives.AddRange(new Primitive[] { title, collapseBtn });

// Add content controls inside panel
Label lblRevenue = new Label
{
    Text = "Total Revenue: $125,450",
    Location = new Point(30, 60),
    Font = new Font("Arial", 12),
    AutoSize = true,
    BackColor = Color.Transparent
};

Label lblOrders = new Label
{
    Text = "Orders: 1,234",
    Location = new Point(30, 100),
    Font = new Font("Arial", 12),
    AutoSize = true,
    BackColor = Color.Transparent
};

dashboardSection.Controls.AddRange(new Control[] { lblRevenue, lblOrders });

this.Controls.Add(dashboardSection);
```

---

## Best Practices

### 1. Choose Appropriate Speed

```csharp
// Subtle content: Slow, smooth
gradientPanel.AnimationSpeed = 1;  // Slow

// Standard UI: Moderate
gradientPanel.AnimationSpeed = 3;  // Balanced

// Fast-paced app: Quick
gradientPanel.AnimationSpeed = 7;  // Fast
```

### 2. Match Animation to Application Style

```csharp
// Professional business app: Smooth, moderate
panel.Animated = true;
panel.AnimationSpeed = 2;

// Gaming/entertainment: Fast, snappy
panel.Animated = true;
panel.AnimationSpeed = 8;

// Performance-critical: No animation
panel.Animated = false;
```

### 3. Use Delay Sparingly

```csharp
// Typically use 0 delay for immediate response
panel.AnimationDelay = 0;

// Only use delay if preventing accidental clicks
panel.AnimationDelay = 200;  // Rare use case
```

### 4. Provide Visual Feedback

```csharp
// Use clear collapse/expand icons
collapseBtn.CollapseImage = Properties.Resources.UpArrow;    // Clearly "minimize"
collapseBtn.ExpandImage = Properties.Resources.DownArrow;    // Clearly "maximize"

// Or use minus/plus
collapseBtn.CollapseImage = Properties.Resources.MinusIcon;
collapseBtn.ExpandImage = Properties.Resources.PlusIcon;
```

---

## Animation Behavior

### Collapse Sequence

1. User clicks CollapsePrimitive
2. AnimationDelay wait period (if > 0)
3. Panel height smoothly reduces at AnimationSpeed rate
4. Panel reaches collapsed height (primitives + minimal height)
5. CollapseImage swaps to ExpandImage

### Expand Sequence

1. User clicks CollapsePrimitive (now showing ExpandImage)
2. AnimationDelay wait period (if > 0)
3. Panel height smoothly increases at AnimationSpeed rate
4. Panel returns to original full height
5. ExpandImage swaps back to CollapseImage

### Collapsed Height

**What's visible when collapsed:**
- Top border + primitives in top border
- Bottom border + primitives in bottom border
- Left/Right borders (if primitives exist there)
- Main content area is hidden

---

## Troubleshooting

### Animation Not Working

**Check:**
1. **Animated** property is `true`
2. **CollapsePrimitive** is added to Primitives collection
3. CollapsePrimitive has both **CollapseImage** and **ExpandImage** set
4. Images are loaded correctly (not null)

```csharp
// Verify animation settings
System.Diagnostics.Debug.WriteLine($"Animated: {panel.Animated}");
System.Diagnostics.Debug.WriteLine($"Speed: {panel.AnimationSpeed}");
System.Diagnostics.Debug.WriteLine($"Primitives Count: {panel.Primitives.Count}");
```

### Animation Too Fast or Too Slow

**Solution:** Adjust AnimationSpeed

```csharp
// Too fast? Decrease speed
panel.AnimationSpeed = 2;  // Slower

// Too slow? Increase speed
panel.AnimationSpeed = 6;  // Faster
```

### Panel Not Collapsing at All

**Check:**
- CollapsePrimitive is correctly configured
- CollapsePrimitive images are set
- Click event is firing (test by handling primitive click)

```csharp
// Test if primitive exists
bool hasCollapsePrimitive = panel.Primitives.OfType<CollapsePrimitive>().Any();
Debug.WriteLine($"Has CollapsePrimitive: {hasCollapsePrimitive}");
```

### Delay Too Long

**Solution:** Reduce or remove AnimationDelay

```csharp
panel.AnimationDelay = 0;  // Immediate response
```

---

## Performance Considerations

- **Animation = true**: Uses slightly more CPU during collapse/expand
- **Animation = false**: Instant, no CPU overhead
- **AnimationSpeed**: Higher values complete faster, less total CPU time
- **For many panels**: Consider disabling animation if performance critical

---

## Related Topics

- **Primitives**: CollapsePrimitive setup → [primitives.md](primitives.md)
- **Getting Started**: Basic panel setup → [getting-started.md](getting-started.md)
- **Events**: Handling collapse events → [scroll-settings-events.md](scroll-settings-events.md)
