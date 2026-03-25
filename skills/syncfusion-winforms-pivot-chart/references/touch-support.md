# Touch Support

This guide covers touch gesture support for tablet and touch-enabled devices.

## Overview

The Pivot Chart control provides touch-friendly interactions for tablets and touch-enabled Windows devices.

## Enabling Touch Support

```csharp
// Enable touch interactions
pivotChart1.EnableTouchMode = true;
```

## Supported Gestures

### Touch Gestures
- **Tap:** Select data points or drill down
- **Swipe:** Scroll through chart data
- **Pinch:** Zoom in/out on chart
- **Two-finger drag:** Pan across chart area

## Touch Configuration

```csharp
// Configure touch settings
pivotChart1.EnableTouchMode = true;
pivotChart1.TouchSensitivity = TouchSensitivity.High;
pivotChart1.MinimumTouchSize = 44;  // Minimum 44px for touch targets (Microsoft guidelines)
```

## Touch-Friendly UI Elements

```csharp
// Increase expander size for touch
pivotChart1.ExpanderSize = new Size(40, 40);  // Larger touch targets

// Increase legend item size
pivotChart1.Legend.ItemsSize = new Size(200, 40);
```

## Best Practices

1. **Target Size:** Minimum 44x44 pixels for touch targets
2. **Spacing:** Adequate space between interactive elements
3. **Feedback:** Visual feedback for touch interactions
4. **Test:** Test on actual touch devices
5. **Gestures:** Support standard Windows touch gestures

## Testing

Test these interactions on touch devices:
- Single tap to select/drill-down
- Pinch to zoom
- Swipe to scroll
- Two-finger pan to navigate
