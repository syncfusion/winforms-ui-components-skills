# Custom Renderers

## Table of Contents
- [Overview](#overview)
- [IRadialGaugeRenderer Interface](#iradialgaugerenderer-interface)
- [ILinearGaugeRenderer Interface](#ilineargaugerenderer-interface)
- [Custom RadialGauge Renderer Example](#custom-radialgauge-renderer-example)
- [Custom LinearGauge Renderer Example](#custom-lineargauge-renderer-example)
- [Advanced Customization Techniques](#advanced-customization-techniques)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)

## Overview

Custom renderers provide **complete control over gauge appearance** by implementing renderer interfaces. Use custom renderers when built-in styling options are insufficient for your design requirements.

**When to use custom renderers:**
- Unique visual designs not achievable with themes
- Custom needle shapes or animations
- Non-standard scales or tick marks
- Special effects (shadows, glows, textures)
- Integration with custom graphics

**When NOT to use:**
- Standard appearance needs (use themes instead)
- Simple color changes (use color properties)
- Performance-critical scenarios (adds rendering overhead)

## IRadialGaugeRenderer Interface

Interface for custom RadialGauge rendering.

### Interface Definition

```csharp
public interface IRadialGaugeRenderer
{
    // Draw outer frame/arc
    void DrawOuterArc(
        Graphics graphics,
        RectangleF bounds,
        float startAngle,
        float sweepAngle,
        Color startColor,
        Color endColor
    );

    // Draw gauge needle
    void DrawNeedle(
        Graphics graphics,
        PointF center,
        float angle,
        float length,
        Color needleColor
    );

    // Draw scale labels
    void DrawLabel(
        Graphics graphics,
        string text,
        Font font,
        Color color,
        PointF location,
        StringFormat format
    );

    // Draw color-coded ranges
    void DrawRanges(
        Graphics graphics,
        RectangleF bounds,
        RangeCollection ranges,
        float startAngle,
        float sweepAngle
    );

    // Draw tick marks and inter-lines
    void DrawLines(
        Graphics graphics,
        PointF center,
        float startAngle,
        float sweepAngle,
        float majorDifference,
        float minorDifference,
        float minimumValue,
        float maximumValue
    );
}
```

### Implementing the Interface

```csharp
using System.Drawing;
using System.Drawing.Drawing2D;
using Syncfusion.Windows.Forms.Gauge;

public class CustomRadialRenderer : IRadialGaugeRenderer
{
    public void DrawOuterArc(Graphics graphics, RectangleF bounds, 
        float startAngle, float sweepAngle, Color startColor, Color endColor)
    {
        // Custom implementation
        using (LinearGradientBrush brush = new LinearGradientBrush(
            bounds, startColor, endColor, LinearGradientMode.ForwardDiagonal))
        {
            using (Pen pen = new Pen(brush, 15))
            {
                graphics.DrawArc(pen, bounds, startAngle, sweepAngle);
            }
        }
    }

    public void DrawNeedle(Graphics graphics, PointF center, 
        float angle, float length, Color needleColor)
    {
        // Custom needle implementation
        // (See complete example below)
    }

    public void DrawLabel(Graphics graphics, string text, Font font, 
        Color color, PointF location, StringFormat format)
    {
        // Custom label implementation
        using (SolidBrush brush = new SolidBrush(color))
        {
            graphics.DrawString(text, font, brush, location, format);
        }
    }

    public void DrawRanges(Graphics graphics, RectangleF bounds, 
        RangeCollection ranges, float startAngle, float sweepAngle)
    {
        // Custom range implementation
        // (See complete example below)
    }

    public void DrawLines(Graphics graphics, PointF center, 
        float startAngle, float sweepAngle, float majorDifference, 
        float minorDifference, float minimumValue, float maximumValue)
    {
        // Custom tick marks implementation
        // (See complete example below)
    }
}
```

## ILinearGaugeRenderer Interface

Interface for custom LinearGauge rendering.

### Interface Definition

```csharp
public interface ILinearGaugeRenderer
{
    // Draw gauge frame/background
    void DrawFrame(
        Graphics graphics,
        RectangleF bounds,
        Color startColor,
        Color endColor,
        LinearFrameType frameType
    );

    // Draw tick marks (major and minor)
    void DrawLines(
        Graphics graphics,
        RectangleF bounds,
        float majorDifference,
        float minorTickCount,
        float minimumValue,
        float maximumValue,
        LinearFrameType frameType
    );

    // Draw color-coded ranges
    void DrawRanges(
        Graphics graphics,
        RectangleF bounds,
        LinearRangeCollection ranges,
        float minimumValue,
        float maximumValue,
        LinearFrameType frameType
    );

    // Draw pointer/needle
    void DrawPointer(
        Graphics graphics,
        RectangleF bounds,
        float value,
        float minimumValue,
        float maximumValue,
        Color pointerColor,
        LinearFrameType frameType,
        Placement pointerPlacement
    );
}
```

### Implementing the Interface

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Gauge;

public class CustomLinearRenderer : ILinearGaugeRenderer
{
    public void DrawFrame(Graphics graphics, RectangleF bounds, 
        Color startColor, Color endColor, LinearFrameType frameType)
    {
        // Custom implementation
    }

    public void DrawLines(Graphics graphics, RectangleF bounds, 
        float majorDifference, float minorTickCount, float minimumValue, 
        float maximumValue, LinearFrameType frameType)
    {
        // Custom implementation
    }

    public void DrawRanges(Graphics graphics, RectangleF bounds, 
        LinearRangeCollection ranges, float minimumValue, float maximumValue, 
        LinearFrameType frameType)
    {
        // Custom implementation
    }

    public void DrawPointer(Graphics graphics, RectangleF bounds, 
        float value, float minimumValue, float maximumValue, 
        Color pointerColor, LinearFrameType frameType, Placement pointerPlacement)
    {
        // Custom implementation
    }
}
```

## Custom RadialGauge Renderer Example

Complete custom renderer with unique styling.

### Custom Renderer Class

```csharp
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using Syncfusion.Windows.Forms.Gauge;

public class NeonRadialRenderer : IRadialGaugeRenderer
{
    public void DrawOuterArc(Graphics graphics, RectangleF bounds, 
        float startAngle, float sweepAngle, Color startColor, Color endColor)
    {
        // Enable anti-aliasing for smooth arcs
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        // Draw outer glow effect
        using (GraphicsPath path = new GraphicsPath())
        {
            path.AddArc(bounds, startAngle, sweepAngle);
            
            // Outer glow
            using (PathGradientBrush glowBrush = new PathGradientBrush(path))
            {
                glowBrush.CenterColor = Color.FromArgb(100, startColor);
                glowBrush.SurroundColors = new[] { Color.FromArgb(0, startColor) };
                
                RectangleF glowBounds = bounds;
                glowBounds.Inflate(5, 5);
                
                using (Pen glowPen = new Pen(glowBrush, 10))
                {
                    graphics.DrawArc(glowPen, glowBounds, startAngle, sweepAngle);
                }
            }
        }

        // Draw main arc with gradient
        using (LinearGradientBrush brush = new LinearGradientBrush(
            bounds, startColor, endColor, LinearGradientMode.ForwardDiagonal))
        {
            using (Pen pen = new Pen(brush, 12))
            {
                pen.StartCap = LineCap.Round;
                pen.EndCap = LineCap.Round;
                graphics.DrawArc(pen, bounds, startAngle, sweepAngle);
            }
        }
    }

    public void DrawNeedle(Graphics graphics, PointF center, 
        float angle, float length, Color needleColor)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        // Calculate needle endpoint
        float radians = (float)(angle * Math.PI / 180);
        PointF endpoint = new PointF(
            center.X + (float)(length * Math.Cos(radians)),
            center.Y + (float)(length * Math.Sin(radians))
        );

        // Draw needle with glow effect
        using (GraphicsPath needlePath = new GraphicsPath())
        {
            // Create arrow shape
            PointF[] points = new PointF[]
            {
                endpoint,
                new PointF(center.X + 5, center.Y - 5),
                center,
                new PointF(center.X + 5, center.Y + 5)
            };
            needlePath.AddPolygon(points);

            // Glow effect
            using (PathGradientBrush glowBrush = new PathGradientBrush(needlePath))
            {
                glowBrush.CenterColor = Color.FromArgb(150, needleColor);
                glowBrush.SurroundColors = new[] { Color.FromArgb(0, needleColor) };
                graphics.FillPath(glowBrush, needlePath);
            }

            // Needle body
            using (SolidBrush brush = new SolidBrush(needleColor))
            {
                graphics.FillPath(brush, needlePath);
            }
        }

        // Draw center pivot
        using (SolidBrush pivotBrush = new SolidBrush(Color.FromArgb(200, needleColor)))
        {
            graphics.FillEllipse(pivotBrush, center.X - 8, center.Y - 8, 16, 16);
        }
    }

    public void DrawLabel(Graphics graphics, string text, Font font, 
        Color color, PointF location, StringFormat format)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;
        graphics.TextRenderingHint = System.Drawing.Text.TextRenderingHint.AntiAlias;

        // Draw text with subtle shadow
        using (SolidBrush shadowBrush = new SolidBrush(Color.FromArgb(100, Color.Black)))
        {
            PointF shadowLocation = new PointF(location.X + 1, location.Y + 1);
            graphics.DrawString(text, font, shadowBrush, shadowLocation, format);
        }

        // Draw main text
        using (SolidBrush brush = new SolidBrush(color))
        {
            graphics.DrawString(text, font, brush, location, format);
        }
    }

    public void DrawRanges(Graphics graphics, RectangleF bounds, 
        RangeCollection ranges, float startAngle, float sweepAngle)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        if (ranges == null || ranges.Count == 0)
            return;

        foreach (Range range in ranges)
        {
            // Calculate range angles
            float rangeStart = startAngle + (sweepAngle * (range.StartValue / 100));
            float rangeSweep = sweepAngle * ((range.EndValue - range.StartValue) / 100);

            // Draw range with transparency
            using (SolidBrush brush = new SolidBrush(Color.FromArgb(180, range.Color)))
            {
                RectangleF rangeBounds = new RectangleF(
                    bounds.X + range.Height,
                    bounds.Y + range.Height,
                    bounds.Width - (range.Height * 2),
                    bounds.Height - (range.Height * 2)
                );

                using (Pen pen = new Pen(brush, range.Height))
                {
                    graphics.DrawArc(pen, rangeBounds, rangeStart, rangeSweep);
                }
            }
        }
    }

    public void DrawLines(Graphics graphics, PointF center, 
        float startAngle, float sweepAngle, float majorDifference, 
        float minorDifference, float minimumValue, float maximumValue)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        float valueRange = maximumValue - minimumValue;
        int majorTickCount = (int)(valueRange / majorDifference) + 1;

        // Draw major ticks
        for (int i = 0; i < majorTickCount; i++)
        {
            float value = minimumValue + (i * majorDifference);
            float angle = startAngle + (sweepAngle * ((value - minimumValue) / valueRange));
            float radians = (float)(angle * Math.PI / 180);

            float innerRadius = 80;
            float outerRadius = 95;

            PointF innerPoint = new PointF(
                center.X + (float)(innerRadius * Math.Cos(radians)),
                center.Y + (float)(innerRadius * Math.Sin(radians))
            );

            PointF outerPoint = new PointF(
                center.X + (float)(outerRadius * Math.Cos(radians)),
                center.Y + (float)(outerRadius * Math.Sin(radians))
            );

            using (Pen pen = new Pen(Color.White, 3))
            {
                pen.StartCap = LineCap.Round;
                pen.EndCap = LineCap.Round;
                graphics.DrawLine(pen, innerPoint, outerPoint);
            }
        }

        // Draw minor ticks (similar logic with smaller size)
        // Omitted for brevity
    }
}
```

### Using Custom RadialGauge Renderer

```csharp
// Create gauge
RadialGauge gauge = new RadialGauge();
gauge.Size = new Size(300, 300);
gauge.MinimumValue = 0;
gauge.MaximumValue = 100;
gauge.Value = 65;
gauge.FrameType = FrameType.HalfCircle;

// Apply custom renderer
gauge.Renderer = new NeonRadialRenderer();

// Add to form
this.Controls.Add(gauge);
```

## Custom LinearGauge Renderer Example

Complete custom renderer for LinearGauge.

### Custom Renderer Class

```csharp
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using Syncfusion.Windows.Forms.Gauge;

public class GlassLinearRenderer : ILinearGaugeRenderer
{
    public void DrawFrame(Graphics graphics, RectangleF bounds, 
        Color startColor, Color endColor, LinearFrameType frameType)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        // Glass effect background
        using (LinearGradientBrush brush = new LinearGradientBrush(
            bounds, Color.FromArgb(220, startColor), Color.FromArgb(220, endColor),
            frameType == LinearFrameType.Horizontal ? 
                LinearGradientMode.Vertical : LinearGradientMode.Horizontal))
        {
            graphics.FillRectangle(brush, bounds);
        }

        // Highlight effect (top half lighter)
        RectangleF highlightBounds = new RectangleF(
            bounds.X, bounds.Y, 
            bounds.Width, bounds.Height / 2
        );

        using (LinearGradientBrush highlightBrush = new LinearGradientBrush(
            highlightBounds, 
            Color.FromArgb(100, Color.White), 
            Color.FromArgb(0, Color.White),
            LinearGradientMode.Vertical))
        {
            graphics.FillRectangle(highlightBrush, highlightBounds);
        }

        // Border
        using (Pen borderPen = new Pen(Color.FromArgb(150, Color.Black), 1))
        {
            graphics.DrawRectangle(borderPen, 
                bounds.X, bounds.Y, bounds.Width, bounds.Height);
        }
    }

    public void DrawLines(Graphics graphics, RectangleF bounds, 
        float majorDifference, float minorTickCount, float minimumValue, 
        float maximumValue, LinearFrameType frameType)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        float valueRange = maximumValue - minimumValue;
        int majorTickTotal = (int)(valueRange / majorDifference) + 1;

        for (int i = 0; i < majorTickTotal; i++)
        {
            float value = minimumValue + (i * majorDifference);
            float position = (value - minimumValue) / valueRange;

            float x, y, x2, y2;

            if (frameType == LinearFrameType.Horizontal)
            {
                x = bounds.X + (bounds.Width * position);
                y = bounds.Y + bounds.Height - 10;
                x2 = x;
                y2 = bounds.Y + bounds.Height;
            }
            else // Vertical
            {
                x = bounds.X;
                y = bounds.Y + bounds.Height - (bounds.Height * position);
                x2 = bounds.X + 10;
                y2 = y;
            }

            using (Pen pen = new Pen(Color.Black, 2))
            {
                graphics.DrawLine(pen, x, y, x2, y2);
            }
        }
    }

    public void DrawRanges(Graphics graphics, RectangleF bounds, 
        LinearRangeCollection ranges, float minimumValue, float maximumValue, 
        LinearFrameType frameType)
    {
        if (ranges == null || ranges.Count == 0)
            return;

        float valueRange = maximumValue - minimumValue;

        foreach (LinearRange range in ranges)
        {
            float startPos = (range.StartValue - minimumValue) / valueRange;
            float endPos = (range.EndValue - minimumValue) / valueRange;

            RectangleF rangeBounds;

            if (frameType == LinearFrameType.Horizontal)
            {
                rangeBounds = new RectangleF(
                    bounds.X + (bounds.Width * startPos),
                    bounds.Y,
                    bounds.Width * (endPos - startPos),
                    range.Height
                );
            }
            else // Vertical
            {
                rangeBounds = new RectangleF(
                    bounds.X,
                    bounds.Y + bounds.Height - (bounds.Height * endPos),
                    range.Height,
                    bounds.Height * (endPos - startPos)
                );
            }

            using (SolidBrush brush = new SolidBrush(Color.FromArgb(180, range.Color)))
            {
                graphics.FillRectangle(brush, rangeBounds);
            }
        }
    }

    public void DrawPointer(Graphics graphics, RectangleF bounds, 
        float value, float minimumValue, float maximumValue, 
        Color pointerColor, LinearFrameType frameType, Placement pointerPlacement)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;

        float position = (value - minimumValue) / (maximumValue - minimumValue);

        PointF[] arrowPoints;

        if (frameType == LinearFrameType.Horizontal)
        {
            float x = bounds.X + (bounds.Width * position);
            float y = bounds.Y + (bounds.Height / 2);

            arrowPoints = new PointF[]
            {
                new PointF(x, y - 8),
                new PointF(x + 5, y),
                new PointF(x, y + 8),
                new PointF(x - 5, y)
            };
        }
        else // Vertical
        {
            float x = bounds.X + (bounds.Width / 2);
            float y = bounds.Y + bounds.Height - (bounds.Height * position);

            arrowPoints = new PointF[]
            {
                new PointF(x - 8, y),
                new PointF(x, y + 5),
                new PointF(x + 8, y),
                new PointF(x, y - 5)
            };
        }

        // Shadow
        using (GraphicsPath shadowPath = new GraphicsPath())
        {
            shadowPath.AddPolygon(arrowPoints);
            using (PathGradientBrush shadowBrush = new PathGradientBrush(shadowPath))
            {
                shadowBrush.CenterColor = Color.FromArgb(100, Color.Black);
                shadowBrush.SurroundColors = new[] { Color.FromArgb(0, Color.Black) };
                graphics.FillPath(shadowBrush, shadowPath);
            }
        }

        // Pointer
        using (SolidBrush brush = new SolidBrush(pointerColor))
        {
            graphics.FillPolygon(brush, arrowPoints);
        }

        // Outline
        using (Pen pen = new Pen(Color.Black, 1))
        {
            graphics.DrawPolygon(pen, arrowPoints);
        }
    }
}
```

### Using Custom LinearGauge Renderer

```csharp
// Create gauge
LinearGauge gauge = new LinearGauge();
gauge.Size = new Size(400, 100);
gauge.LinearFrameType = LinearFrameType.Horizontal;
gauge.MinimumValue = 0;
gauge.MaximumValue = 100;
gauge.Value = 75;

