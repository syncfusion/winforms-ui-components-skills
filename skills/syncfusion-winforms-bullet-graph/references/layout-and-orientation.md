# Layout and Orientation in Windows Forms Bullet Graph

## Table of Contents
- [Overview](#overview)
- [Orientation](#orientation)
- [Flow Direction](#flow-direction)
- [Caption](#caption)
- [Quantitative Scale Length](#quantitative-scale-length)
- [Layout Properties](#layout-properties)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Bullet Graph's layout and orientation settings control how the graph is displayed and oriented within your application. These settings determine the visual flow, caption placement, and overall presentation of the component.

**Key Configuration Areas:**
- **Orientation**: Horizontal or vertical display
- **Flow Direction**: Forward or backward progression
- **Caption**: Label and positioning
- **Scale Length**: Customizable quantitative scale size
- **Layout Properties**: Docking and sizing

## Orientation

The `Orientation` property controls whether the bullet graph displays horizontally or vertically.

### Available Orientations

- **Horizontal**: Graph flows left-to-right (default)
- **Vertical**: Graph flows top-to-bottom or bottom-to-top

### Horizontal Orientation

Horizontal is the **default and most common** orientation for bullet graphs.

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.Orientation = Orientation.Horizontal;  // Default
bullet.FlowDirection = BulletGraphFlowDirection.Forward;

bullet.FeaturedMeasure = 5;
bullet.ComparativeMeasure = 7;

bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });

this.Controls.Add(bullet);
```

**Characteristics:**
- Scale runs left-to-right
- Caption typically on left or right side
- Most natural for reading in Western cultures
- Best for wide displays

**Use Cases:**
- Standard dashboard layouts
- Multiple graphs stacked vertically
- Wide screen displays
- Revenue/sales tracking

### Vertical Orientation

Vertical orientation rotates the graph 90 degrees.

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Left;
bullet.Width = 150;
bullet.Orientation = Orientation.Vertical;
bullet.FlowDirection = BulletGraphFlowDirection.Forward;

bullet.FeaturedMeasure = 5;
bullet.ComparativeMeasure = 7;

bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });

this.Controls.Add(bullet);
```

**Characteristics:**
- Scale runs bottom-to-top or top-to-bottom (depending on flow direction)
- Caption typically on top or bottom
- Good for tall, narrow spaces
- Less common but effective in certain layouts

**Use Cases:**
- Sidebar displays
- Multiple graphs arranged horizontally
- Tall screen displays
- Temperature or gauge-like displays

## Flow Direction

The `FlowDirection` property controls the direction of value progression on the scale.

### Available Flow Directions

- **Forward**: Values increase from left-to-right (horizontal) or bottom-to-top (vertical)
- **Backward**: Values increase from right-to-left (horizontal) or top-to-bottom (vertical)

### Forward Flow Direction

**Forward** is the standard, most intuitive direction.

```csharp
BulletGraph bullet = new BulletGraph();
bullet.FlowDirection = BulletGraphFlowDirection.Forward;
bullet.Orientation = Orientation.Horizontal;

// Scale: 0 on left → 10 on right
bullet.Minimum = 0;
bullet.Maximum = 10;
```

**Horizontal + Forward**: 0 on left, maximum on right (→)  
**Vertical + Forward**: 0 on bottom, maximum on top (↑)

### Backward Flow Direction

**Backward** reverses the scale direction.

```csharp
BulletGraph bullet = new BulletGraph();
bullet.FlowDirection = BulletGraphFlowDirection.Backward;
bullet.Orientation = Orientation.Horizontal;

// Scale: 0 on right → 10 on left
bullet.Minimum = 0;
bullet.Maximum = 10;
```

**Horizontal + Backward**: 0 on right, maximum on left (←)  
**Vertical + Backward**: 0 on top, maximum on bottom (↓)

### Orientation and Flow Direction Combinations

```csharp
// Example showing all four combinations
void CreateOrientationExamples(Form form)
{
    // 1. Horizontal + Forward (most common)
    BulletGraph hf = new BulletGraph();
    hf.Orientation = Orientation.Horizontal;
    hf.FlowDirection = BulletGraphFlowDirection.Forward;
    hf.Dock = DockStyle.Top;
    hf.Height = 80;
    hf.Caption = "Horizontal + Forward";
    ConfigureBullet(hf);
    form.Controls.Add(hf);
    
    // 2. Horizontal + Backward
    BulletGraph hb = new BulletGraph();
    hb.Orientation = Orientation.Horizontal;
    hb.FlowDirection = BulletGraphFlowDirection.Backward;
    hb.Dock = DockStyle.Top;
    hb.Height = 80;
    hb.Caption = "Horizontal + Backward";
    ConfigureBullet(hb);
    form.Controls.Add(hb);
    
    // 3. Vertical + Forward
    BulletGraph vf = new BulletGraph();
    vf.Orientation = Orientation.Vertical;
    vf.FlowDirection = BulletGraphFlowDirection.Forward;
    vf.Dock = DockStyle.Left;
    vf.Width = 150;
    vf.Caption = "Vertical + Forward";
    ConfigureBullet(vf);
    form.Controls.Add(vf);
    
    // 4. Vertical + Backward
    BulletGraph vb = new BulletGraph();
    vb.Orientation = Orientation.Vertical;
    vb.FlowDirection = BulletGraphFlowDirection.Backward;
    vb.Dock = DockStyle.Left;
    vb.Width = 150;
    vb.Caption = "Vertical + Backward";
    ConfigureBullet(vb);
    form.Controls.Add(vb);
}

void ConfigureBullet(BulletGraph bullet)
{
    bullet.Minimum = 0;
    bullet.Maximum = 10;
    bullet.Interval = 2;
    bullet.FeaturedMeasure = 6;
    bullet.ComparativeMeasure = 8;
    
    bullet.QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 4, RangeStroke = Color.LightCoral });
    bullet.QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 7, RangeStroke = Color.LightYellow });
    bullet.QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 10, RangeStroke = Color.LightGreen });
}
```

## Caption

The `Caption` property provides a text label that describes what the bullet graph represents.

### Setting Caption

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Caption = "Revenue YTD\n$ in thousands";
```

**Caption Features:**
- Supports multi-line text using `\n`
- Positioned at start or end of scale
- Important for identifying the metric

### Caption Position

The `CaptionPosition` property controls where the caption appears relative to the scale.

**Available Positions:**
- **Near** (Default): At the start of the scale
- **Far**: At the end of the scale

#### Near Position (Default)

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.Caption = "Revenue YTD\n$ in thousands";
bullet.CaptionPosition = BulletGraphCaptionPosition.Near;  // Default

bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });

this.Controls.Add(bullet);
```

**Near Position Behavior:**
- **Horizontal**: Caption on left side
- **Vertical**: Caption on bottom

#### Far Position

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.Caption = "Revenue YTD\n$ in thousands";
bullet.CaptionPosition = BulletGraphCaptionPosition.Far;

bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });

this.Controls.Add(bullet);
```

**Far Position Behavior:**
- **Horizontal**: Caption on right side
- **Vertical**: Caption on top

### Multi-Line Captions

Use `\n` for multi-line captions:

```csharp
// Single line
bullet.Caption = "Revenue YTD ($ in thousands)";

// Multi-line (better formatting)
bullet.Caption = "Revenue YTD\n$ in thousands";

// Multiple lines
bullet.Caption = "Q4 2024\nRevenue\n$ Thousands";
```

## Quantitative Scale Length

The `QuantitativeScaleLength` property customizes the length of the quantitative scale.

```csharp
BulletGraph bullet = new BulletGraph();
bullet.FlowDirection = BulletGraphFlowDirection.Forward;
bullet.Orientation = Orientation.Horizontal;
bullet.QuantitativeScaleLength = 600;  // 600 pixels long

bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });
```

**Usage Guidelines:**
- Typically only needed when not using `Dock` property
- Explicit size control for specific layouts
- Combined with caption and other elements

## Layout Properties

### Dock Property

The `Dock` property controls how the bullet graph fills its parent container.

```csharp
// Fill entire parent
bullet.Dock = DockStyle.Fill;

