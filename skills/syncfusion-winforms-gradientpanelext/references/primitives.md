# Primitives

Comprehensive guide to using primitives - the unique feature of GradientPanelExt that allows hosting text, images, collapse controls, and even .NET controls in panel borders.

## Table of Contents
- [Primitives Overview](#primitives-overview)
- [CollapsePrimitive](#collapseprimitive)
- [ImagePrimitive](#imageprimitive)
- [TextPrimitive](#textprimitive)
- [HostPrimitive](#hostprimitive)
- [Common Properties](#common-properties)
- [PrimitiveCollection Editor](#primitivecollection-editor)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Primitives Overview

**Primitives** are UI elements that can be placed in the borders (Top, Bottom, Left, Right) of a GradientPanelExt. They don't occupy space inside the panel - they're positioned in the border areas.

### Types of Primitives

| Primitive | Description | Use Case |
|-----------|-------------|----------|
| **CollapsePrimitive** | Expand/collapse button with images | Collapsible sections |
| **ImagePrimitive** | Static image in border | Logos, icons, decorations |
| **TextPrimitive** | Text label in border | Titles, captions, buttons |
| **HostPrimitive** | Hosts any .NET control | Buttons, progress bars, custom controls |

### Key Concept

Primitives are **border-hosted** - they sit in the panel's edge areas (top, bottom, left, right margins), not in the main content area where child controls go.

---

## CollapsePrimitive

Provides expand/collapse functionality with clickable images.

**Namespace:** `Syncfusion.Windows.Forms.Tools.CollapsePrimitive`

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **CollapseImage** | Image | Image shown when panel is expanded (click to collapse) |
| **ExpandImage** | Image | Image shown when panel is collapsed (click to expand) |
| **Alignment** | Alignment enum | Border position (Top, Bottom, Left, Right) |
| **Position** | int | Pixel offset along the border |
| **Size** | Size | Width and height of the image button |

### Basic Collapse Primitive

**C# Example:**
```csharp
// Create collapse primitive
CollapsePrimitive collapseButton = new CollapsePrimitive

# Primitives (trimmed)

Overview of the primitives feature with compact C# examples and a single VB sample for parity.

## Types (summary)
- `CollapsePrimitive`: expand/collapse image button
- `ImagePrimitive`: place images in borders
- `TextPrimitive`: text labels or button-like elements in borders
- `HostPrimitive`: host any `Control` in a border slot

## Compact Examples (C#)

```csharp
// Collapse button (C#)
var collapse = new CollapsePrimitive { Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top, Position = 350, Size = new Size(30,30) };
collapse.CollapseImage = Properties.Resources.CollapseIcon;
collapse.ExpandImage = Properties.Resources.ExpandIcon;
gradientPanel.Primitives.Add(collapse);

// Image primitive
var logo = new ImagePrimitive { Image = Properties.Resources.Logo, Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top, Position = 10, Size = new Size(32,32) };
gradientPanel.Primitives.Add(logo);

// Host a control
var btn = new Button { Text = "Settings" };
var host = new HostPrimitive { HostControl = btn, Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top, Position = 300, Size = new Size(80,25) };
gradientPanel.Primitives.Add(host);
```

**VB (single):**
```vb
' Single compact VB example
Dim collapse As New CollapsePrimitive With {.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top, .Position = 350, .Size = New Size(30,30)}
collapse.CollapseImage = My.Resources.CollapseIcon
collapse.ExpandImage = My.Resources.ExpandIcon
gradientPanel.Primitives.Add(collapse)
```

## Notes
- Primitives live in borders, not the content area.
- Use transparent `BackColor` to let the gradient show through.

## Related
- Getting started: [getting-started.md](getting-started.md)

```csharp
primitive1.Position = 10;
primitive2.Position = primitive1.Position + primitive1.Size.Width + 10;  // 10px gap
```

---

## Related Topics

- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Collapse Animation**: Animation settings → [collapse-expand-animation.md](collapse-expand-animation.md)
- **Events**: Handling clicks → [scroll-settings-events.md](scroll-settings-events.md)