// Apply custom renderer
gauge.Renderer = new GlassLinearRenderer();

// Add to form
this.Controls.Add(gauge);
```

## Advanced Customization Techniques

### Animated Needles

```csharp
public class AnimatedNeedleRenderer : IRadialGaugeRenderer
{
    private float animationProgress = 0f;

    public void DrawNeedle(Graphics graphics, PointF center, 
        float angle, float length, Color needleColor)
    {
        // Animate needle with pulsing effect
        float animatedLength = length * (1.0f + (float)Math.Sin(animationProgress) * 0.1f);
        
        // Draw needle with animated length
        // ... (drawing code)

        animationProgress += 0.1f;
    }

    // Trigger animation refresh
    public void Animate(RadialGauge gauge)
    {
        Timer animationTimer = new Timer();
        animationTimer.Interval = 50;
        animationTimer.Tick += (s, e) => gauge.Refresh();
        animationTimer.Start();
    }

    // Implement other interface methods...
}
```

### Textured Backgrounds

```csharp
public void DrawOuterArc(Graphics graphics, RectangleF bounds, 
    float startAngle, float sweepAngle, Color startColor, Color endColor)
{
    // Load texture image
    using (Image texture = Image.FromFile("gauge_texture.png"))
    {
        using (TextureBrush textureBrush = new TextureBrush(texture))
        {
            using (Pen pen = new Pen(textureBrush, 15))
            {
                graphics.DrawArc(pen, bounds, startAngle, sweepAngle);
            }
        }
    }
}
```

### Custom Tick Patterns

```csharp
public void DrawLines(Graphics graphics, PointF center, 
    float startAngle, float sweepAngle, float majorDifference, 
    float minorDifference, float minimumValue, float maximumValue)
{
    // Alternating tick colors
    Color[] tickColors = { Color.Red, Color.Blue, Color.Green };
    int colorIndex = 0;

    float valueRange = maximumValue - minimumValue;
    int tickCount = (int)(valueRange / majorDifference) + 1;

    for (int i = 0; i < tickCount; i++)
    {
        Color tickColor = tickColors[colorIndex % tickColors.Length];
        
        // Draw tick with alternating color
        // ... (drawing code)

        colorIndex++;
    }
}
```

## Common Use Cases

### Use Case 1: Neon/Glow Effect Gauge

```csharp
// Apply neon renderer for futuristic look
RadialGauge neonGauge = new RadialGauge();
neonGauge.Renderer = new NeonRadialRenderer();
neonGauge.BackColor = Color.Black;
```

### Use Case 2: Glass/Transparent Effect

```csharp
// Apply glass renderer for modern look
LinearGauge glassGauge = new LinearGauge();
glassGauge.Renderer = new GlassLinearRenderer();
```

### Use Case 3: Custom Brand Styling

```csharp
// Create renderer matching corporate branding
public class BrandedRenderer : IRadialGaugeRenderer
{
    // Implement with brand colors, logos, fonts
}

