# Slide Transitions and Animation Direction

This reference guide covers the advanced transition effects available for the SplashPanel control, including slide transitions and marquee animations.

## Table of Contents

- [Overview](#overview)
- [Slide Transitions](#slide-transitions)
  - [Default Animation Direction](#default-animation-direction)
  - [LeftToRight Transition](#lefttoright-transition)
  - [RightToLeft Transition](#righttoLeft-transition)
- [Marquee Transitions](#marquee-transitions)
  - [LeftToRight Marquee](#lefttoright-marquee)
  - [RightToLeft Marquee](#righttoLeft-marquee)
  - [TopToBottom Marquee](#toptobottom-marquee)
  - [BottomToTop Marquee](#bottomtotop-marquee)
- [Combining with DesktopAlignment](#combining-with-desktopalignment)
- [Next Steps](#next-steps)

## Overview

The SplashPanel control supports two types of transition effects that enhance the visual presentation of splash screens:

1. **Slide Transitions** - Panel slides into view from various directions
2. **Marquee Transitions** - Panel moves across the screen in a scrolling marquee style

These transitions can be combined with the `DesktopAlignment` property to create sophisticated entry effects.

## Slide Transitions

Slide transitions allow the splash panel to slide into its final position from different directions. These transitions work in conjunction with the `SlideStyle.Slide` setting.

### AnimationDirection Property

The `AnimationDirection` property controls the direction of the slide transition.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| AnimationDirection | AnimationDirection Enum | Gets or sets the slide animation direction |

**Available AnimationDirection Values:**

- **Default** - Standard transition based on desktop alignment
- **LeftToRight** - Slides from left to right
- **RightToLeft** - Slides from right to left

### Default Animation Direction

When the `AnimationDirection` is set to `Default`, the transition direction depends on the `DesktopAlignment` property.

**Behavior:**
- For `LeftBottom` or `RightBottom` alignment, the panel transitions from bottom to top
- The default animation provides a natural entrance effect based on panel position

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Slide;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom;

// Set animation direction
this.splashPanel1.AnimationDirection = AnimationDirection.Default;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Slide

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom

'Set animation direction.
Me.splashPanel1.AnimationDirection = AnimationDirection.Default

'Show panel.
Me.splashPanel1.ShowSplash()
```

### LeftToRight Transition

The `LeftToRight` animation direction creates a slide effect from the left edge of the screen to the right. This works best with `LeftBottom` or `LeftTop` desktop alignments.

**Recommended Alignments:**
- `SplashAlignment.LeftBottom`
- `SplashAlignment.LeftTop`

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Slide;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom;

// Set animation direction
this.splashPanel1.AnimationDirection = AnimationDirection.LeftToRight;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Slide

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom

'Set animation direction.
Me.splashPanel1.AnimationDirection = AnimationDirection.LeftToRight

'Show panel.
Me.splashPanel1.ShowSplash()
```

**C# Example - Complete Configuration:**

```csharp
// Configure complete left-to-right slide
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.AnimationSpeed = 25;
this.splashPanel1.SlideStyle = SlideStyle.Slide;
this.splashPanel1.DesktopAlignment = SplashAlignment.LeftTop;
this.splashPanel1.AnimationDirection = AnimationDirection.LeftToRight;

// Display the splash panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example - Complete Configuration:**

```vb
' Configure complete left-to-right slide
Me.splashPanel1.ShowAnimation = True
Me.splashPanel1.AnimationSpeed = 25
Me.splashPanel1.SlideStyle = SlideStyle.Slide
Me.splashPanel1.DesktopAlignment = SplashAlignment.LeftTop
Me.splashPanel1.AnimationDirection = AnimationDirection.LeftToRight

' Display the splash panel
Me.splashPanel1.ShowSplash()
```

### RightToLeft Transition

The `RightToLeft` animation direction creates a slide effect from the right edge of the screen to the left. This works best with `RightBottom` or `RightTop` desktop alignments.

**Recommended Alignments:**
- `SplashAlignment.RightBottom`
- `SplashAlignment.RightTop`

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Slide;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom;

// Set animation direction
this.splashPanel1.AnimationDirection = AnimationDirection.RightToLeft;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Slide

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom

'Set animation direction.
Me.splashPanel1.AnimationDirection = AnimationDirection.RightToLeft

'Show panel.
Me.splashPanel1.ShowSplash()
```

## Marquee Transitions

Marquee transitions create a scrolling effect where the splash panel moves across the entire screen before settling into its final position. This provides a dynamic, attention-grabbing entrance effect.

### MarqueeDirection Property

The `MarqueeDirection` property controls the direction of the marquee animation.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| MarqueeDirection | MarqueeDirection Enum | Gets or sets the marquee direction |

**Available MarqueeDirection Values:**

- **LeftToRight** - Marquee traverses from left to right
- **RightToLeft** - Marquee traverses from right to left
- **TopToBottom** - Marquee traverses from top to bottom
- **BottomToTop** - Marquee traverses from bottom to top

### LeftToRight Marquee

The `LeftToRight` marquee direction makes the splash panel traverse the screen from the left edge to the right edge.

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Marquee;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom;

// Set marquee direction
this.splashPanel1.MarqueeDirection = MarqueeDirection.LeftToRight;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Marquee

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.LeftBottom

'Set marquee direction.
Me.splashPanel1.MarqueeDirection = MarqueeDirection.LeftToRight

'Show panel.
Me.splashPanel1.ShowSplash()
```

### RightToLeft Marquee

The `RightToLeft` marquee direction makes the splash panel traverse the screen from the right edge to the left edge.

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Marquee;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom;

// Set marquee direction
this.splashPanel1.MarqueeDirection = MarqueeDirection.RightToLeft;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Marquee

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom

'Set marquee direction.
Me.splashPanel1.MarqueeDirection = MarqueeDirection.RightToLeft

'Show panel.
Me.splashPanel1.ShowSplash()
```

### TopToBottom Marquee

The `TopToBottom` marquee direction makes the splash panel traverse the screen from the top edge to the bottom edge.

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Marquee;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom;

// Set marquee direction
this.splashPanel1.MarqueeDirection = MarqueeDirection.TopToBottom;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Marquee

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom

'Set marquee direction.
Me.splashPanel1.MarqueeDirection = MarqueeDirection.TopToBottom

'Show panel.
Me.splashPanel1.ShowSplash()
```

### BottomToTop Marquee

The `BottomToTop` marquee direction makes the splash panel traverse the screen from the bottom edge to the top edge.

**C# Example:**

```csharp
// Set panel slide style
this.splashPanel1.SlideStyle = SlideStyle.Marquee;

// Set panel location
this.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom;

// Set marquee direction
this.splashPanel1.MarqueeDirection = MarqueeDirection.BottomToTop;

// Show panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example:**

```vb
'Set panel slide style.
Me.splashPanel1.SlideStyle = SlideStyle.Marquee

'Set panel location. 
Me.splashPanel1.DesktopAlignment = SplashAlignment.RightBottom

'Set marquee direction.
Me.splashPanel1.MarqueeDirection = MarqueeDirection.BottomToTop

'Show panel.
Me.splashPanel1.ShowSplash()
```

## Combining with DesktopAlignment

For optimal visual effects, combine transition settings with appropriate desktop alignments.

**Best Practice Combinations:**

| Transition Type | Direction | Recommended Alignment |
|----------------|-----------|----------------------|
| Slide | LeftToRight | LeftBottom, LeftTop |
| Slide | RightToLeft | RightBottom, RightTop |
| Slide | Default | LeftBottom, RightBottom |
| Marquee | LeftToRight | Any alignment |
| Marquee | RightToLeft | Any alignment |
| Marquee | TopToBottom | Any alignment |
| Marquee | BottomToTop | Any alignment |

**C# Example - Advanced Configuration:**

```csharp
// Configure marquee transition with animation
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.AnimationSpeed = 30;
this.splashPanel1.ShowAsTopMost = true;
this.splashPanel1.SlideStyle = SlideStyle.Marquee;
this.splashPanel1.MarqueeDirection = MarqueeDirection.LeftToRight;
this.splashPanel1.DesktopAlignment = SplashAlignment.Center;

// Configure timer
this.splashPanel1.TimerInterval = 5000;

// Display the splash panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example - Advanced Configuration:**

```vb
' Configure marquee transition with animation
Me.splashPanel1.ShowAnimation = True
Me.splashPanel1.AnimationSpeed = 30
Me.splashPanel1.ShowAsTopMost = True
Me.splashPanel1.SlideStyle = SlideStyle.Marquee
Me.splashPanel1.MarqueeDirection = MarqueeDirection.LeftToRight
Me.splashPanel1.DesktopAlignment = SplashAlignment.Center

' Configure timer
Me.splashPanel1.TimerInterval = 5000

' Display the splash panel
Me.splashPanel1.ShowSplash()
```

**C# Example - Multiple Transition Scenarios:**

```csharp
private void ShowBottomToTopMarquee()
{
    this.splashPanel1.SlideStyle = SlideStyle.Marquee;
    this.splashPanel1.MarqueeDirection = MarqueeDirection.BottomToTop;
    this.splashPanel1.DesktopAlignment = SplashAlignment.SystemTray;
    this.splashPanel1.ShowSplash();
}

private void ShowRightToLeftSlide()
{
    this.splashPanel1.SlideStyle = SlideStyle.Slide;
    this.splashPanel1.AnimationDirection = AnimationDirection.RightToLeft;
    this.splashPanel1.DesktopAlignment = SplashAlignment.RightTop;
    this.splashPanel1.ShowSplash();
}
```

**VB.NET Example - Multiple Transition Scenarios:**

```vb
Private Sub ShowBottomToTopMarquee()
    Me.splashPanel1.SlideStyle = SlideStyle.Marquee
    Me.splashPanel1.MarqueeDirection = MarqueeDirection.BottomToTop
    Me.splashPanel1.DesktopAlignment = SplashAlignment.SystemTray
    Me.splashPanel1.ShowSplash()
End Sub

Private Sub ShowRightToLeftSlide()
    Me.splashPanel1.SlideStyle = SlideStyle.Slide
    Me.splashPanel1.AnimationDirection = AnimationDirection.RightToLeft
    Me.splashPanel1.DesktopAlignment = SplashAlignment.RightTop
    Me.splashPanel1.ShowSplash()
End Sub
```

## Next Steps

- **[Getting Started](./getting-started.md)** - Learn the basics of implementing SplashPanel
- **[Display Methods](./display-methods.md)** - Explore different ways to show and hide splash panels
- **[Animation and Appearance](./animation-appearance.md)** - Configure visual appearance and basic animations
- **[Events](./events.md)** - Handle splash panel lifecycle events
