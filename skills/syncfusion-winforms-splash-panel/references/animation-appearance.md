# Animation and Appearance Configuration

This reference guide covers animation settings, visual appearance, and behavior customization for the SplashPanel control in Windows Forms applications.

## Table of Contents

- [Animation Settings](#animation-settings)
  - [ShowAnimation Property](#showanimation-property)
  - [AnimationSpeed Property](#animationspeed-property)
  - [ShowAsTopMost Property](#showastopmost-property)
- [Slide Styles](#slide-styles)
  - [SlideStyle Options](#slidestyle-options)
  - [Configuration Examples](#configuration-examples)
- [Background Settings](#background-settings)
  - [BackgroundColor with BrushInfo](#backgroundcolor-with-brushinfo)
  - [GradientStyle Options](#gradientstyle-options)
  - [BackgroundImage](#backgroundimage)
  - [TransparentColor](#transparentcolor)
  - [RefreshRegionFromImage Method](#refreshregionfromimage-method)
- [Border Settings](#border-settings)
  - [BorderStyle Options](#borderstyle-options)
  - [BorderType Options](#bordertype-options)
- [Desktop Alignment](#desktop-alignment)
  - [Alignment Options](#alignment-options)
  - [Custom Positioning](#custom-positioning)
- [Behavior Settings](#behavior-settings)
  - [AllowMove Property](#allowmove-property)
  - [AllowResize Property](#allowresize-property)
  - [CloseOnClick Property](#closeonclick-property)
  - [SuspendAutoCloseWhenMouseOver Property](#suspendautoclosewhen-mouseover-property)
  - [Auto Close Methods](#auto-close-methods)
- [ToolTip Support](#tooltip-support)
- [Next Steps](#next-steps)

## Animation Settings

The SplashPanel control provides comprehensive animation capabilities to enhance the visual presentation of splash screens during application startup.

### ShowAnimation Property

The `ShowAnimation` property controls whether the splash panel window displays with animation effects.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| ShowAnimation | Boolean | Specifies if the window display should be animated |

### AnimationSpeed Property

The `AnimationSpeed` property determines the speed at which the animation unfolds on the screen and the SplashPanel becomes visible.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| AnimationSpeed | Integer | Indicates the speed at which the animation unfolds on the screen |

### ShowAsTopMost Property

The `ShowAsTopMost` property ensures the splash panel appears above all other windows during display.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| ShowAsTopMost | Boolean | Specifies if the SplashPanel is to be displayed as a topmost window |

**C# Example:**

```csharp
// Configure animation settings
this.splashPanel1.AnimationSpeed = 30;
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.ShowAsTopMost = true;
```

**VB.NET Example:**

```vb
' Configure animation settings
Me.splashPanel1.AnimationSpeed = 30
Me.splashPanel1.ShowAnimation = True
Me.splashPanel1.ShowAsTopMost = True
```

## Slide Styles

The splash panel can be displayed with different animation styles using the `SlideStyle` property.

### SlideStyle Options

By default, the splash image draws from left to right when animation is enabled. However, you can customize this behavior with various slide styles.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| SlideStyle | SlideStyle Enum | Gets or sets the slide style for the SplashPanel |

**Available SlideStyle Values:**

- **Horizontal** - Slides horizontally across the screen
- **Vertical** - Slides vertically across the screen
- **LeftToRight** - Animates from left to right
- **RightToLeft** - Animates from right to left
- **TopToBottom** - Animates from top to bottom
- **BottomToTop** - Animates from bottom to top
- **FadeIn** - Fades in gradually

> **Note:** The `ShowAnimation` property must be set to `true` for slide styles to take effect.

### Configuration Examples

**C# Example - FadeIn Effect:**

```csharp
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.SlideStyle = Syncfusion.Windows.Forms.Tools.SlideStyle.FadeIn;
```

**VB.NET Example - FadeIn Effect:**

```vb
Me.splashPanel1.ShowAnimation = True
Me.splashPanel1.SlideStyle = Syncfusion.Windows.Forms.Tools.SlideStyle.FadeIn
```

**C# Example - RightToLeft Effect:**

```csharp
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.SlideStyle = Syncfusion.Windows.Forms.Tools.SlideStyle.RightToLeft;
this.splashPanel1.AnimationSpeed = 25;
```

**VB.NET Example - RightToLeft Effect:**

```vb
Me.splashPanel1.ShowAnimation = True
Me.splashPanel1.SlideStyle = Syncfusion.Windows.Forms.Tools.SlideStyle.RightToLeft
Me.splashPanel1.AnimationSpeed = 25
```

## Background Settings

The SplashPanel provides extensive customization options for the background appearance including colors, gradients, images, and transparency.

### BackgroundColor with BrushInfo

The `BackgroundColor` property uses the `BrushInfo` class to configure gradient and other advanced background styles.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| BackgroundColor | BrushInfo | Gets or sets the background gradient and other styles |

**BrushInfo Properties:**

| Property | Type | Description |
|----------|------|-------------|
| Style | BrushStyle | Specifies the brush style: Solid, Pattern, or Gradient |
| BackColor | Color | Specifies the back color of the control |
| ForeColor | Color | Specifies the fore color for any text or graphics in the control |
| GradientColors | Color Array | Specifies the gradient colors array |

### GradientStyle Options

When using gradient backgrounds, you can choose from multiple gradient styles:

**Available GradientStyle Values:**

- **ForwardDiagonal** - Diagonal gradient from top-left to bottom-right
- **BackwardDiagonal** - Diagonal gradient from top-right to bottom-left
- **Horizontal** - Horizontal gradient from left to right
- **Vertical** - Vertical gradient from top to bottom
- **PathRectangle** - Rectangular path gradient
- **PathEllipse** - Elliptical path gradient

### BackgroundImage

The `BackgroundImage` property allows you to set a custom image as the splash panel background.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| BackgroundImage | Image | Specifies the background image used for the control |

### TransparentColor

The `TransparentColor` property defines which color in the background should be treated as transparent.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| TransparentColor | Color | Specifies the transparent color for the background |

### RefreshRegionFromImage Method

The `RefreshRegionFromImage()` method can be used to refresh the region from the background image, ensuring the image displays correctly.

**C# Example - Complete Background Configuration:**

```csharp
// Configure gradient background
this.splashPanel1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.PathRectangle, 
    System.Drawing.Color.AliceBlue, 
    System.Drawing.Color.SteelBlue);

// Set background image
this.splashPanel1.BackgroundImage = ((System.Drawing.Image)(resources.GetObject("blue hills")));

// Set transparent color
this.splashPanel1.TransparentColor = System.Drawing.Color.White;

// Refresh region from image
this.splashPanel1.RefreshRegionFromImage();
```

**VB.NET Example - Complete Background Configuration:**

```vb
' Configure gradient background
Me.splashPanel1.BackgroundColor = New Syncfusion.Drawing.BrushInfo( _
    Syncfusion.Drawing.GradientStyle.PathRectangle, _
    System.Drawing.Color.AliceBlue, _
    System.Drawing.Color.SteelBlue)

' Set background image
Me.splashPanel1.BackgroundImage = CType((resources.GetObject("blue hills")), System.Drawing.Image)

' Set transparent color
Me.splashPanel1.TransparentColor = System.Drawing.Color.White

' Refresh region from image
Me.splashPanel1.RefreshRegionFromImage()
```

## Border Settings

The SplashPanel control supports customizable 3D borders to enhance visual appearance.

### BorderStyle Options

The `BorderStyle` property provides various 3D border effects.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| BorderStyle | Border3DStyle Enum | Specifies the 3D border for the SplashPanel |

**Available Border3DStyle Values:**

- **RaisedOuter** - Raised outer edge
- **SunkenOuter** - Sunken outer edge
- **RaisedInner** - Raised inner edge
- **SunkenInner** - Sunken inner edge
- **Raised** - Fully raised border
- **Etched** - Etched border effect
- **Bump** - Bumped border effect
- **Sunken** - Fully sunken border
- **Adjust** - Adjusted border
- **Flat** - Flat border with no 3D effect

### BorderType Options

The `BorderType` property determines whether a border is displayed.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| BorderType | SplashBorderType Enum | Specifies the type of border |

**Available BorderType Values:**

- **Border3D** - Display a 3D border
- **None** - No border displayed

**C# Example:**

```csharp
this.splashPanel1.BorderStyle = System.Windows.Forms.Border3DStyle.Flat;
this.splashPanel1.BorderType = Syncfusion.Windows.Forms.Tools.SplashBorderType.Border3D;
```

**VB.NET Example:**

```vb
Me.splashPanel1.BorderStyle = System.Windows.Forms.Border3DStyle.Flat
Me.splashPanel1.BorderType = Syncfusion.Windows.Forms.Tools.SplashBorderType.Border3D
```

## Desktop Alignment

The SplashPanel can be positioned at various locations on the desktop screen.

### Alignment Options

The `DesktopAlignment` property controls where the splash panel appears on the screen.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| DesktopAlignment | SplashAlignment Enum | Specifies the desktop alignment of the splash image |

**Available SplashAlignment Values:**

- **Center** - Center of the screen
- **SystemTray** - Near the system tray area
- **LeftTop** - Top-left corner of the screen
- **LeftBottom** - Bottom-left corner of the screen
- **RightTop** - Top-right corner of the screen
- **RightBottom** - Bottom-right corner of the screen
- **Custom** - Custom position (use with `DiscreetLocation` property)

### Custom Positioning

When using `Custom` alignment, you can specify the exact position using the `DiscreetLocation` property.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| DiscreetLocation | Point | Gets or sets the location to display the splash window |

**C# Example:**

```csharp
// Position at system tray
this.splashPanel1.DesktopAlignment = Syncfusion.Windows.Forms.Tools.SplashAlignment.SystemTray;
```

**VB.NET Example:**

```vb
' Position at system tray
Me.splashPanel1.DesktopAlignment = Syncfusion.Windows.Forms.Tools.SplashAlignment.SystemTray
```

**C# Example - Custom Position:**

```csharp
// Set custom alignment
this.splashPanel1.DesktopAlignment = Syncfusion.Windows.Forms.Tools.SplashAlignment.Custom;
this.splashPanel1.DiscreetLocation = new Point(200, 150);

// Display the splash panel
this.splashPanel1.ShowSplash();
```

**VB.NET Example - Custom Position:**

```vb
' Set custom alignment
Me.splashPanel1.DesktopAlignment = Syncfusion.Windows.Forms.Tools.SplashAlignment.Custom
Me.splashPanel1.DiscreetLocation = New Point(200, 150)

' Display the splash panel
Me.splashPanel1.ShowSplash()
```

## Behavior Settings

The SplashPanel provides several properties to control user interaction behavior during runtime.

### AllowMove Property

The `AllowMove` property determines whether users can reposition the splash panel.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| AllowMove | Boolean | Indicates whether the SplashPanel can be moved by the user at run time |

When set to `true`, users can click and drag the splash panel to a new position on the screen.

### AllowResize Property

The `AllowResize` property controls whether users can resize the splash panel.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| AllowResize | Boolean | Indicates whether the SplashPanel can be resized by the user at run time |

When set to `true`, resize handles appear when the user moves the mouse near the border of the splash panel.

### CloseOnClick Property

The `CloseOnClick` property enables users to close the splash panel with a single mouse click.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| CloseOnClick | Boolean | Indicates whether the SplashPanel gets closed when the user clicks on it |

### SuspendAutoCloseWhenMouseOver Property

The `SuspendAutoCloseWhenMouseOver` property controls auto-close behavior when the mouse hovers over the splash panel.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| SuspendAutoCloseWhenMouseOver | Boolean | Indicates whether the SplashPanel should not be closed when the mouse is over it |

By default, this property is set to `false`. When enabled, the splash panel will not auto-close while the mouse is positioned over it.

> **Note:** When `AllowMove` or `AllowResize` is enabled, the splash panel will not close until the host form is closed.

**C# Example:**

```csharp
this.splashPanel1.AllowMove = true;
this.splashPanel1.AllowResize = true;
this.splashPanel1.CloseOnClick = true;
this.splashPanel1.SuspendAutoCloseWhenMouseOver = true;
```

**VB.NET Example:**

```vb
Me.splashPanel1.AllowMove = True
Me.splashPanel1.AllowResize = True
Me.splashPanel1.CloseOnClick = True
Me.splashPanel1.SuspendAutoCloseWhenMouseOver = True
```

### Auto Close Methods

The SplashPanel provides methods to control the auto-close behavior programmatically.

#### SuspendAutoCloseMode Method

The `SuspendAutoCloseMode()` method suspends the automatic closing of the splash panel after the timer interval.

**Method Details:**

| Method | Description |
|--------|-------------|
| SuspendAutoCloseMode() | Suspends the auto closing of the SplashPanel after the TimerInterval |

When called, the splash panel will remain visible until the application closes or the `RestoreAutoCloseMode()` method is invoked.

**C# Example:**

```csharp
private void button1_Click(object sender, EventArgs e)
{
    this.splashPanel1.SuspendAutoCloseMode();
}
```

**VB.NET Example:**

```vb
Private Sub button1_Click(ByVal sender As Object, ByVal e As EventArgs)
    Me.splashPanel1.SuspendAutoCloseMode()
End Sub
```

#### RestoreAutoCloseMode Method

The `RestoreAutoCloseMode()` method restores the automatic closing behavior that was suspended.

**Method Details:**

| Method | Description |
|--------|-------------|
| RestoreAutoCloseMode() | Restores the auto closing of the SplashPanel |

**C# Example:**

```csharp
private void button1_Click(object sender, EventArgs e)
{
    this.splashPanel1.RestoreAutoCloseMode();
}
```

**VB.NET Example:**

```vb
Private Sub button1_Click(ByVal sender As Object, ByVal e As EventArgs)
    Me.splashPanel1.RestoreAutoCloseMode()
End Sub
```

## ToolTip Support

The SplashPanel supports displaying tooltips when the mouse hovers over the control.

**Property Details:**

| Property | Type | Description |
|----------|------|-------------|
| ToolTip on toolTip1 | String | Determines the ToolTip shown when the mouse hovers over the control |

> **Note:** The `ToolTip on toolTip1` property is an extender property that appears in the Property grid only when you add a ToolTip control to the form.

**C# Example:**

```csharp
// Add a ToolTip component to the form
private System.Windows.Forms.ToolTip toolTip1;

// In the constructor or initialization code
this.toolTip1 = new System.Windows.Forms.ToolTip();
this.toolTip1.SetToolTip(this.splashPanel1, "Splash Panel Tooltip");
```

**VB.NET Example:**

```vb
' Add a ToolTip component to the form
Friend WithEvents toolTip1 As System.Windows.Forms.ToolTip

' In the constructor or initialization code
Me.toolTip1 = New System.Windows.Forms.ToolTip()
Me.toolTip1.SetToolTip(Me.splashPanel1, "Splash Panel Tooltip")
```

## Next Steps

- **[Getting Started](./getting-started.md)** - Learn the basics of implementing SplashPanel
- **[Display Methods](./display-methods.md)** - Explore different ways to show and hide splash panels
- **[Slide Transitions](./slide-transitions.md)** - Configure advanced slide and marquee animations
- **[Events](./events.md)** - Handle splash panel lifecycle events