gauge.Renderer = new BrandedRenderer();
```

### Use Case 4: Animated Dashboard

```csharp
// Use animated renderer for dynamic displays
AnimatedNeedleRenderer animRenderer = new AnimatedNeedleRenderer();
gauge.Renderer = animRenderer;
animRenderer.Animate(gauge);
```

## Troubleshooting

### Issue: Renderer not being called

**Cause:** Renderer property not set correctly

**Solution:**
```csharp
// Ensure renderer is set before gauge is displayed
gauge.Renderer = new CustomRadialRenderer();
this.Controls.Add(gauge);  // After setting renderer
```

### Issue: Graphics artifacts or flickering

**Cause:** Missing anti-aliasing or double-buffering

**Solution:**
```csharp
// Enable anti-aliasing in all drawing methods
graphics.SmoothingMode = SmoothingMode.AntiAlias;
graphics.TextRenderingHint = System.Drawing.Text.TextRenderingHint.AntiAlias;

// Enable double-buffering on form
this.DoubleBuffered = true;
```

### Issue: Performance degradation

**Cause:** Complex rendering in custom renderer

**Solution:**
- Cache brushes and pens instead of creating each time
- Reduce transparency/alpha blending
- Simplify path calculations
- Avoid expensive operations in DrawXxx methods

```csharp
// Cache expensive objects
private SolidBrush cachedBrush;

