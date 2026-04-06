# Scrollbar Visibility Features in Windows Forms Splitter Control

This guide covers how to control the visibility of horizontal and vertical scrollbars in the SplitterControl, enabling clean, minimalist interfaces or focused user experiences.

## Overview

The SplitterControl provides granular control over scrollbar visibility through two independent properties:
- `ShowHorizontalScrollBar` - Controls horizontal scrollbar visibility
- `ShowVerticalScrollBar` - Controls vertical scrollbar visibility

These properties allow you to:
- Create clean, minimalist interfaces
- Hide scrollbars when not needed
- Control which scrolling directions are available
- Optimize screen real estate
- Match specific design requirements

## Controlling Scrollbar Visibility

### ShowHorizontalScrollBar Property

Controls whether the horizontal scrollbar is visible.

**Type:** `bool` | **Default:** `true`

```csharp
this.splitterControl1.ShowHorizontalScrollBar = false; // Hide horizontal scrollbar
```

**Visual Result:** When set to `false`, the horizontal scrollbar is hidden, providing more vertical space.

### ShowVerticalScrollBar Property

Controls whether the vertical scrollbar is visible.

**Type:** `bool` | **Default:** `true`

```csharp
this.splitterControl1.ShowVerticalScrollBar = false; // Hide vertical scrollbar
```

**Visual Result:** When set to `false`, the vertical scrollbar is hidden, providing more horizontal space.

## Common Configurations

```csharp
// Hide horizontal scrollbar only (portrait-oriented content)
splitterControl1.ShowHorizontalScrollBar = false;
splitterControl1.ShowVerticalScrollBar = true;

// Hide vertical scrollbar only (wide tables, timelines)
splitterControl1.ShowHorizontalScrollBar = true;
splitterControl1.ShowVerticalScrollBar = false;

// Hide both scrollbars (touch UI, minimalist design)
splitterControl1.ShowHorizontalScrollBar = false;
splitterControl1.ShowVerticalScrollBar = false;

// Show both scrollbars - Default (spreadsheets, large datasets)
splitterControl1.ShowHorizontalScrollBar = true;
splitterControl1.ShowVerticalScrollBar = true;
```

## Implementation Examples

### Minimalist Touch Interface

```csharp
splitterControl1.ShowHorizontalScrollBar = false;
splitterControl1.ShowVerticalScrollBar = false;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;
splitterControl1.SplitBars = DynamicSplitBars.SplitColumns;
```

### Document Reader (Vertical Scrolling)

```csharp
splitterControl1.ShowHorizontalScrollBar = false;
splitterControl1.ShowVerticalScrollBar = true;
splitterControl1.SplitBars = DynamicSplitBars.SplitRows;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
```

### Wide Table Viewer (Horizontal Scrolling)

```csharp
splitterControl1.ShowHorizontalScrollBar = true;
splitterControl1.ShowVerticalScrollBar = false;
splitterControl1.SplitBars = DynamicSplitBars.None;
```

## Dynamic Scrollbar Visibility

```csharp
// Toggle scrollbars
private void ToggleScrollbars()
{
    splitterControl1.ShowHorizontalScrollBar = !splitterControl1.ShowHorizontalScrollBar;
    splitterControl1.ShowVerticalScrollBar = !splitterControl1.ShowVerticalScrollBar;
}

// Context-based visibility
private void AdjustScrollbarVisibility()
{
    splitterControl1.ShowHorizontalScrollBar = this.ClientSize.Width <= 1200;
    splitterControl1.ShowVerticalScrollBar = this.ClientSize.Height <= 800;
}
```

## Best Practices

**When to Hide Scrollbars:**
- Touch-optimized interfaces, minimalist design, kiosk modes
- Content fits within viewport

**Accessibility:** Ensure users can still scroll via keyboard, mouse wheel, or touch when scrollbars are hidden.

## Troubleshooting

**Scrollbars still appear:** Set property after control initialization.
**Can't scroll:** Users can still scroll using mouse wheel, keyboard arrows, or touch gestures.
**Scrollbars reappear:** Check if other code resets the property after layout changes.

## Quick Reference

| Configuration | Horizontal | Vertical | Use Case |
|--------------|-----------|----------|----------|
| Both visible (default) | `true` | `true` | Standard data grids |
| Horizontal only | `true` | `false` | Wide tables, timelines |
| Vertical only | `false` | `true` | Documents, feeds |
| Neither visible | `false` | `false` | Touch UI, kiosks |
