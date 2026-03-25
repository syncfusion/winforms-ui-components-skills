# Digital Clock

## Table of Contents
- [Enabling Digital Mode](#enabling-digital-mode)
- [Frames vs Shapes — Mutually Exclusive](#frames-vs-shapes--mutually-exclusive)
- [Frames](#frames)
- [Shapes](#shapes)
- [Color Customization](#color-customization)
- [Behavior Properties](#behavior-properties)
- [Custom Time in Digital Mode](#custom-time-in-digital-mode)
- [Custom Digital Frame Renderer](#custom-digital-frame-renderer)

---

## Enabling Digital Mode

Switch the existing `Clock` control to display time as digital text:

**C#:**
```csharp
this.clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Digital;
```

**VB.NET:**
```vb
Me.clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Digital
```

---

## Frames vs Shapes — Mutually Exclusive

The DigitalClock supports two visual container styles — **frames** and **shapes** — that are mutually exclusive:

| Mode | `ShowClockFrame` | Property used |
|---|---|---|
| **Frames** (built-in frame images) | `true` | `ClockFrame` |
| **Shapes** (geometric fill) | `false` | `ClockShape` |

- Set `ShowClockFrame = true` → use `ClockFrame` to pick a frame style.
- Set `ShowClockFrame = false` → use `ClockShape` to pick a background shape.

---

## Frames

Frames are decorative border images drawn around the digital display. Enable them with `ShowClockFrame = true`.

| `ClockFrames` Value | Appearance |
|---|---|
| `RectangularFrame` | Rectangle-shaped frame |
| `CircularFrame` | Circular-shaped frame |
| `SquareFrame` | Square-shaped frame |

**C# — Rectangular frame:**
```csharp
this.clock1.ShowClockFrame = true;
this.clock1.ClockFrame = Syncfusion.Windows.Forms.Tools.ClockFrames.RectangularFrame;
```

**C# — Circular frame:**
```csharp
this.clock1.ShowClockFrame = true;
this.clock1.ClockFrame = Syncfusion.Windows.Forms.Tools.ClockFrames.CircularFrame;
```

**C# — Square frame:**
```csharp
this.clock1.ShowClockFrame = true;
this.clock1.ClockFrame = Syncfusion.Windows.Forms.Tools.ClockFrames.SquareFrame;
```

**VB.NET:**
```vb
Me.clock1.ShowClockFrame = True
Me.clock1.ClockFrame = Syncfusion.Windows.Forms.Tools.ClockFrames.RectangularFrame
```

---

## Shapes

Shapes are solid geometric backgrounds rendered behind the digital text. Disable frames first: `ShowClockFrame = false`.

| `ClockShapes` Value | Appearance |
|---|---|
| `Rectangle` | Rectangular background |
| `RoundedRectangle` | Rectangle with rounded corners |
| `Circle` | Circular background |
| `Square` | Square background |
| `RoundedSquare` | Square with rounded corners |

**C# — Rectangle shape:**
```csharp
this.clock1.ShowClockFrame = false;
this.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.Rectangle;
```

**C# — Circle shape:**
```csharp
this.clock1.ShowClockFrame = false;
this.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.Circle;
```

**C# — RoundedRectangle shape:**
```csharp
this.clock1.ShowClockFrame = false;
this.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.RoundedRectangle;
```

**C# — Square / RoundedSquare:**
```csharp
this.clock1.ShowClockFrame = false;
this.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.Square;

this.clock1.ShowClockFrame = false;
this.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.RoundedSquare;
```

**VB.NET:**
```vb
Me.clock1.ShowClockFrame = False
Me.clock1.ClockShape = Syncfusion.Windows.Forms.Tools.ClockShapes.Circle
```

---

## Color Customization

The DigitalClock uses three distinct color properties:

| Property | Description |
|---|---|
| `ForeColor` | Text/digit color |
| `BackgroundColor` | Background fill color (behind the digits) |
| `BorderColor` | Border color — only visible when a `ClockShape` is active |

**C#:**
```csharp
this.clock1.ForeColor        = System.Drawing.Color.Yellow;
this.clock1.BackgroundColor  = System.Drawing.SystemColors.ActiveCaption;
this.clock1.BorderColor      = System.Drawing.Color.Yellow;
```

**VB.NET:**
```vb
Me.clock1.ForeColor       = System.Drawing.Color.Yellow
Me.clock1.BackgroundColor = System.Drawing.SystemColors.ActiveCaption
Me.clock1.BorderColor     = System.Drawing.Color.Yellow
```

> `BorderColor` is only rendered when `ShowClockFrame = false` and a `ClockShape` is set. It has no visible effect when a frame is active.

---

## Behavior Properties

| Property | Default | Description |
|---|---|---|
| `DisplayDates` | `false` | Show the current weekday name and date below the time |
| `ShowHourDesignator` | `true` | Show AM / PM indicator alongside the time |

**C#:**
```csharp
this.clock1.DisplayDates       = true;   // show weekday + date
this.clock1.ShowHourDesignator = false;  // hide AM/PM
```

**VB.NET:**
```vb
Me.clock1.DisplayDates       = True
Me.clock1.ShowHourDesignator = False
```

---

## Custom Time in Digital Mode

Custom time works identically in digital mode — enable `ShowCustomTimeClock` and provide a `DateTime`:

**C#:**
```csharp
this.clock1.ShowCustomTimeClock = true;
this.clock1.CustomTime = new System.DateTime(2013, 9, 14, 10, 10, 15, 0);
```

**VB.NET:**
```vb
Me.clock1.ShowCustomTimeClock = True
Me.clock1.CustomTime = New Date(2013, 9, 14, 10, 10, 15, 0)
```

---

## Custom Digital Frame Renderer

To replace the default frame image with your own (e.g., a PNG asset), subclass `DigitalClockRenderer` and override `DrawDigitalClockFrame`:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// 1. Create custom renderer
public class MyDigitalRenderer : DigitalClockRenderer
{
    public override void DrawDigitalClockFrame(Graphics g, Image newImage, Clock clock)
    {
        // Load a custom frame image from disk
        Image image = Image.FromFile(@"C:\Assets\CustomClock.PNG");
        base.DrawDigitalClockFrame(g, image, clock);
    }
}

// 2. Assign to the clock
MyDigitalRenderer render = new MyDigitalRenderer();
this.clock1.DigitalRenderer = render;
```

**VB.NET:**
```vb
Public Class MyDigitalRenderer
    Inherits DigitalClockRenderer

    Public Overrides Sub DrawDigitalClockFrame(ByVal g As Graphics, ByVal newImage As Image, ByVal clock As Clock)
        Dim image As Image = Image.FromFile("C:\Assets\CustomClock.PNG")
        MyBase.DrawDigitalClockFrame(g, image, clock)
    End Sub
End Class

Dim render As New MyDigitalRenderer()
Me.clock1.DigitalRenderer = render
```

> Call `base.DrawDigitalClockFrame` (or `MyBase.DrawDigitalClockFrame` in VB.NET) with your custom image to let the control handle the actual drawing. You can also skip the base call and draw entirely from scratch using `Graphics g`.
