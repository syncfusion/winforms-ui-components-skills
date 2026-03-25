# Appearance and Styling in ProgressBarAdv

This reference covers comprehensive appearance customization options including foreground styles, background styles, colors, gradients, segments, images, and Office 2016 themes.

## Table of Contents

- [Overview](#overview)
- [When to Read This](#when-to-read-this)
- [Foreground Settings](#foreground-settings)
  - [Foreground Segments](#foreground-segments)
  - [Segment Width Configuration](#segment-width-configuration)
  - [Foreground Colors](#foreground-colors)
  - [Gradient Colors](#gradient-colors)
  - [Multiple Gradient Colors](#multiple-gradient-colors)
  - [Tube Colors](#tube-colors)
  - [Foreground Images](#foreground-images)
- [Progress Styles](#progress-styles)
  - [Constant Style](#constant-style)
  - [Gradient Style](#gradient-style)
  - [Multiple Gradient Style](#multiple-gradient-style)
  - [Tube Style](#tube-style)
  - [Image Style](#image-style)
  - [System Style](#system-style)
  - [Waiting Gradient Style](#waiting-gradient-style)
  - [Metro Style](#metro-style)
  - [Office 2016 Styles](#office-2016-styles)
- [Background Settings](#background-settings)
  - [Background Styles](#background-styles)
  - [Background Segments](#background-segments)
  - [Background Gradient Colors](#background-gradient-colors)
  - [Background Multiple Colors](#background-multiple-colors)
  - [Background Tube Colors](#background-tube-colors)
- [Border Settings](#border-settings)
  - [3D Border Styles](#3d-border-styles)
  - [2D Border Styles](#2d-border-styles)
  - [Border Colors](#border-colors)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

ProgressBarAdv provides extensive styling capabilities allowing you to create visually appealing progress indicators that match your application's design. You can customize foreground, background, borders, apply gradients, use images, and implement modern Office 2016 themes.

**Key Styling Properties:**
- `ProgressStyle` - Defines foreground appearance
- `BackgroundStyle` - Defines background appearance
- `ForeColor` - Foreground color
- `BackColor` - Background color
- `BorderStyle` - Border appearance
- `ThemesEnabled` - Enable theme support

## When to Read This

Read this reference when:
- Customizing progress bar appearance
- Implementing gradient or tube effects
- Using images for progress display
- Applying Office 2016 themes
- Creating branded progress indicators
- Troubleshooting appearance issues

## Foreground Settings

The foreground represents the filled portion of the progress bar showing actual progress.

### Foreground Segments

Display progress with a segmented appearance.

**C#:**
```csharp
// Enable segmented foreground
progressBarAdv1.ForeSegments = true;
progressBarAdv1.SegmentWidth = 10;
progressBarAdv1.ForeColor = System.Drawing.Color.Green;
progressBarAdv1.BackColor = System.Drawing.Color.LightGray;

// Disable segmented foreground (continuous bar)
progressBarAdv2.ForeSegments = false;
progressBarAdv2.ForeColor = System.Drawing.Color.Blue;
```

**VB.NET:**
```vbnet
' Enable segmented foreground
progressBarAdv1.ForeSegments = True
progressBarAdv1.SegmentWidth = 10
progressBarAdv1.ForeColor = System.Drawing.Color.Green
progressBarAdv1.BackColor = System.Drawing.Color.LightGray

' Disable segmented foreground (continuous bar)
progressBarAdv2.ForeSegments = False
progressBarAdv2.ForeColor = System.Drawing.Color.Blue
```

### Segment Width Configuration

Control the width of individual segments.

**C#:**
```csharp
// Small segments
progressBarAdv1.ForeSegments = true;
progressBarAdv1.SegmentWidth = 5;

// Medium segments
progressBarAdv2.ForeSegments = true;
progressBarAdv2.SegmentWidth = 15;

// Large segments
progressBarAdv3.ForeSegments = true;
progressBarAdv3.SegmentWidth = 30;

// Dynamic segment width based on progress bar size
private void SetSegmentWidth()
{
    int progressBarWidth = progressBarAdv1.Width;
    progressBarAdv1.SegmentWidth = progressBarWidth / 20; // 20 segments across
}
```

**VB.NET:**
```vbnet
' Small segments
progressBarAdv1.ForeSegments = True
progressBarAdv1.SegmentWidth = 5

' Medium segments
progressBarAdv2.ForeSegments = True
progressBarAdv2.SegmentWidth = 15

' Large segments
progressBarAdv3.ForeSegments = True
progressBarAdv3.SegmentWidth = 30

' Dynamic segment width based on progress bar size
Private Sub SetSegmentWidth()
    Dim progressBarWidth As Integer = progressBarAdv1.Width
    progressBarAdv1.SegmentWidth = progressBarWidth \ 20 ' 20 segments across
End Sub
```

### Foreground Colors

Set solid foreground and font colors.

**C#:**
```csharp
// Set foreground color (progress fill color)
progressBarAdv1.ForeColor = System.Drawing.Color.Turquoise;
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Constant;

// Set font color (text color)
progressBarAdv1.FontColor = System.Drawing.Color.SteelBlue;
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Percentage;

// Coordinated color scheme
progressBarAdv1.ForeColor = System.Drawing.Color.RoyalBlue;
progressBarAdv1.FontColor = System.Drawing.Color.White;
progressBarAdv1.BackColor = System.Drawing.Color.AliceBlue;
```

**VB.NET:**
```vbnet
' Set foreground color (progress fill color)
progressBarAdv1.ForeColor = System.Drawing.Color.Turquoise
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Constant

' Set font color (text color)
progressBarAdv1.FontColor = System.Drawing.Color.SteelBlue
progressBarAdv1.TextVisible = True
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Percentage

' Coordinated color scheme
progressBarAdv1.ForeColor = System.Drawing.Color.RoyalBlue
progressBarAdv1.FontColor = System.Drawing.Color.White
progressBarAdv1.BackColor = System.Drawing.Color.AliceBlue
```

### Gradient Colors

Create smooth color transitions in the foreground.

**C#:**
```csharp
// Simple two-color gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient;
progressBarAdv1.GradientStartColor = System.Drawing.Color.OrangeRed;
progressBarAdv1.GradientEndColor = System.Drawing.Color.Yellow;

// Blue gradient
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient;
progressBarAdv2.GradientStartColor = System.Drawing.Color.DarkBlue;
progressBarAdv2.GradientEndColor = System.Drawing.Color.LightSkyBlue;

// Green success gradient
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient;
progressBarAdv3.GradientStartColor = System.Drawing.Color.LimeGreen;
progressBarAdv3.GradientEndColor = System.Drawing.Color.PaleGreen;
```

**VB.NET:**
```vbnet
' Simple two-color gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient
progressBarAdv1.GradientStartColor = System.Drawing.Color.OrangeRed
progressBarAdv1.GradientEndColor = System.Drawing.Color.Yellow

' Blue gradient
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient
progressBarAdv2.GradientStartColor = System.Drawing.Color.DarkBlue
progressBarAdv2.GradientEndColor = System.Drawing.Color.LightSkyBlue

' Green success gradient
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient
progressBarAdv3.GradientStartColor = System.Drawing.Color.LimeGreen
progressBarAdv3.GradientEndColor = System.Drawing.Color.PaleGreen
```

### Multiple Gradient Colors

Apply multiple colors for rich gradient effects.

**C#:**
```csharp
// Rainbow gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient;
progressBarAdv1.MultipleColors = new System.Drawing.Color[] 
{
    System.Drawing.Color.Red,
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow,
    System.Drawing.Color.Green,
    System.Drawing.Color.Blue,
    System.Drawing.Color.Indigo,
    System.Drawing.Color.Violet
};
progressBarAdv1.StretchMultGrad = true;

// Fire gradient (warm colors)
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient;
progressBarAdv2.MultipleColors = new System.Drawing.Color[]
{
    System.Drawing.Color.DarkRed,
    System.Drawing.Color.OrangeRed,
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow
};
progressBarAdv2.StretchMultGrad = false;

// Ocean gradient (cool colors)
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient;
progressBarAdv3.MultipleColors = new System.Drawing.Color[]
{
    System.Drawing.Color.DarkBlue,
    System.Drawing.Color.RoyalBlue,
    System.Drawing.Color.DeepSkyBlue,
    System.Drawing.Color.LightCyan
};
progressBarAdv3.StretchMultGrad = true;
```

**VB.NET:**
```vbnet
' Rainbow gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient
progressBarAdv1.MultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.Red,
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow,
    System.Drawing.Color.Green,
    System.Drawing.Color.Blue,
    System.Drawing.Color.Indigo,
    System.Drawing.Color.Violet
}
progressBarAdv1.StretchMultGrad = True

' Fire gradient (warm colors)
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient
progressBarAdv2.MultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.DarkRed,
    System.Drawing.Color.OrangeRed,
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow
}
progressBarAdv2.StretchMultGrad = False

' Ocean gradient (cool colors)
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient
progressBarAdv3.MultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.DarkBlue,
    System.Drawing.Color.RoyalBlue,
    System.Drawing.Color.DeepSkyBlue,
    System.Drawing.Color.LightCyan
}
progressBarAdv3.StretchMultGrad = True
```

### Tube Colors

Create 3D tube-style progress bars.

**C#:**
```csharp
// Red tube style
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube;
progressBarAdv1.TubeStartColor = System.Drawing.Color.Red;
progressBarAdv1.TubeEndColor = System.Drawing.Color.Black;

// Green tube style
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube;
progressBarAdv2.TubeStartColor = System.Drawing.Color.LimeGreen;
progressBarAdv2.TubeEndColor = System.Drawing.Color.DarkGreen;

// Blue metallic tube
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube;
progressBarAdv3.TubeStartColor = System.Drawing.Color.LightSteelBlue;
progressBarAdv3.TubeEndColor = System.Drawing.Color.MidnightBlue;
```

**VB.NET:**
```vbnet
' Red tube style
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube
progressBarAdv1.TubeStartColor = System.Drawing.Color.Red
progressBarAdv1.TubeEndColor = System.Drawing.Color.Black

' Green tube style
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube
progressBarAdv2.TubeStartColor = System.Drawing.Color.LimeGreen
progressBarAdv2.TubeEndColor = System.Drawing.Color.DarkGreen

' Blue metallic tube
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube
progressBarAdv3.TubeStartColor = System.Drawing.Color.LightSteelBlue
progressBarAdv3.TubeEndColor = System.Drawing.Color.MidnightBlue
```

### Foreground Images

Use custom images for progress display.

**C#:**
```csharp
// Set foreground image
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image;
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image;
progressBarAdv1.ForegroundImage = Image.FromFile("progress_texture.png");
progressBarAdv1.StretchImage = true;

// Using resources
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image;
progressBarAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image;
progressBarAdv2.ForegroundImage = Properties.Resources.ProgressTexture;
progressBarAdv2.StretchImage = true;

// Pattern image without stretching
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image;
progressBarAdv3.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image;
progressBarAdv3.ForegroundImage = Properties.Resources.TilePattern;
progressBarAdv3.StretchImage = false;
```

**VB.NET:**
```vbnet
' Set foreground image
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image
progressBarAdv1.ForegroundImage = Image.FromFile("progress_texture.png")
progressBarAdv1.StretchImage = True

' Using resources
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image
progressBarAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image
progressBarAdv2.ForegroundImage = My.Resources.ProgressTexture
progressBarAdv2.StretchImage = True

' Pattern image without stretching
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image
progressBarAdv3.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image
progressBarAdv3.ForegroundImage = My.Resources.TilePattern
progressBarAdv3.StretchImage = False
```

## Progress Styles

### Constant Style

Solid color progress bar.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Constant;
progressBarAdv1.ForeColor = System.Drawing.Color.Green;
progressBarAdv1.BackColor = System.Drawing.Color.LightGray;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Constant
progressBarAdv1.ForeColor = System.Drawing.Color.Green
progressBarAdv1.BackColor = System.Drawing.Color.LightGray
```

### Gradient Style

Two-color gradient progress bar.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient;
progressBarAdv1.GradientStartColor = System.Drawing.Color.Blue;
progressBarAdv1.GradientEndColor = System.Drawing.Color.LightBlue;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Gradient
progressBarAdv1.GradientStartColor = System.Drawing.Color.Blue
progressBarAdv1.GradientEndColor = System.Drawing.Color.LightBlue
```

### Multiple Gradient Style

Multi-color gradient progress bar.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient;
progressBarAdv1.MultipleColors = new System.Drawing.Color[]
{
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow,
    System.Drawing.Color.Blue,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Green
};
progressBarAdv1.StretchMultGrad = false;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient
progressBarAdv1.MultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.Orange,
    System.Drawing.Color.Yellow,
    System.Drawing.Color.Blue,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Green
}
progressBarAdv1.StretchMultGrad = False
```

### Tube Style

3D tube appearance.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube;
progressBarAdv1.TubeStartColor = System.Drawing.Color.Red;
progressBarAdv1.TubeEndColor = System.Drawing.Color.Black;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Tube
progressBarAdv1.TubeStartColor = System.Drawing.Color.Red
progressBarAdv1.TubeEndColor = System.Drawing.Color.Black
```

### Image Style

Custom image foreground.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image;
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image;
progressBarAdv1.ForegroundImage = Properties.Resources.ProgressImage;
progressBarAdv1.StretchImage = true;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Image
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Image
progressBarAdv1.ForegroundImage = My.Resources.ProgressImage
progressBarAdv1.StretchImage = True
```

### System Style

Uses operating system theme.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.System;
progressBarAdv1.ProgressFallbackStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.System
progressBarAdv1.ProgressFallbackStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.MultipleGradient
```

### Waiting Gradient Style

Animated gradient for indeterminate progress.

**C#:**
```csharp
// Standard waiting gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.WaitingGradient;
progressBarAdv1.WaitingGradientEnabled = true;
progressBarAdv1.WaitingGradientInterval = 20;
progressBarAdv1.WaitingGradientWidth = 500;

// Custom waiting render with segments
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.WaitingGradient;
progressBarAdv2.WaitingGradientEnabled = true;
progressBarAdv2.CustomWaitingRender = true;
progressBarAdv2.ForeColor = System.Drawing.Color.Crimson;
progressBarAdv2.WaitingGradientInterval = 20;
progressBarAdv2.WaitingGradientWidth = 500;
```

**VB.NET:**
```vbnet
' Standard waiting gradient
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.WaitingGradient
progressBarAdv1.WaitingGradientEnabled = True
progressBarAdv1.WaitingGradientInterval = 20
progressBarAdv1.WaitingGradientWidth = 500

' Custom waiting render with segments
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.WaitingGradient
progressBarAdv2.WaitingGradientEnabled = True
progressBarAdv2.CustomWaitingRender = True
progressBarAdv2.ForeColor = System.Drawing.Color.Crimson
progressBarAdv2.WaitingGradientInterval = 20
progressBarAdv2.WaitingGradientWidth = 500
```

### Metro Style

Modern flat design style.

**C#:**
```csharp
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Metro;
progressBarAdv1.BackColor = System.Drawing.Color.WhiteSmoke;
progressBarAdv1.ForeColor = System.Drawing.Color.DeepSkyBlue;
```

**VB.NET:**
```vbnet
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Metro
progressBarAdv1.BackColor = System.Drawing.Color.WhiteSmoke
progressBarAdv1.ForeColor = System.Drawing.Color.DeepSkyBlue
```

### Office 2016 Styles

Modern Office-inspired themes.

**C#:**
```csharp
// Office2016Colorful
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016Colorful;

// Office2016White
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016White;

// Office2016DarkGray
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016DarkGray;

// Office2016Black
progressBarAdv4.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016Black;
```

**VB.NET:**
```vbnet
' Office2016Colorful
progressBarAdv1.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016Colorful

' Office2016White
progressBarAdv2.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016White

' Office2016DarkGray
progressBarAdv3.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016DarkGray

' Office2016Black
progressBarAdv4.ProgressStyle = Syncfusion.Windows.Forms.Tools.ProgressBarStyles.Office2016Black
```

## Background Settings

### Background Styles

**C#:**
```csharp
// Gradient background
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Gradient;
progressBarAdv1.BackGradientStartColor = System.Drawing.Color.IndianRed;
progressBarAdv1.BackGradientEndColor = System.Drawing.Color.Aquamarine;

// Vertical gradient background
progressBarAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.VerticalGradient;
progressBarAdv2.BackGradientStartColor = System.Drawing.Color.LightBlue;
progressBarAdv2.BackGradientEndColor = System.Drawing.Color.DarkBlue;

// Tube background
progressBarAdv3.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube;
progressBarAdv3.BackTubeStartColor = System.Drawing.Color.Yellow;
progressBarAdv3.BackTubeEndColor = System.Drawing.Color.RosyBrown;

// Multiple gradient background
progressBarAdv4.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.MultipleGradient;
progressBarAdv4.BackMultipleColors = new System.Drawing.Color[]
{
    System.Drawing.Color.Blue,
    System.Drawing.Color.Red,
    System.Drawing.Color.Green,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Yellow
};

// Office 2016 backgrounds
progressBarAdv5.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016Colorful;
progressBarAdv6.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016White;
progressBarAdv7.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016DarkGray;
progressBarAdv8.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016Black;
```

**VB.NET:**
```vbnet
' Gradient background
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Gradient
progressBarAdv1.BackGradientStartColor = System.Drawing.Color.IndianRed
progressBarAdv1.BackGradientEndColor = System.Drawing.Color.Aquamarine

' Vertical gradient background
progressBarAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.VerticalGradient
progressBarAdv2.BackGradientStartColor = System.Drawing.Color.LightBlue
progressBarAdv2.BackGradientEndColor = System.Drawing.Color.DarkBlue

' Tube background
progressBarAdv3.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube
progressBarAdv3.BackTubeStartColor = System.Drawing.Color.Yellow
progressBarAdv3.BackTubeEndColor = System.Drawing.Color.RosyBrown

' Multiple gradient background
progressBarAdv4.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.MultipleGradient
progressBarAdv4.BackMultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.Blue,
    System.Drawing.Color.Red,
    System.Drawing.Color.Green,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Yellow
}

' Office 2016 backgrounds
progressBarAdv5.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016Colorful
progressBarAdv6.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016White
progressBarAdv7.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016DarkGray
progressBarAdv8.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Office2016Black
```

### Background Segments

**C#:**
```csharp
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube;
progressBarAdv1.BackSegments = true;
progressBarAdv1.BackTubeStartColor = System.Drawing.Color.LightGray;
progressBarAdv1.BackTubeEndColor = System.Drawing.Color.DarkGray;
```

**VB.NET:**
```vbnet
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube
progressBarAdv1.BackSegments = True
progressBarAdv1.BackTubeStartColor = System.Drawing.Color.LightGray
progressBarAdv1.BackTubeEndColor = System.Drawing.Color.DarkGray
```

### Background Gradient Colors

**C#:**
```csharp
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Gradient;
progressBarAdv1.BackGradientStartColor = System.Drawing.Color.IndianRed;
progressBarAdv1.BackGradientEndColor = System.Drawing.Color.Aquamarine;
```

**VB.NET:**
```vbnet
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Gradient
progressBarAdv1.BackGradientStartColor = System.Drawing.Color.IndianRed
progressBarAdv1.BackGradientEndColor = System.Drawing.Color.Aquamarine
```

### Background Multiple Colors

**C#:**
```csharp
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.MultipleGradient;
progressBarAdv1.BackMultipleColors = new System.Drawing.Color[]
{
    System.Drawing.Color.Blue,
    System.Drawing.Color.Red,
    System.Drawing.Color.Green,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Yellow
};
```

**VB.NET:**
```vbnet
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.MultipleGradient
progressBarAdv1.BackMultipleColors = New System.Drawing.Color() _
{
    System.Drawing.Color.Blue,
    System.Drawing.Color.Red,
    System.Drawing.Color.Green,
    System.Drawing.Color.Pink,
    System.Drawing.Color.Yellow
}
```

### Background Tube Colors

**C#:**
```csharp
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube;
progressBarAdv1.BackTubeStartColor = System.Drawing.Color.Yellow;
progressBarAdv1.BackTubeEndColor = System.Drawing.Color.RosyBrown;
```

**VB.NET:**
```vbnet
progressBarAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.ProgressBarBackgroundStyles.Tube
progressBarAdv1.BackTubeStartColor = System.Drawing.Color.Yellow
progressBarAdv1.BackTubeEndColor = System.Drawing.Color.RosyBrown
```

## Border Settings

### 3D Border Styles

**C#:**
```csharp
// Raised outer border
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
progressBarAdv1.Border3DStyle = System.Windows.Forms.Border3DStyle.RaisedOuter;

// Sunken border
progressBarAdv2.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
progressBarAdv2.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken;

// Etched border
progressBarAdv3.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
progressBarAdv3.Border3DStyle = System.Windows.Forms.Border3DStyle.Etched;
```

**VB.NET:**
```vbnet
' Raised outer border
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
progressBarAdv1.Border3DStyle = System.Windows.Forms.Border3DStyle.RaisedOuter

' Sunken border
progressBarAdv2.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
progressBarAdv2.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken

' Etched border
progressBarAdv3.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
progressBarAdv3.Border3DStyle = System.Windows.Forms.Border3DStyle.Etched
```

### 2D Border Styles

**C#:**
```csharp
// Solid single border
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
progressBarAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid;
progressBarAdv1.BorderColor = System.Drawing.Color.Black;

// Dashed border
progressBarAdv2.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
progressBarAdv2.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dashed;
progressBarAdv2.BorderColor = System.Drawing.Color.DarkBlue;

// Dotted border
progressBarAdv3.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
progressBarAdv3.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dotted;
progressBarAdv3.BorderColor = System.Drawing.Color.Red;
```

**VB.NET:**
```vbnet
' Solid single border
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
progressBarAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid
progressBarAdv1.BorderColor = System.Drawing.Color.Black

' Dashed border
progressBarAdv2.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
progressBarAdv2.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dashed
progressBarAdv2.BorderColor = System.Drawing.Color.DarkBlue

' Dotted border
progressBarAdv3.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
progressBarAdv3.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dotted
progressBarAdv3.BorderColor = System.Drawing.Color.Red
```

### Border Colors

**C#:**
```csharp
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
progressBarAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid;
progressBarAdv1.BorderColor = System.Drawing.Color.Black;
```

**VB.NET:**
```vbnet
progressBarAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
progressBarAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid
progressBarAdv1.BorderColor = System.Drawing.Color.Black
```

## Use Cases

### Use Case 1: Download Progress with Blue Gradient
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
progressBarAdv1.GradientStartColor = Color.DarkBlue;
progressBarAdv1.GradientEndColor = Color.LightSkyBlue;
progressBarAdv1.BackColor = Color.LightGray;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
```

### Use Case 2: Installation with Rainbow Gradient
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.MultipleGradient;
progressBarAdv1.MultipleColors = new Color[] 
{ 
    Color.Red, Color.Orange, Color.Yellow, 
    Color.Green, Color.Blue, Color.Purple 
};
progressBarAdv1.StretchMultGrad = true;
```

### Use Case 3: Modern Flat Design
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Metro;
progressBarAdv1.ForeColor = Color.DeepSkyBlue;
progressBarAdv1.BackColor = Color.WhiteSmoke;
progressBarAdv1.BorderStyle = BorderStyle.None;
```

## Best Practices

1. **Match Application Theme**
   - Use Office 2016 styles for Office-like applications
   - Use Metro style for modern flat designs
   - Use gradients for visually appealing interfaces

2. **Ensure Readability**
   - Choose contrasting foreground/background colors
   - Test text visibility on all progress styles
   - Consider color-blind friendly palettes

3. **Performance Considerations**
   - Avoid complex images for frequently updated progress bars
   - Use simple styles for high-performance scenarios
   - Cache gradient colors when possible

4. **Consistency**
   - Use consistent styles across your application
   - Match progress bar style with overall UI theme
   - Coordinate colors with brand guidelines

## Common Scenarios

### Scenario 1: Simple Green Progress Bar
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Constant;
progressBarAdv1.ForeColor = Color.Green;
progressBarAdv1.BackColor = Color.LightGray;
```

### Scenario 2: Gradient File Transfer
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
progressBarAdv1.GradientStartColor = Color.LimeGreen;
progressBarAdv1.GradientEndColor = Color.DarkGreen;
```

### Scenario 3: Office 2016 Themed Application
```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Office2016Colorful;
```

## Troubleshooting

### Issue: Colors Not Displaying

**Solutions:**
1. Verify `ProgressStyle` matches color properties used
2. Check `ThemesEnabled` property
3. Ensure colors are set after style selection
4. Verify gradient requires start and end colors

### Issue: Image Not Showing

**Solutions:**
1. Set both `ProgressStyle` and `BackgroundStyle` to Image
2. Verify image resource exists
3. Check `StretchImage` property
4. Ensure image format is supported

### Issue: Office 2016 Styles Look Wrong

**Solutions:**
1. Match `ProgressStyle` with `BackgroundStyle`
2. Don't override theme colors manually
3. Ensure Syncfusion assemblies are updated
4. Check theme is supported in your version

### Issue: Segments Not Visible

**Solutions:**
1. Set `ForeSegments = true`
2. Adjust `SegmentWidth` property
3. Ensure sufficient progress bar width
4. Check segment colors have contrast

## Related Topics

- **[text-display.md](text-display.md)** - Text styling and colors
- **[orientation-layout.md](orientation-layout.md)** - Layout considerations
- **[themes.md](themes.md)** - Theme configuration
- **[events-advanced.md](events-advanced.md)** - Custom rendering events
