# Scroll Settings and Events

Guide to configuring scrollable content and handling events in GradientPanelExt for interactive panel implementations.

## Table of Contents
- [Scroll Settings](#scroll-settings)
- [Events Overview](#events-overview)
- [Common Events](#common-events)
- [Primitive Events](#primitive-events)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Scroll Settings

GradientPanelExt inherits scroll functionality from Panel, allowing scrollable content when child controls exceed panel bounds.

### AutoScroll Property

Enables automatic scrollbars when content overflows.

**Property Type:** `bool`  
**Default Value:** `false`

**C# Example:**
```csharp
gradientPanel.AutoScroll = true;   // Enable scrollbars
gradientPanel.AutoScroll = false;  // Disable scrollbars
```

**VB.NET Example:**
```vb
gradientPanel.AutoScroll = True   ' Enable scrollbars
gradientPanel.AutoScroll = False  ' Disable scrollbars
```

---

### Scrollable Panel Example

# Scroll Settings and Events (trimmed)

Compact reference for scrolling and events. Keeps one VB example only.

## Scrolling (C#)

```csharp
// Enable scrolling
gradientPanel.AutoScroll = true;
gradientPanel.AutoScrollMinSize = new Size(400, 600);
```

**VB (compact):**
```vb
' Enable scrolling (single VB sample)
gradientPanel.AutoScroll = True
```

## Common Events (C# compact)

```csharp
gradientPanel.Click += (s,e) => MessageBox.Show("Panel clicked");
gradientPanel.PrimitiveClick += (s,e) => { /* check e.Primitive */ };
gradientPanel.Paint += (s,e) => { /* custom paint */ };
```

## Best Practices
- Unsubscribe event handlers in `Dispose` to avoid memory leaks.
- Use `AutoScroll` for dynamic content and test content height vs panel height.

## Related
- Primitives: [primitives.md](primitives.md)
- Animation: [collapse-expand-animation.md](collapse-expand-animation.md)

```csharp
// In Form_FormClosing or Dispose method
gradientPanel.Click -= GradientPanel_Click;
gradientPanel.MouseEnter -= GradientPanel_MouseEnter;
gradientPanel.PrimitiveClick -= GradientPanel_PrimitiveClick;
```

### 3. Use Event Args to Identify Primitives

```csharp
private void Panel_PrimitiveClick(object sender, PrimitiveEventArgs e)
{
    // Type checking
    if (e.Primitive is TextPrimitive textPrim)
    {
        Console.WriteLine($"Text primitive clicked: {textPrim.Text}");
    }
    else if (e.Primitive is ImagePrimitive imgPrim)
    {
        Console.WriteLine("Image primitive clicked");
    }
}
```

### 4. Handle Exceptions in Event Handlers

```csharp
private void GradientPanel_Click(object sender, EventArgs e)
{
    try
    {
        // Your logic
        PerformAction();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error: {ex.Message}", "Error", 
            MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
```

---

## Troubleshooting

### Scrollbars Not Appearing

**Check:**
1. AutoScroll = true
2. Child controls actually exceed panel bounds
3. Child control locations are set correctly

```csharp
// Verify AutoScroll
Debug.WriteLine($"AutoScroll: {panel.AutoScroll}");

// Check if content exceeds bounds
int maxY = 0;
foreach (Control ctrl in panel.Controls)
{
    maxY = Math.Max(maxY, ctrl.Bottom);
}
Debug.WriteLine($"Content height: {maxY}, Panel height: {panel.Height}");
```

### Events Not Firing

**Check:**
- Event is subscribed (use += operator)
- Control is enabled
- No other control is blocking clicks

```csharp
// Verify subscription
Debug.WriteLine($"Click event count: {panel.Click?.GetInvocationList().Length ?? 0}");
```

### Primitive Click Not Working

**Check:**
- PrimitiveClick event is subscribed
- Primitive is added to Primitives collection
- Primitive size and position are within panel bounds

```csharp
// Verify primitives
Debug.WriteLine($"Primitive count: {panel.Primitives.Count}");
```

### Memory Leaks from Events

**Solution:** Always unsubscribe in Dispose

```csharp
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        // Unsubscribe all events
        gradientPanel.Click -= GradientPanel_Click;
        gradientPanel.PrimitiveClick -= GradientPanel_PrimitiveClick;
    }
    base.Dispose(disposing);
}
```

---

## Related Topics

- **Primitives**: Primitive types and usage → [primitives.md](primitives.md)
- **Collapse Animation**: Animation events → [collapse-expand-animation.md](collapse-expand-animation.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
