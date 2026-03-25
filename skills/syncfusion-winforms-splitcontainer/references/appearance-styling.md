# Appearance and Styling in SplitContainerAdv

## Background Color Settings

Customize the background appearance of the SplitContainerAdv and its panels.

### Container Background

Set the background color of the entire SplitContainerAdv:

```csharp
this.splitContainerAdv1.BackColor = System.Drawing.Color.LightSteelBlue;
```

```vb
Me.splitContainerAdv1.BackColor = System.Drawing.Color.LightSteelBlue
```

### Panel-Specific Backgrounds

Each panel can have its own background color that overrides the container's background:

```csharp
// Panel1 background with gradient
this.splitContainerAdv1.Panel1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.BackwardDiagonal, 
    System.Drawing.Color.AliceBlue, 
    System.Drawing.Color.LightSteelBlue);

// Panel2 solid background
this.splitContainerAdv1.Panel2.BackColor = System.Drawing.Color.AliceBlue;
```

```vb
' Panel1 background with gradient
Me.splitContainerAdv1.Panel1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.BackwardDiagonal,
    System.Drawing.Color.AliceBlue,
    System.Drawing.Color.LightSteelBlue)

' Panel2 solid background
Me.splitContainerAdv1.Panel2.BackColor = System.Drawing.Color.AliceBlue
```

### Gradient Backgrounds

Create gradient effects in panels using different gradient styles:

```csharp
// Horizontal gradient
this.splitContainerAdv1.Panel1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Horizontal, 
    System.Drawing.Color.White, 
    System.Drawing.Color.LightBlue);

// Vertical gradient
this.splitContainerAdv1.Panel2.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Vertical, 
    System.Drawing.Color.LightGray, 
    System.Drawing.Color.DarkGray);

// Forward diagonal gradient
this.splitContainerAdv1.Panel1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.ForwardDiagonal, 
    System.Drawing.Color.Yellow, 
    System.Drawing.Color.Orange);
```

```vb
' Horizontal gradient
Me.splitContainerAdv1.Panel1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Horizontal,
    System.Drawing.Color.White,
    System.Drawing.Color.LightBlue)

' Vertical gradient
Me.splitContainerAdv1.Panel2.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Vertical,
    System.Drawing.Color.LightGray,
    System.Drawing.Color.DarkGray)

' Forward diagonal gradient
Me.splitContainerAdv1.Panel1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.ForwardDiagonal,
    System.Drawing.Color.Yellow,
    System.Drawing.Color.Orange)
```

## Foreground Properties

Configure text and content appearance within panels.

### Font Configuration

Set the font for text displayed in panels:

```csharp
// Panel1 with bold Arial font
this.splitContainerAdv1.Panel1.Font = new System.Drawing.Font("Arial", 12, System.Drawing.FontStyle.Bold);

// Panel2 with regular Courier font
this.splitContainerAdv1.Panel2.Font = new System.Drawing.Font("Courier New", 10, System.Drawing.FontStyle.Regular);
```

```vb
' Panel1 with bold Arial font
Me.splitContainerAdv1.Panel1.Font = New System.Drawing.Font("Arial", 12, System.Drawing.FontStyle.Bold)

' Panel2 with regular Courier font
Me.splitContainerAdv1.Panel2.Font = New System.Drawing.Font("Courier New", 10, System.Drawing.FontStyle.Regular)
```

### Foreground Color

Set the text/foreground color for panels:

```csharp
// Panel1 foreground color
this.splitContainerAdv1.Panel1.ForeColor = System.Drawing.Color.Black;

// Panel2 foreground color
this.splitContainerAdv1.Panel2.ForeColor = System.Drawing.Color.DarkBlue;
```

```vb
' Panel1 foreground color
Me.splitContainerAdv1.Panel1.ForeColor = System.Drawing.Color.Black

' Panel2 foreground color
Me.splitContainerAdv1.Panel2.ForeColor = System.Drawing.Color.DarkBlue
```

## Border Styling

Customize the border appearance of the SplitContainerAdv.

### Border Style Options

Set the border style using the `BorderStyle` property:

```csharp
// No border
this.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.None;

// Single line border (2D)
this.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// 3D border effect
this.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
```

```vb
' No border
Me.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.None

' Single line border (2D)
Me.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle

' 3D border effect
Me.splitContainerAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
```

### Border Effects

- **None**: No visible border
- **FixedSingle**: Simple single-line border
- **Fixed3D**: Raised or sunken 3D effect

## Complete Styling Example

```csharp
// Comprehensive appearance configuration
private void StyleSplitContainer()
{
    SplitContainerAdv splitContainer = new SplitContainerAdv();
    
    // Container background
    splitContainer.BackColor = System.Drawing.Color.DarkGray;
    splitContainer.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
    
    // Panel1 styling
    splitContainer.Panel1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(
        Syncfusion.Drawing.GradientStyle.Vertical,
        System.Drawing.Color.LightBlue,
        System.Drawing.Color.SkyBlue);
    splitContainer.Panel1.Font = new System.Drawing.Font("Arial", 11, System.Drawing.FontStyle.Bold);
    splitContainer.Panel1.ForeColor = System.Drawing.Color.Navy;
    
    // Panel2 styling
    splitContainer.Panel2.BackColor = System.Drawing.Color.White;
    splitContainer.Panel2.Font = new System.Drawing.Font("Segoe UI", 10, System.Drawing.FontStyle.Regular);
    splitContainer.Panel2.ForeColor = System.Drawing.Color.Black;
    
    // Splitter appearance
    splitContainer.ExpandFill = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.LightGray);
    splitContainer.GripDark = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Gray);
    splitContainer.GripLight = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.White);
    
    splitContainer.Dock = System.Windows.Forms.DockStyle.Fill;
    this.Controls.Add(splitContainer);
}
```

```vb
' Comprehensive appearance configuration
Private Sub StyleSplitContainer()
    Dim splitContainer As New SplitContainerAdv()
    
    ' Container background
    splitContainer.BackColor = System.Drawing.Color.DarkGray
    splitContainer.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
    
    ' Panel1 styling
    splitContainer.Panel1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(
        Syncfusion.Drawing.GradientStyle.Vertical,
        System.Drawing.Color.LightBlue,
        System.Drawing.Color.SkyBlue)
    splitContainer.Panel1.Font = New System.Drawing.Font("Arial", 11, System.Drawing.FontStyle.Bold)
    splitContainer.Panel1.ForeColor = System.Drawing.Color.Navy
    
    ' Panel2 styling
    splitContainer.Panel2.BackColor = System.Drawing.Color.White
    splitContainer.Panel2.Font = New System.Drawing.Font("Segoe UI", 10, System.Drawing.FontStyle.Regular)
    splitContainer.Panel2.ForeColor = System.Drawing.Color.Black
    
    ' Splitter appearance
    splitContainer.ExpandFill = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.LightGray)
    splitContainer.GripDark = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Gray)
    splitContainer.GripLight = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.White)
    
    splitContainer.Dock = System.Windows.Forms.DockStyle.Fill
    Me.Controls.Add(splitContainer)
End Sub
```

## Best Practices for Styling

- **Contrast**: Ensure sufficient color contrast between text and background for readability
- **Consistency**: Use consistent fonts and colors across related panels
- **3D Border**: Use Fixed3D for professional appearance with depth
- **Gradient Direction**: Choose gradient direction that complements your layout
- **Accessibility**: Avoid color-only differentiation; use fonts or patterns for important distinctions