public CustomRenderer()
{
    cachedBrush = new SolidBrush(Color.Blue);
}

public void DrawXxx(...)
{
    // Reuse cached brush instead of creating new one
    graphics.FillRectangle(cachedBrush, bounds);
}
```

### Issue: Incorrect scaling or positioning

**Cause:** Not accounting for bounds parameter

**Solution:**
```csharp
// Always use bounds parameter for positioning
public void DrawXxx(Graphics graphics, RectangleF bounds, ...)
{
    // Calculate positions relative to bounds
    float centerX = bounds.X + (bounds.Width / 2);
    float centerY = bounds.Y + (bounds.Height / 2);
    
    // Use calculated positions for drawing
}
```

## Best Practices

1. **Implement all interface methods** - Even if some just call base implementation
2. **Enable anti-aliasing** - For smooth, professional appearance
3. **Cache graphics objects** - Reuse brushes, pens, fonts when possible
4. **Test at different sizes** - Ensure renderer scales correctly
5. **Handle edge cases** - Null checks for collections, bounds validation
6. **Document renderer purpose** - Explain design intent and usage scenarios
7. **Provide configuration options** - Allow customization of renderer behavior
8. **Optimize performance** - Profile and optimize drawing code
9. **Maintain consistency** - Keep styling consistent across all drawing methods
10. **Dispose resources** - Use `using` statements for IDisposable objects
