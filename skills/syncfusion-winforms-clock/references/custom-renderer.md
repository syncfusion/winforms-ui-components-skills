# Custom Renderer (Analog Clock)

## Overview

The `ClockRenderer` class allows you to fully replace how the analog clock hands and minute lines are drawn. Subclass it, override `DrawInterior`, and assign the instance to `clock1.Renderer`.

The `DrawInterior` method is called once for each visual element (each hand, each minute tick line). Use the `sender` string to identify which element is currently being drawn and apply different pen styles per element.

---

## DrawInterior Signature

```csharp
public override void DrawInterior(
    Graphics g,
    float thickness,
    PointF startPoint,
    PointF endPoint,
    Color color,
    string sender)
```

| Parameter | Description |
|---|---|
| `g` | The `Graphics` context to draw on |
| `thickness` | The default thickness for this element |
| `startPoint` | Start point of the line (hand root or tick start) |
| `endPoint` | End point of the line (hand tip or tick end) |
| `color` | The configured color for this element |
| `sender` | Identifies which element is being drawn (see table below) |

### Sender Values

| `sender` value | Element |
|---|---|
| `"SecondsHand"` | Second hand |
| `"MinutesHand"` | Minute hand |
| `"HoursHand"` | Hour hand |
| *(anything else)* | Minute tick lines on the clock face |

---

## Implementation Example

**C#:**
```csharp
using System.Drawing;
using System.Drawing.Drawing2D;
using Syncfusion.Windows.Forms.Tools;

public class CustomRenderer : ClockRenderer
{
    public override void DrawInterior(
        Graphics g,
        float thickness,
        PointF startPoint,
        PointF endPoint,
        Color color,
        string sender)
    {
        g.SmoothingMode = SmoothingMode.AntiAlias;

        if (sender == "SecondsHand")
        {
            Pen p = new Pen(color, thickness + thickness);
            p.StartCap = LineCap.SquareAnchor;
            p.EndCap   = LineCap.ArrowAnchor;
            g.DrawLine(p, startPoint, endPoint);
            p.Dispose();
        }
        else if (sender == "MinutesHand")
        {
            Pen p = new Pen(color, thickness + thickness);
            p.StartCap = LineCap.SquareAnchor;
            p.EndCap   = LineCap.ArrowAnchor;
            g.DrawLine(p, startPoint, endPoint);
            p.Dispose();
        }
        else if (sender == "HoursHand")
        {
            Pen p = new Pen(color, thickness + thickness);
            p.StartCap = LineCap.SquareAnchor;
            p.EndCap   = LineCap.ArrowAnchor;
            g.DrawLine(p, startPoint, endPoint);
            p.Dispose();
        }
        else
        {
            // Minute tick lines
            Pen p = new Pen(color, 5);
            p.DashStyle = DashStyle.Dot;
            g.DrawLine(p, startPoint, endPoint);
            p.Dispose();
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Drawing
Imports System.Drawing.Drawing2D
Imports Syncfusion.Windows.Forms.Tools

Public Class CustomRenderer
    Inherits ClockRenderer

    Public Overrides Sub DrawInterior(
        ByVal g As Graphics,
        ByVal thickness As Single,
        ByVal startPoint As PointF,
        ByVal endPoint As PointF,
        ByVal color As Color,
        ByVal sender As String)

        g.SmoothingMode = SmoothingMode.AntiAlias

        If sender = "SecondsHand" Then
            Dim p As New Pen(color, thickness + thickness)
            p.StartCap = LineCap.SquareAnchor
            p.EndCap   = LineCap.ArrowAnchor
            g.DrawLine(p, startPoint, endPoint)
            p.Dispose()
        ElseIf sender = "MinutesHand" Then
            Dim p As New Pen(color, thickness + thickness)
            p.StartCap = LineCap.SquareAnchor
            p.EndCap   = LineCap.ArrowAnchor
            g.DrawLine(p, startPoint, endPoint)
            p.Dispose()
        ElseIf sender = "HoursHand" Then
            Dim p As New Pen(color, thickness + thickness)
            p.StartCap = LineCap.SquareAnchor
            p.EndCap   = LineCap.ArrowAnchor
            g.DrawLine(p, startPoint, endPoint)
            p.Dispose()
        Else
            Dim p As New Pen(color, 5)
            p.DashStyle = DashStyle.Dot
            g.DrawLine(p, startPoint, endPoint)
            p.Dispose()
        End If
    End Sub
End Class
```

---

## Assigning the Renderer

**C#:**
```csharp
CustomRenderer renderer = new CustomRenderer();
this.clock1.Renderer = renderer;
```

**VB.NET:**
```vb
Dim renderer As New CustomRenderer()
Me.clock1.Renderer = renderer
```

---

## Tips and Patterns

- **Always dispose pens** after use to prevent GDI resource leaks.
- **Always set `SmoothingMode = AntiAlias`** at the start of each call for smooth lines.
- Use `LineCap.ArrowAnchor` on `EndCap` to give hands a pointed tip; `LineCap.SquareAnchor` on `StartCap` anchors the root cleanly.
- The `else` branch (minute ticks) is called many times per render (once per tick mark) — keep it lightweight.
- You do not need to call a base method; `DrawInterior` replaces the default drawing entirely.
