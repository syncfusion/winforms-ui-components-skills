# Border and Corner Settings

Guide to customizing corner radius and border gaps in GradientPanelExt for rounded or sharp panel edges.

## Overview

GradientPanelExt provides two key border properties:
- **CornerRadius**: Rounds panel corners
- **BorderGap**: Adds spacing between border and margins

These properties allow you to create modern rounded panels or classic sharp-edged rectangles.

---

## CornerRadius Property

Controls the roundness of panel corners. GradientPanelExt is rounded by default.

**Property Type:** `int`  
**Default Value:** Non-zero (rounded)  
**Range:** 0 (sharp corners) to high values (very rounded)

### Setting Corner Radius

**C# Example:**
```csharp
// Rounded corners (default behavior)
gradientPanel.CornerRadius = 10;

// More rounded
gradientPanel.CornerRadius = 20;

// Sharp corners (remove rounding)
gradientPanel.CornerRadius = 0;
```

**VB.NET Example:**
```vb
' Rounded corners
gradientPanel.CornerRadius = 10

' Sharp corners
gradientPanel.CornerRadius = 0
```

---

### Sharp Corners (CornerRadius = 0)

Creates traditional rectangle with 90-degree corners.

**C# Example:**
```csharp

# Border and Corner Settings (trimmed)

Short guide to `CornerRadius` and `BorderGap` with compact examples. Keeps a single VB sample.

## CornerRadius

Controls roundness of panel corners (0 = sharp).

```csharp
// Light rounding
gradientPanel.CornerRadius = 10;

// Sharp corners
gradientPanel.CornerRadius = 0;
```

**VB (compact):**
```vb
' Set rounded corners
gradientPanel.CornerRadius = 10
```

## BorderGap

Pixel spacing between border and margins.

```csharp
gradientPanel.BorderGap = 8; // small inset
```

## Best Practices
- Use small CornerRadius for subtle rounding (6–12).
- Adjust `BorderGap` when primitives are used in borders.
- Keep consistent radius across related panels.

## Related
- Getting started: [getting-started.md](getting-started.md)
- Background styling: [background-styling.md](background-styling.md)


```csharp
panel.BorderGap = 15;
panel.Size = new Size(400, 300);

// Position child controls away from edges
button.Location = new Point(20 + 15, 20 + 15);  // Add BorderGap offset
```

### Inconsistent Appearance

**Solution:** Verify CornerRadius is same across similar panels

```csharp
// Check current value
int currentRadius = panel.CornerRadius;
System.Diagnostics.Debug.WriteLine($"CornerRadius: {currentRadius}");
```

---

## Related Topics

- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Background Styling**: Gradients → [background-styling.md](background-styling.md)
- **Primitives**: Border elements → [primitives.md](primitives.md)
