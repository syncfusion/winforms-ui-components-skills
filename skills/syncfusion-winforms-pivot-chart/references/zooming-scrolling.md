# Zooming and Scrolling

This guide covers zoom and scroll functionality for exploring large datasets in Pivot Charts.

## Overview

Zooming and scrolling provide interactive exploration of chart data, especially useful for large datasets where all data points cannot be displayed simultaneously.

## Enabling Zooming

```csharp
// Enable zoom functionality
pivotChart1.AllowZooming = true;
```

## Zoom Configuration

```csharp
// Configure zoom options
pivotChart1.ZoomFactorX = 1.5;  // 150% zoom on X-axis
pivotChart1.ZoomFactorY = 1.2;  // 120% zoom on Y-axis

// Allow mouse wheel zooming
pivotChart1.MouseWheelZoomingEnabled = true;
```

## Scrolling

```csharp
// Enable scrollbars
pivotChart1.ShowScrollBars = true;

// Configure scroll behavior
pivotChart1.HorizontalScrollMode = ScrollMode.Continuous;
pivotChart1.VerticalScrollMode = ScrollMode.Continuous;
```

## Interactive Zoom Controls

```csharp
// Add zoom buttons
private void btnZoomIn_Click(object sender, EventArgs e)
{
    pivotChart1.ZoomFactorX *= 1.2;
    pivotChart1.ZoomFactorY *= 1.2;
}

private void btnZoomOut_Click(object sender, EventArgs e)
{
    pivotChart1.ZoomFactorX /= 1.2;
    pivotChart1.ZoomFactorY /= 1.2;
}

private void btnResetZoom_Click(object sender, EventArgs e)
{
    pivotChart1.ZoomFactorX = 1.0;
    pivotChart1.ZoomFactorY = 1.0;
}
```

## Mouse Wheel Zoom

```csharp
// Configure mouse wheel zoom
pivotChart1.MouseWheelZoomingEnabled = true;
pivotChart1.MouseWheelZoomDelta = 0.1;  // 10% per scroll
```

## Best Practices

1. Enable zooming for large datasets (>50 data points)
2. Provide zoom reset button
3. Show current zoom level to users
4. Test scroll performance with real data volumes
5. Consider touch-friendly zoom for tablet devices
