# Custom Styling and Renderers

## Table of Contents
- [Overview](#overview)
- [IToggleButtonRenderer Interface](#itogglebutton-renderer-interface)
- [Custom Renderer Implementation](#custom-renderer-implementation)
- [Assigning Custom Renderers](#assigning-custom-renderers)
- [Advanced Styling Examples](#advanced-styling-examples)
- [Common Customizations](#common-customizations)

## Overview

The Toggle Button control can be customized beyond the built-in color and text properties by implementing a custom renderer. The `IToggleButtonRenderer` interface provides methods that give you complete control over how the button is drawn.

**When to use custom renderers:**
- ✅ Create completely custom appearances
- ✅ Add decorative elements (shadows, gradients, borders)
- ✅ Implement complex styling logic
- ✅ Match specialized design requirements
- ✅ Create branded or themed buttons

## IToggleButtonRenderer Interface

The `IToggleButtonRenderer` interface defines the methods you must implement:

```csharp
public interface IToggleButtonRenderer
{
    // Core rendering methods
    void DrawBorder(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered);
    void DrawBackground(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered);
    void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered);
    void DrawArrow(Graphics graphics, Rectangle bounds, bool isActive);
    void DrawFocusRectangle(Graphics graphics, Rectangle bounds);
    
    // Additional methods
    void Initialize(ToggleButton toggleButton);
    void Dispose();
}
```

### Method Purposes

| Method | Purpose |
|--------|---------|
| `DrawBorder` | Draw the button's border/outline |
| `DrawBackground` | Draw the button's background |
| `DrawText` | Draw the button's text content |
| `DrawArrow` | Draw optional arrow indicator |
| `DrawFocusRectangle` | Draw focus rectangle for keyboard navigation |
| `Initialize` | Initialize renderer with the button instance |
| `Dispose` | Clean up resources |

## Custom Renderer Implementation

### Basic Custom Renderer

Create a class that implements `IToggleButtonRenderer`:

```csharp
public class RoundedCornerToggleButtonRenderer : IToggleButtonRenderer
{
    private ToggleButton _toggleButton;
    private int _cornerRadius = 10;
    
    public void Initialize(ToggleButton toggleButton)
    {
        _toggleButton = toggleButton;
    }
    
    public void DrawBorder(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        Color borderColor = isActive ? Color.FromArgb(0, 120, 215) : Color.Gray;
        
        using (Pen pen = new Pen(borderColor, 2))
        using (GraphicsPath path = CreateRoundedRectanglePath(bounds, _cornerRadius))
        {
            graphics.DrawPath(pen, path);
        }
    }
    
    public void DrawBackground(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        Color backgroundColor = isActive ? Color.FromArgb(0, 150, 255) : Color.WhiteSmoke;
        if (isHovered)
        {
            backgroundColor = isActive ? Color.FromArgb(0, 180, 255) : Color.LightGray;
        }
        
        using (Brush brush = new SolidBrush(backgroundColor))
        using (GraphicsPath path = CreateRoundedRectanglePath(bounds, _cornerRadius))
        {
            graphics.FillPath(brush, path);
        }
    }
    
    public void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered)
    {
        Color textColor = isActive ? Color.White : Color.Black;
        
        using (Font font = new Font("Arial", 10, FontStyle.Bold))
        using (Brush brush = new SolidBrush(textColor))
        {
            StringFormat format = new StringFormat();
            format.Alignment = StringAlignment.Center;
            format.LineAlignment = StringAlignment.Center;
            
            graphics.DrawString(text, font, brush, bounds, format);
        }
    }
    
    public void DrawArrow(Graphics graphics, Rectangle bounds, bool isActive)
    {
        // Draw arrow if needed
    }
    
    public void DrawFocusRectangle(Graphics graphics, Rectangle bounds)
    {
        ControlPaint.DrawFocusRectangle(graphics, bounds);
    }
    
    public void Dispose()
    {
        // Cleanup
    }
    
    private GraphicsPath CreateRoundedRectanglePath(Rectangle bounds, int radius)
    {
        GraphicsPath path = new GraphicsPath();
        path.AddArc(bounds.X, bounds.Y, radius * 2, radius * 2, 180, 90);
        path.AddArc(bounds.Right - radius * 2, bounds.Y, radius * 2, radius * 2, 270, 90);
        path.AddArc(bounds.Right - radius * 2, bounds.Bottom - radius * 2, radius * 2, radius * 2, 0, 90);
        path.AddArc(bounds.X, bounds.Bottom - radius * 2, radius * 2, radius * 2, 90, 90);
        path.CloseFigure();
        return path;
    }
}
```

## Assigning Custom Renderers

Assign custom renderer to the Toggle Button and optionally support dynamic switching:

```csharp
// Direct assignment
public Form1()
{
    InitializeComponent();
    toggleButton1.Renderer = new CustomToggleButtonRenderer();
}

// Dynamic renderer switching
private IToggleButtonRenderer _defaultRenderer;
private IToggleButtonRenderer _customRenderer = new RoundedCornerToggleButtonRenderer();

private void UseDefaultRenderer() => toggleButton1.Renderer = _defaultRenderer;
private void UseCustomRenderer() => toggleButton1.Renderer = _customRenderer;
```

## Advanced Styling Examples

### Example 1: Gradient Background Renderer

```csharp
public class GradientToggleButtonRenderer : IToggleButtonRenderer
{
    private ToggleButton _toggleButton;
    
    public void Initialize(ToggleButton toggleButton)
    {
        _toggleButton = toggleButton;
    }
    
    public void DrawBorder(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        Color borderColor = isActive ? Color.DarkBlue : Color.DarkGray;
        using (Pen pen = new Pen(borderColor, 2))
        {
            graphics.DrawRectangle(pen, bounds);
        }
    }
    
    public void DrawBackground(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        Color topColor = isActive ? Color.Blue : Color.LightGray;
        Color bottomColor = isActive ? Color.RoyalBlue : Color.Gray;
        
        if (isHovered)
        {
            topColor = isActive ? Color.RoyalBlue : Color.Gray;
            bottomColor = isActive ? Color.DodgerBlue : Color.DarkGray;
        }
        
        using (LinearGradientBrush brush = new LinearGradientBrush(
            bounds, topColor, bottomColor, LinearGradientMode.Vertical))
        {
            graphics.FillRectangle(brush, bounds);
        }
    }
    
    public void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered)
    {
        Color textColor = Color.White;
        using (Font font = new Font("Arial", 10, FontStyle.Bold))
        using (Brush brush = new SolidBrush(textColor))
        {
            StringFormat format = new StringFormat();
            format.Alignment = StringAlignment.Center;
            format.LineAlignment = StringAlignment.Center;
            graphics.DrawString(text, font, brush, bounds, format);
        }
    }
    
    public void DrawArrow(Graphics graphics, Rectangle bounds, bool isActive) { }
    public void DrawFocusRectangle(Graphics graphics, Rectangle bounds)
    {
        ControlPaint.DrawFocusRectangle(graphics, bounds);
    }
    public void Dispose() { }
}
```

### Example 2: Shadow Effect Renderer

```csharp
public class ShadowToggleButtonRenderer : IToggleButtonRenderer
{
    private ToggleButton _toggleButton;
    private const int ShadowOffset = 3;
    
    public void Initialize(ToggleButton toggleButton)
    {
        _toggleButton = toggleButton;
    }
    
    public void DrawBorder(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        // Draw shadow
        Rectangle shadowBounds = new Rectangle(bounds.X + ShadowOffset, bounds.Y + ShadowOffset,
            bounds.Width, bounds.Height);
        
        using (Brush shadowBrush = new SolidBrush(Color.FromArgb(128, Color.Gray)))
        {
            graphics.FillRectangle(shadowBrush, shadowBounds);
        }
        
        // Draw border
        Color borderColor = isActive ? Color.FromArgb(34, 177, 76) : Color.FromArgb(100, 100, 100);
        using (Pen pen = new Pen(borderColor, 2))
        {
            graphics.DrawRectangle(pen, bounds);
        }
    }
    
    public void DrawBackground(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
    {
        Color backgroundColor = isActive ? Color.FromArgb(76, 207, 95) : Color.White;
        if (isHovered)
        {
            backgroundColor = isActive ? Color.FromArgb(100, 220, 115) : Color.LightGray;
        }
        
        using (Brush brush = new SolidBrush(backgroundColor))
        {
            graphics.FillRectangle(brush, bounds);
        }
    }
    
    public void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered)
    {
        Color textColor = isActive ? Color.White : Color.Black;
        using (Font font = new Font("Arial", 10, FontStyle.Bold))
        using (Brush brush = new SolidBrush(textColor))
        {
            StringFormat format = new StringFormat();
            format.Alignment = StringAlignment.Center;
            format.LineAlignment = StringAlignment.Center;
            graphics.DrawString(text, font, brush, bounds, format);
        }
    }
    
    public void DrawArrow(Graphics graphics, Rectangle bounds, bool isActive) { }
    public void DrawFocusRectangle(Graphics graphics, Rectangle bounds)
    {
        ControlPaint.DrawFocusRectangle(graphics, bounds);
    }
    public void Dispose() { }
}
```

## Common Customizations

```csharp
// Border style with hover effect
public void DrawBorder(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
{
    using (Pen pen = new Pen(isActive ? Color.Blue : Color.Gray, isHovered ? 3 : 2)
    { DashStyle = DashStyle.Solid })
        graphics.DrawRectangle(pen, bounds);
}

// Text with custom font size
public void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered)
{
    using (Font font = new Font("Arial", 12, FontStyle.Bold))
    using (Brush brush = new SolidBrush(isActive ? Color.White : Color.Black))
    {
        StringFormat format = new StringFormat { Alignment = StringAlignment.Center, LineAlignment = StringAlignment.Center };
        graphics.DrawString(text, font, brush, bounds, format);
    }
}

// Background with opacity effects
public void DrawBackground(Graphics graphics, Rectangle bounds, bool isActive, bool isHovered)
{
    int alpha = isHovered ? 255 : 200;
    using (Brush brush = new SolidBrush(isActive ? Color.FromArgb(alpha, 34, 177, 76) : Color.FromArgb(alpha, 150, 150, 150)))
        graphics.FillRectangle(brush, bounds);
}
```
## Performance Considerations

1. **Avoid Creating Objects in Draw Methods**: Pre-create Fonts and Brushes
2. **Use Graphics Smoothing**: Enable anti-aliasing for smooth lines
3. **Cache Calculations**: Store frequently-used values as member variables
4. **Minimize Redraws**: Only invalidate when necessary

```csharp
public class OptimizedToggleButtonRenderer : IToggleButtonRenderer
{
    private ToggleButton _toggleButton;
    private Font _textFont;
    private Brush _activeTextBrush;
    private Brush _inactiveTextBrush;
    
    public void Initialize(ToggleButton toggleButton)
    {
        _toggleButton = toggleButton;
        _textFont = new Font("Arial", 10, FontStyle.Bold);
        _activeTextBrush = new SolidBrush(Color.White);
        _inactiveTextBrush = new SolidBrush(Color.Black);
    }
    
    public void DrawText(Graphics graphics, Rectangle bounds, string text, bool isActive, bool isHovered)
    {
        graphics.SmoothingMode = SmoothingMode.AntiAlias;
        Brush brush = isActive ? _activeTextBrush : _inactiveTextBrush;
        
        StringFormat format = new StringFormat();
        format.Alignment = StringAlignment.Center;
        format.LineAlignment = StringAlignment.Center;
        
        graphics.DrawString(text, _textFont, brush, bounds, format);
    }
    
    public void Dispose()
    {
        _textFont?.Dispose();
        _activeTextBrush?.Dispose();
        _inactiveTextBrush?.Dispose();
    }
}
```
