# Advanced Customization

This guide covers advanced customization techniques for the SplitButton control, including custom rendering using ISplitButtonRenderer for the button itself and ToolStripRenderer for dropdown items.

## Table of Contents
- [Overview](#overview)
- [ISplitButtonRenderer Interface](#isplitbuttonrenderer-interface)
- [Custom Button Renderer Implementation](#custom-button-renderer-implementation)
- [DropDown Items Customization](#dropdown-items-customization)
- [Complete Custom Renderer Examples](#complete-custom-renderer-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SplitButton control provides two levels of customization:

1. **Button Customization:** Use `ISplitButtonRenderer` interface to control button appearance (borders, text, arrow)
2. **DropDown Customization:** Use `DropDownRenderer` property with `ToolStripRenderer` to customize dropdown menu appearance

Both approaches provide complete control over drawing and appearance beyond the built-in themes.

## ISplitButtonRenderer Interface

The `ISplitButtonRenderer` interface provides methods for custom painting of the button portion of the SplitButton.

### Interface Methods

**DrawText:**
```csharp
void DrawText(PaintEventArgs e, string text, Font font, Color color, 
              int width, int height, int split)
```
Controls how button text is drawn. Parameters include graphics context, text string, font, color, and dimensions.

**DrawBorder:**
```csharp
void DrawBorder(PaintEventArgs e, int width, int height, int split, 
                Color outerColor, Color innerColor, Color arrowOuter, 
                Color arrowInner, Color buttonInner)
```
Controls button border and background drawing. Provides dimensions and color parameters for different border regions.

**DrawArrow:**
```csharp
void DrawArrow(int left, int top, int width, int height, 
               PaintEventArgs e, Color ArrowColor)
```
Controls dropdown arrow appearance. Can draw custom arrow graphics or images.

**SplitButton Property:**
```csharp
SplitButton SplitButton { get; set; }
```
Reference to the SplitButton control being rendered.

## Custom Button Renderer Implementation

### Step 1: Create Custom Renderer Class

Implement the `ISplitButtonRenderer` interface:

```csharp
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class CustomRenderer : ISplitButtonRenderer
{
    private SplitButton splitButton;
    
    #region ISplitButtonRenderer Members
    
    public void DrawText(PaintEventArgs e, string text, Font font, Color color, 
                         int width, int height, int split)
    {
        // Custom text drawing implementation
        SolidBrush brush = new SolidBrush(color);
        StringFormat format = new StringFormat
        {
            Trimming = StringTrimming.EllipsisCharacter,
            LineAlignment = StringAlignment.Center,
            Alignment = StringAlignment.Center
        };
        
        Rectangle textArea = new Rectangle(7, 1, width - split, height);
        e.Graphics.DrawString(text, font, brush, textArea, format);
        
        // Optional: Draw icon
        Rectangle imageRect = new Rectangle(4, 11, 15, height - 24);
        Image img = Image.FromFile(@"../../logo_16.ico");
        e.Graphics.DrawImage(img, imageRect);
        
        brush.Dispose();
    }
    
    public void DrawBorder(PaintEventArgs e, int width, int height, int split, 
                           Color outerColor, Color innerColor, Color arrowOuter, 
                           Color arrowInner, Color buttonInner)
    {
        // Custom border and background drawing
        Form1 frm = new Form1();
        Color color1 = frm.startcolor;
        Color color2 = frm.endcolor;
        
        // Gradient background
        Brush linearGradientBrush = new LinearGradientBrush(
            new Rectangle(0, 0, width, height), color1, color2, 90);
        e.Graphics.FillRectangle(linearGradientBrush, new Rectangle(0, 0, width, height));
        linearGradientBrush.Dispose();
        
        // Draw borders
        Pen outerPen = new Pen(Color.DarkGreen);
        Pen innerPen = new Pen(Color.LightGreen);
        Pen arrowInnerPen = new Pen(Color.LightGreen);
        Pen arrowOuterPen = new Pen(arrowOuter);
        Pen buttonInnerPen = new Pen(buttonInner);
        
        // Inner border
        e.Graphics.DrawLine(innerPen, new Point(1, 1), new Point(width - 2, 1));
        e.Graphics.DrawLine(innerPen, new Point(width - 2, 1), new Point(width - 2, height - 2));
        e.Graphics.DrawLine(innerPen, new Point(1, height - 2), new Point(width - 2, height - 2));
        e.Graphics.DrawLine(innerPen, new Point(1, 1), new Point(1, height - 2));
        
        // Arrow separator
        e.Graphics.DrawLine(arrowOuterPen, new Point(width - split, 0), 
                           new Point(width - split, height - 1));
        e.Graphics.DrawLine(buttonInnerPen, new Point(width - split - 1, 2), 
                           new Point(width - split - 1, height - 3));
        e.Graphics.DrawRectangle(arrowInnerPen, width - split + 1, 1, split - 3, height - 3);
        
        // Outer border
        e.Graphics.DrawLine(outerPen, new Point(1, 0), new Point(width - 2, 0));
        e.Graphics.DrawLine(outerPen, new Point(width - 2, 0), new Point(width - 2, 1));
        e.Graphics.DrawLine(outerPen, new Point(width - 1, 1), new Point(width - 1, height - 2));
        e.Graphics.DrawLine(outerPen, new Point(width - 2, height - 2), 
                           new Point(width - 2, height - 1));
        e.Graphics.DrawLine(outerPen, new Point(1, height - 1), new Point(width - 2, height - 1));
        e.Graphics.DrawLine(outerPen, new Point(1, height - 1), new Point(1, height - 2));
        e.Graphics.DrawLine(outerPen, new Point(0, 1), new Point(0, height - 2));
        e.Graphics.DrawLine(outerPen, new Point(1, 0), new Point(1, 1));
        
        // Dispose pens
        buttonInnerPen.Dispose();
        innerPen.Dispose();
        arrowInnerPen.Dispose();
        arrowOuterPen.Dispose();
        outerPen.Dispose();
    }
    
    public void DrawArrow(int left, int top, int width, int height, 
                          PaintEventArgs e, Color ArrowColor)
    {
        // Custom arrow drawing - use image or draw custom shape
        Image arrowImage = Image.FromFile(@"../../arrow4.png");
        Rectangle imageRect = new Rectangle(left + 4, top + 14, width - 9, height - 28);
        e.Graphics.DrawImage(arrowImage, imageRect);
    }
    
    public SplitButton SplitButton
    {
        get { return splitButton; }
        set { splitButton = value; }
    }
    
    #endregion
}
```

### Step 2: Apply Custom Renderer

Assign your custom renderer to the SplitButton:

```csharp
// Create SplitButton
SplitButton splitButton = new SplitButton();
splitButton.Text = "Custom Button";
splitButton.Size = new Size(120, 40);
splitButton.Location = new Point(20, 20);

// Assign custom renderer
splitButton.Renderer = new CustomRenderer();

// Add to form
this.Controls.Add(splitButton);
```

## DropDown Items Customization

### ToolStripProfessionalRenderer

Customize dropdown menu appearance by creating a custom `ToolStripProfessionalRenderer`:

```csharp
public class CustomDropDownRenderer : ToolStripProfessionalRenderer
{
    Rectangle ItemBound = new Rectangle(0, 0, 1, 1);
    Rectangle selectedItemBound = new Rectangle(0, 0, 1, 1);
    
    // Customize dropdown item text
    protected override void OnRenderItemText(ToolStripItemTextRenderEventArgs e)
    {
        e.Item.ForeColor = Color.Blue;
        base.OnRenderItemText(e);
    }
    
    // Customize image margin appearance
    protected override void OnRenderImageMargin(ToolStripRenderEventArgs e)
    {
        base.OnRenderImageMargin(e);
        var LinearBrush = new LinearGradientBrush(e.AffectedBounds, 
            Color.LightPink, Color.LightBlue, 0.0f);
        e.Graphics.FillRectangle(LinearBrush, e.AffectedBounds);
    }
    
    // Customize dropdown item image
    protected override void OnRenderItemImage(ToolStripItemImageRenderEventArgs e)
    {
        e.Graphics.SmoothingMode = SmoothingMode.AntiAlias;
        var LinearBrush = new LinearGradientBrush(e.ImageRectangle, 
            Color.LightGreen, Color.Orange, 0.0f);
        e.Graphics.FillEllipse(LinearBrush, e.ImageRectangle);
        e.Graphics.DrawEllipse(new Pen(Brushes.BlueViolet, 2), e.ImageRectangle);
        e.Graphics.DrawImage(e.Image, 122, e.ImageRectangle.Y, 20, 20);
    }
    
    // Customize dropdown background
    protected override void OnRenderToolStripBackground(ToolStripRenderEventArgs e)
    {
        base.OnRenderToolStripBackground(e);
        ItemBound = e.AffectedBounds;
        LinearGradientBrush LinearBrush = new LinearGradientBrush(ItemBound, 
            Color.LightBlue, Color.White, 0.0f);
        e.Graphics.FillRectangle(LinearBrush, ItemBound);
        this.RoundedEdges = true;
    }
    
    // Customize dropdown border
    protected override void OnRenderToolStripBorder(ToolStripRenderEventArgs e)
    {
        base.OnRenderToolStripBorder(e);
        e.Graphics.DrawRectangle(new Pen(Brushes.BlueViolet, 6f), 0, 0, 
            e.AffectedBounds.Width, e.AffectedBounds.Height);
    }
}
```

### Applying DropDown Renderer

```csharp
// Create SplitButton
SplitButton splitButton = new SplitButton();
splitButton.Text = "Custom Dropdown";
splitButton.Size = new Size(140, 40);

// Add dropdown items
splitButton.DropDownItems.Add(new ToolStripMenuItem("Item 1"));
splitButton.DropDownItems.Add(new ToolStripMenuItem("Item 2"));
splitButton.DropDownItems.Add(new ToolStripMenuItem("Item 3"));

// Assign custom dropdown renderer
splitButton.DropDownRenderer = new CustomDropDownRenderer();

this.Controls.Add(splitButton);
```

## Complete Custom Renderer Examples

### Example 1: Gradient Button with Custom Arrow

```csharp
public class GradientRenderer : ISplitButtonRenderer
{
    private SplitButton splitButton;
    
    public void DrawText(PaintEventArgs e, string text, Font font, Color color, 
                         int width, int height, int split)
    {
        using (SolidBrush brush = new SolidBrush(Color.White))
        using (StringFormat format = new StringFormat
        {
            LineAlignment = StringAlignment.Center,
            Alignment = StringAlignment.Center
        })
        {
            Rectangle textArea = new Rectangle(5, 0, width - split - 5, height);
            e.Graphics.DrawString(text, font, brush, textArea, format);
        }
    }
    
    public void DrawBorder(PaintEventArgs e, int width, int height, int split, 
                           Color outerColor, Color innerColor, Color arrowOuter, 
                           Color arrowInner, Color buttonInner)
    {
        // Blue gradient background
        using (LinearGradientBrush brush = new LinearGradientBrush(
            new Rectangle(0, 0, width, height), 
            Color.FromArgb(41, 128, 185), 
            Color.FromArgb(109, 178, 227), 
            90f))
        {
            e.Graphics.FillRectangle(brush, 0, 0, width, height);
        }
        
        // Dark blue border
        using (Pen borderPen = new Pen(Color.FromArgb(21, 67, 96), 2))
        {
            e.Graphics.DrawRectangle(borderPen, 1, 1, width - 2, height - 2);
        }
        
        // Arrow separator line
        using (Pen separatorPen = new Pen(Color.FromArgb(21, 67, 96), 1))
        {
            e.Graphics.DrawLine(separatorPen, width - split, 2, width - split, height - 2);
        }
    }
    
    public void DrawArrow(int left, int top, int width, int height, 
                          PaintEventArgs e, Color ArrowColor)
    {
        // Draw custom arrow shape
        Point[] arrowPoints = new Point[]
        {
            new Point(left + width / 2 - 3, top + height / 2 - 2),
            new Point(left + width / 2 + 3, top + height / 2 - 2),
            new Point(left + width / 2, top + height / 2 + 2)
        };
        
        using (SolidBrush arrowBrush = new SolidBrush(Color.White))
        {
            e.Graphics.FillPolygon(arrowBrush, arrowPoints);
        }
    }
    
    public SplitButton SplitButton
    {
        get { return splitButton; }
        set { splitButton = value; }
    }
}

// Usage
SplitButton btn = new SplitButton
{
    Text = "Gradient Button",
    Size = new Size(150, 40),
    Renderer = new GradientRenderer()
};
```

### Example 2: Flat Modern Design

```csharp
public class FlatModernRenderer : ISplitButtonRenderer
{
    private SplitButton splitButton;
    private Color accentColor = Color.FromArgb(0, 122, 204);
    
    public void DrawText(PaintEventArgs e, string text, Font font, Color color, 
                         int width, int height, int split)
    {
        using (SolidBrush brush = new SolidBrush(Color.Black))
        using (StringFormat format = new StringFormat
        {
            LineAlignment = StringAlignment.Center,
            Alignment = StringAlignment.Center
        })
        {
            Rectangle textArea = new Rectangle(5, 0, width - split - 5, height);
            e.Graphics.DrawString(text, font, brush, textArea, format);
        }
    }
    
    public void DrawBorder(PaintEventArgs e, int width, int height, int split, 
                           Color outerColor, Color innerColor, Color arrowOuter, 
                           Color arrowInner, Color buttonInner)
    {
        // Flat white background
        using (SolidBrush bgBrush = new SolidBrush(Color.White))
        {
            e.Graphics.FillRectangle(bgBrush, 0, 0, width, height);
        }
        
        // Accent color bottom border
        using (SolidBrush accentBrush = new SolidBrush(accentColor))
        {
            e.Graphics.FillRectangle(accentBrush, 0, height - 3, width, 3);
        }
        
        // Minimal border
        using (Pen borderPen = new Pen(Color.FromArgb(220, 220, 220), 1))
        {
            e.Graphics.DrawRectangle(borderPen, 0, 0, width - 1, height - 1);
        }
        
        // Arrow separator
        using (Pen separatorPen = new Pen(Color.FromArgb(220, 220, 220), 1))
        {
            e.Graphics.DrawLine(separatorPen, width - split, 0, width - split, height);
        }
    }
    
    public void DrawArrow(int left, int top, int width, int height, 
                          PaintEventArgs e, Color ArrowColor)
    {
        // Simple chevron down
        Point[] arrowPoints = new Point[]
        {
            new Point(left + width / 2 - 4, top + height / 2 - 2),
            new Point(left + width / 2 + 4, top + height / 2 - 2),
            new Point(left + width / 2, top + height / 2 + 2)
        };
        
        using (SolidBrush arrowBrush = new SolidBrush(accentColor))
        {
            e.Graphics.SmoothingMode = SmoothingMode.AntiAlias;
            e.Graphics.FillPolygon(arrowBrush, arrowPoints);
        }
    }
    
    public SplitButton SplitButton
    {
        get { return splitButton; }
        set { splitButton = value; }
    }
}

// Usage
SplitButton btn = new SplitButton
{
    Text = "Modern Button",
    Size = new Size(150, 40),
    Renderer = new FlatModernRenderer()
};
```

### Example 3: Combined Button and DropDown Customization

```csharp
public Form1()
{
    InitializeComponent();
    
    // Create SplitButton
    SplitButton customBtn = new SplitButton
    {
        Text = "Fully Custom",
        Size = new Size(160, 45),
        Location = new Point(20, 20)
    };
    
    // Add items
    customBtn.DropDownItems.Add(new ToolStripMenuItem("Option 1"));
    customBtn.DropDownItems.Add(new ToolStripMenuItem("Option 2"));
    customBtn.DropDownItems.Add(new ToolStripMenuItem("Option 3"));
    
    // Apply custom button renderer
    customBtn.Renderer = new GradientRenderer();
    
    // Apply custom dropdown renderer
    customBtn.DropDownRenderer = new CustomDropDownRenderer();
    
    this.Controls.Add(customBtn);
}
```

## Best Practices

**Renderer Implementation:**
- Dispose of GDI+ objects (Brush, Pen, Font) properly using `using` statements or explicit `.Dispose()`
- Use `SmoothingMode.AntiAlias` for smooth shapes and text
- Cache frequently used colors and resources as class fields
- Test rendering at different DPI settings (100%, 125%, 150%)

**Performance:**
- Avoid creating new objects in DrawText/DrawBorder/DrawArrow on every paint
- Cache brushes, pens, and images at the class level
- Use simple shapes for better performance with frequent repaints
- Profile painting performance if button updates frequently

**Design:**
- Ensure text is readable with sufficient contrast
- Make arrow clearly visible and recognizable
- Test appearance in different Windows themes and DPI settings
- Maintain consistent styling across all custom buttons

**Compatibility:**
- Test custom renderers with different .NET versions
- Verify appearance on different Windows versions (7, 10, 11)
- Check behavior with high contrast themes
- Ensure touch-friendly hit areas for modern interfaces

## Troubleshooting

**Issue: Custom renderer not applied**
- Verify Renderer property is set after control creation
- Ensure custom renderer class implements all ISplitButtonRenderer members
- Check that SplitButton property in renderer is being set correctly

**Issue: Graphics artifacts or flicker**
- Enable double buffering on parent form: `this.DoubleBuffered = true`
- Dispose of all GDI+ objects properly
- Avoid drawing outside the control bounds

**Issue: Text not visible or incorrectly positioned**
- Check Rectangle bounds in DrawText method
- Verify StringFormat alignment settings
- Ensure text color contrasts with background
- Account for split parameter when calculating text area width

**Issue: Memory leaks**
- Dispose of all Brush, Pen, Font, and Image objects
- Use `using` statements for temporary GDI+ objects
- Don't store undisposed graphics objects as class fields

**Issue: DropDown renderer not applying**
- Verify DropDownRenderer property is set (not Renderer)
- Inherit from ToolStripProfessionalRenderer or ToolStripRenderer
- Override the correct rendering methods
- Call base methods if you want to preserve some default behavior

## Next Steps

- **Visual Styles:** Read [visual-styles.md](visual-styles.md) for built-in theme options (simpler than custom rendering)
- **Button Modes:** Read [button-modes.md](button-modes.md) to understand state rendering for toggle buttons
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