// Dock to top, fixed height
bullet.Dock = DockStyle.Top;
bullet.Height = 100;

// Dock to left, fixed width
bullet.Dock = DockStyle.Left;
bullet.Width = 150;

// Dock to bottom
bullet.Dock = DockStyle.Bottom;
bullet.Height = 80;
```

**Common Patterns:**
- **Horizontal graphs**: Use `DockStyle.Top` or `DockStyle.Fill`
- **Vertical graphs**: Use `DockStyle.Left` or `DockStyle.Fill`

### Size and Location

When not using `Dock`, set explicit size and position:

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Size = new Size(600, 100);
bullet.Location = new Point(20, 20);

// Or set individually
bullet.Width = 600;
bullet.Height = 100;
bullet.Left = 20;
bullet.Top = 20;
```

## Common Patterns

### Standard Horizontal Dashboard

```csharp
void CreateHorizontalDashboard(Panel container)
{
    string[] metrics = { "Revenue", "Profit", "Customers" };
    double[] actuals = { 275, 45, 1250 };
    double[] targets = { 300, 50, 1500 };
    
    for (int i = 0; i < metrics.Length; i++)
    {
        BulletGraph bullet = new BulletGraph();
        bullet.Dock = DockStyle.Top;
        bullet.Height = 80;
        
        // Standard horizontal layout
        bullet.Orientation = Orientation.Horizontal;
        bullet.FlowDirection = BulletGraphFlowDirection.Forward;
        
        // Caption on left
        bullet.Caption = $"{metrics[i]}\nYTD";
        bullet.CaptionPosition = BulletGraphCaptionPosition.Near;
        
        bullet.FeaturedMeasure = actuals[i];
        bullet.ComparativeMeasure = targets[i];
        
        container.Controls.Add(bullet);
    }
}
```

### Vertical Sidebar Display

```csharp
void CreateVerticalSidebar(Panel sidebar)
{
    BulletGraph performance = new BulletGraph();
    performance.Dock = DockStyle.Left;
    performance.Width = 120;
    
    // Vertical layout
    performance.Orientation = Orientation.Vertical;
    performance.FlowDirection = BulletGraphFlowDirection.Backward;  // Top to bottom
    
    // Caption at bottom
    performance.Caption = "Performance\nScore";
    performance.CaptionPosition = BulletGraphCaptionPosition.Far;
    
    performance.Minimum = 0;
    performance.Maximum = 100;
    performance.FeaturedMeasure = 75;
    performance.ComparativeMeasure = 90;
    
    sidebar.Controls.Add(performance);
}
```

### Full-Width Display

```csharp
BulletGraph fullWidth = new BulletGraph();
fullWidth.Dock = DockStyle.Fill;  // Fill entire container

fullWidth.Orientation = Orientation.Horizontal;
fullWidth.FlowDirection = BulletGraphFlowDirection.Forward;

fullWidth.Caption = "Annual Revenue\n$ in millions";
fullWidth.CaptionPosition = BulletGraphCaptionPosition.Near;

fullWidth.Minimum = 0;
fullWidth.Maximum = 10;
fullWidth.Interval = 2;
fullWidth.FeaturedMeasure = 6.5;
fullWidth.ComparativeMeasure = 8.0;
```

### Compact Multi-Graph Layout

```csharp
void CreateCompactLayout(FlowLayoutPanel container)
{
    // Horizontal graphs with consistent sizing
    string[] kpis = { "Sales", "Profit", "Growth", "Satisfaction" };
    
    foreach (string kpi in kpis)
    {
        BulletGraph bullet = new BulletGraph();
        bullet.Size = new Size(400, 60);  // Fixed size
        bullet.Margin = new Padding(10);
        
        bullet.Orientation = Orientation.Horizontal;
        bullet.FlowDirection = BulletGraphFlowDirection.Forward;
        bullet.Caption = kpi;
        
        container.Controls.Add(bullet);
    }
}
```

## Best Practices

### Orientation Selection

**Choose Horizontal when:**
- Building standard dashboards
- Stacking multiple graphs vertically
- Wide display areas available
- Following common dashboard conventions

**Choose Vertical when:**
- Creating sidebar displays
- Arranging graphs horizontally
- Tall, narrow spaces
- Mimicking gauge-like displays

### Flow Direction Guidelines

**Use Forward (default) for:**
- Most standard scenarios
- Left-to-right or bottom-to-top progression
- Intuitive value increase direction

**Use Backward for:**
- Specific design requirements
- Right-to-left locales
- Matching existing visual patterns

### Caption Best Practices

```csharp
// Good: Clear, concise, multi-line
bullet.Caption = "Revenue YTD\n$ in thousands";

// Good: Includes units
bullet.Caption = "Temperature\n°C";

// Avoid: Too verbose
bullet.Caption = "This graph shows the current year-to-date revenue in thousands of dollars";

// Avoid: Missing units
bullet.Caption = "Revenue";  // What units? What period?
```

### Layout Consistency

Maintain consistent sizing across related graphs:

```csharp
void CreateConsistentDashboard(Panel container)
{
    int standardHeight = 80;
    
    for (int i = 0; i < 5; i++)
    {
        BulletGraph bullet = new BulletGraph();
        bullet.Dock = DockStyle.Top;
        bullet.Height = standardHeight;  // Consistent
        bullet.Orientation = Orientation.Horizontal;
        
        container.Controls.Add(bullet);
    }
}
```

## Troubleshooting

### Issue: Graph Not Visible

**Problem**: Bullet graph doesn't appear in the form.

**Solution**: Ensure size is set (via Dock or explicit Size):
```csharp
// Option 1: Use Dock
bullet.Dock = DockStyle.Fill;

// Option 2: Set explicit size
bullet.Size = new Size(600, 100);
```

### Issue: Caption Cut Off

**Problem**: Multi-line caption text is truncated.

**Solution**: Increase control size or adjust layout:
```csharp
bullet.Dock = DockStyle.Top;
bullet.Height = 100;  // Increase height for multi-line captions
```

### Issue: Wrong Flow Direction

**Problem**: Scale values appear reversed.

**Solution**: Adjust `FlowDirection`:
```csharp
// If values are reversed, switch direction
bullet.FlowDirection = BulletGraphFlowDirection.Backward;
```

### Issue: Vertical Graph Too Narrow

**Problem**: Vertical bullet graph appears squeezed.

**Solution**: Set adequate width:
```csharp
bullet.Orientation = Orientation.Vertical;
bullet.Dock = DockStyle.Left;
bullet.Width = 150;  // Ensure sufficient width
```

### Issue: Caption in Wrong Position

**Problem**: Caption appears on the wrong side.

**Solution**: Adjust `CaptionPosition`:
```csharp
// Move caption to other end
bullet.CaptionPosition = BulletGraphCaptionPosition.Far;
```

### Issue: Inconsistent Sizing in Dashboard

**Problem**: Multiple bullet graphs have different sizes.

**Solution**: Use consistent docking and sizing:
```csharp
foreach (BulletGraph bullet in bullets)
{
    bullet.Dock = DockStyle.Top;
    bullet.Height = 80;  // Consistent height
}
```